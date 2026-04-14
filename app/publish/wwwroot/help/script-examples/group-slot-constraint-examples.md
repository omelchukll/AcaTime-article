# Constraint Script Examples

Examples of constraint scripts used during schedule generation.

## Types −1 and 11 — Slot Selector and Slot Constraint

These two types work together:
- **Type −1 (Slot Selector)** — selects the slots to constrain
- **Type 11 (Slot Constraint)** — tests whether a lesson may be placed in a slot

**Important:** Multiple pairs of types −1 and 11 work as a cascade of filters. A date+period combination is valid only if it passes **all** constraints.

---

### Example 1 — No Classes on Weekends

**Selector (type −1):** select all slots.

```csharp
(faculty) => {
    return faculty.GroupSubjects.SelectMany(gs => gs.ScheduleSlots);
}
```

**Constraint (type 11):** allow only Monday through Friday.

```csharp
(slot) => {
    bool isNotSaturday = slot.Date.DayOfWeek != DayOfWeek.Saturday;
    bool isNotSunday   = slot.Date.DayOfWeek != DayOfWeek.Sunday;
    return isNotSaturday && isNotSunday;
}
```

---

### Example 2 — 3rd-Year Lessons Only After a Specific Date

**Selector (type −1):** select slots for 3rd-year groups.

```csharp
(faculty) => {
    return faculty.GroupSubjects
        .Where(gs => gs.Groups.Any(g => g.CourseYearName == "3rd year"))
        .SelectMany(gs => gs.ScheduleSlots);
}
```

**Constraint (type 11):** allow only on or after 3 March 2025.

```csharp
(slot) => slot.Date >= new DateTime(2025, 3, 3);
```

---

### Example 3 — 1st-Year Lectures Only on Monday and Tuesday

**Selector (type −1):** select lecture slots for 1st-year groups.

```csharp
(faculty) => {
    return faculty.GroupSubjects
        .Where(gs => gs.Groups.Any(g => g.CourseYearName == "1st year"))
        .SelectMany(gs => gs.ScheduleSlots
            .Where(slot => slot.GroupSubject.Subject.SubjectTypeName == "Lecture"));
}
```

**Constraint (type 11):** allow only Monday and Tuesday.

```csharp
(slot) => slot.Date.DayOfWeek == DayOfWeek.Monday
       || slot.Date.DayOfWeek == DayOfWeek.Tuesday;
```

---

### Example 4 — Specific Teacher Available Only on Certain Days and Periods

**Selector (type −1):** select slots for a specific teacher.

```csharp
(faculty) => {
    return faculty.GroupSubjects
        .Where(gs => gs.Teacher.Name == "Maria Petrenko"
                  && gs.Teacher.Position == "Professor")
        .SelectMany(gs => gs.ScheduleSlots);
}
```

**Constraint (type 11):** allow Monday periods 1–2 and Wednesday periods 2 and 4 only.

```csharp
(slot) => {
    bool mondayOk    = slot.Date.DayOfWeek == DayOfWeek.Monday
                    && (slot.PairNumber == 1 || slot.PairNumber == 2);
    bool wednesdayOk = slot.Date.DayOfWeek == DayOfWeek.Wednesday
                    && (slot.PairNumber == 2 || slot.PairNumber == 4);
    return mondayOk || wednesdayOk;
}
```

---

## Type 3 — Day/Period Validator

```csharp
Func<IScheduleSlot, IAssignedSlots, bool>
```

Tests whether a lesson may be placed in a slot given the **current schedule state**.

Multiple type 3 scripts work as sequential filters — a slot is valid only if all validators return `true`.

---

### Example — Max 3 Lessons per Day for 1st and 2nd Year Students

```csharp
(slot, assignedSlots) =>
{
    foreach (var candidateGroup in slot.GroupSubject.Groups
        .Where(x => x.CourseYearName == "1st year" || x.CourseYearName == "2nd year"))
    {
        var groupDaySlots = assignedSlots
            .GetSlotsByGroupAndDate(candidateGroup.Id, slot.Date.Date)
            .ToList();

        var variants = groupDaySlots
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
            return false;
    }
    return true;
}
```

**Logic:**
1. Only 1st and 2nd year groups are checked
2. All lessons already assigned to the group on that day are collected
3. The maximum lessons a student from any subgroup would attend is computed, accounting for all subgroup variants
4. If the total exceeds 3, the slot is rejected (`false`)
5. If all groups are within the limit, the slot is allowed (`true`)

This prevents overloading junior students even when lessons are distributed across different subgroups.
