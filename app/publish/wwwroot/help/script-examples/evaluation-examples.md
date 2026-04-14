# Schedule Evaluation Script Examples

Examples of evaluation scripts used to assess the quality of a generated schedule.

## Type 1 — Overall Schedule Evaluator

```csharp
Func<IFacultySeason, int>
```

Type 1 scripts run *after* schedule variants are found and do not affect the generation process itself. They are used to compare variants and select the best ones.

---

### Example 1 — Penalising Gaps in Student Timetables

Penalises free periods between lessons for students.

```csharp
facultySeason =>
{
    int score = 0;

    var studentSchedules = facultySeason.GroupSubjects
        .SelectMany(gs => gs.Groups, (gs, group) => new { group.Id, group.SubgroupId, group.SubgroupVariantId, gs.ScheduleSlots })
        .GroupBy(x => new { x.Id, x.SubgroupId, x.SubgroupVariantId })
        .ToDictionary(
            g => g.Key,
            g => g.SelectMany(x => x.ScheduleSlots)
                  .GroupBy(s => s.Date.Date)
                  .ToDictionary(
                      d => d.Key,
                      d => d.Select(s => s.PairNumber).OrderBy(p => p).ToList()
                  )
        );

    foreach (var student in studentSchedules)
    {
        foreach (var daySchedule in student.Value)
        {
            var pairs = daySchedule.Value;
            for (int i = 0; i < pairs.Count - 1; i++)
            {
                int gap = pairs[i + 1] - pairs[i];
                if (gap == 2)
                    score -= 30;   // One-period gap
                else if (gap > 2)
                    score -= 100;  // Two or more period gap
            }
        }
    }

    return score;
}
```

**Logic:**
1. For each student (unique group + subgroup + variant combination) collect lessons grouped by date
2. Check consecutive period numbers — a gap of 2 means one free period; gap > 2 means multiple free periods
3. Apply penalties accordingly — the fewer gaps, the higher (less negative) the score

---

### Example 2 — Rewarding Free Days for Teachers

Rewards schedules where teachers have more complete free days.

```csharp
facultySeason =>
{
    int score = 0;

    var teachers = facultySeason.GroupSubjects
        .Select(gs => gs.Teacher)
        .Where(t => t != null)
        .DistinctBy(t => t.Id)
        .ToList();

    var scheduleByTeacher = facultySeason.GroupSubjects
        .Where(gs => gs.Teacher != null)
        .SelectMany(gs => gs.ScheduleSlots, (gs, slot) => new
        {
            TeacherId = gs.Teacher.Id,
            Date = slot.Date.Date
        })
        .GroupBy(x => x.TeacherId)
        .ToDictionary(g => g.Key, g => g.Select(x => x.Date).Distinct().ToHashSet());

    var allDates = Enumerable.Range(0, (facultySeason.EndSeason - facultySeason.BeginSeason).Days + 1)
        .Select(d => facultySeason.BeginSeason.AddDays(d).Date)
        .ToList();

    foreach (var teacher in teachers)
    {
        if (!scheduleByTeacher.TryGetValue(teacher.Id, out var scheduledDates))
            scheduledDates = new HashSet<DateTime>();

        int freeDaysCount = allDates.Count(date => !scheduledDates.Contains(date));
        score += freeDaysCount * 100;
    }

    return score;
}
```

**Logic:**
1. Collect all unique teachers
2. Find the days on which each teacher has lessons
3. Generate all calendar days in the season
4. Count free days per teacher → add 100 points per free day

---

## Combining Multiple Scripts

Multiple type 1 scripts are active simultaneously and their scores are summed:

1. Student gap script → −500
2. Teacher free-day script → +1200
3. Combined score → **+700**

This allows modular evaluation where each script covers one quality aspect independently.

---

## Type 2 — Day/Period Estimator

```csharp
Func<IScheduleSlot, IAssignedSlots, int>
```

Used *during* schedule generation to choose the best time slot for each lesson.

### Example 1 — Student Schedule Compactness

Rewards adjacent periods and penalises overloading student groups.

```csharp
(slot, assignedSlots) =>
{
    int score = 0;

    foreach (var candidateGroup in slot.GroupSubject.Groups)
    {
        var groupAdjacentSlots = assignedSlots
            .GetSlotsByGroupAndDate(candidateGroup.Id, slot.Date.Date)
            .ToList();

        foreach (var pair in groupAdjacentSlots)
        {
            if (Math.Abs(pair.PairNumber - slot.PairNumber) == 1)
                score += 3;   // Adjacent period bonus

            if (pair.PairNumber == slot.PairNumber)
            {
                score += 5;   // Same slot for different subgroups

                if (pair.LessonSeriesLength == slot.LessonSeriesLength)
                    score += 12;

                if (pair.GroupSubject.Subject.Id == slot.GroupSubject.Subject.Id)
                    score += 10;
            }
        }

        // Overload penalty
        var variants = groupAdjacentSlots
            .Union([slot])
            .SelectMany(x => x.GroupSubject.Groups.Where(g => g.Id == candidateGroup.Id))
            .Select(x => new { variant = x.SubgroupVariantId ?? 0, subgroup = x.SubgroupId ?? 0 });

        var maxInVariants = variants
            .GroupBy(v => v.variant)
            .Select(g => new
            {
                Variant = g.Key,
                MaxCount = g.GroupBy(x => x.subgroup).Max(sg => sg.Count())
            })
            .ToList();

        if (maxInVariants.Sum(x => x.MaxCount) > 3)
            score -= 1000;
    }

    return score;
}
```

**Logic:**
- Bonuses for adjacent periods, aligned subgroup slots, matching series lengths, and same subject for different subgroups
- Heavy penalty if a student ends up with more than 3 lessons on the same day

### Example 2 — Teacher Schedule Compactness

Prefers placing a new lesson adjacent to existing lessons for the same teacher.

```csharp
(slot, assignedSlots) =>
{
    int score = 0;
    var teacherId = slot.GroupSubject.Teacher.Id;

    var teacherSlots = assignedSlots
        .GetSlotsByTeacherAndDate(teacherId, slot.Date.Date)
        .Select(s => s.PairNumber)
        .ToList();

    foreach (var pair in teacherSlots)
    {
        if (Math.Abs(pair - slot.PairNumber) == 1)
            score += 1;
    }

    return score;
}
```

---

## Type 4 — Lesson Priority Estimator

```csharp
Func<ISlotEstimation, int>
```

Determines which lesson to schedule next. Higher = scheduled sooner.

### Example — Prioritise Constrained and Important Lessons

> **Note:** `AvailableDomains` is the number of valid date+period combinations available for a slot after constraints are applied.

```csharp
(slot) => {
    var res = slot.AvailableDomains * -10   // Fewer options → higher priority
            + slot.LessonSeriesLength * 2    // Longer series → higher priority
            + slot.GroupCount                // More groups → higher priority
            + (slot.EndsOnIncompleteWeek ? 100 : 0);

    if (slot.AvailableDomains == 1)
        res += 1_000_000;   // Only one option — must be scheduled immediately

    if (slot.LessonSeriesLength < 4)
        res -= 10_000;      // Short series — defer

    if (slot.GroupSubject.Subject.SubjectTypeName == "Consultation")
        res -= 10_000;      // Consultations have low priority

    return res;
}
```

**Logic:**
- Base score accounts for number of available slots, series length, group count, and whether the slot ends on an incomplete week
- Lessons with only one available slot receive an extreme priority boost
- Short series and consultations are deprioritised
