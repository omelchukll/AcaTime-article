# Script Types in the Schedule System

AcaTime uses several script types, each fulfilling a specific role in the timetable generation process. Scripts let you tune the heuristic algorithm to your institution's requirements.

**Important:** You can create and use **multiple scripts** of each type simultaneously. Scores from all scripts of the same type are **summed**. Constraint scripts (types −1, 11, and 3) work as sequential filters — a lesson must pass all of them to be included in the schedule.

---

## Type −1 and 11 — Slot Selector and Slot Constraint

These two types work in tandem:

**Slot Selector (type −1)**
```csharp
Func<IFacultySeason, IEnumerable<IScheduleSlot>>
```
Selects a subset of slots that will be passed to the type 11 constraint. Using a selector focuses checks on a specific subset instead of testing every possible slot.

**Slot Constraint (type 11)**
```csharp
Func<IScheduleSlot, bool>
```
Determines whether a lesson may be placed in a given slot:
- `true` — allowed
- `false` — not allowed

**Interaction:**
1. Type −1 selects the slots to constrain
2. Type 11 tests each selected slot
3. The result is a filtered set of valid slots

**Multiple constraint pairs:**  
If several pairs of types −1 and 11 are active they act as a cascade — a slot must pass all filters.

**Applications:**
- Restricting lessons to specific weekdays
- Banning certain subject types at certain times
- Defining special requirements for individual subjects

---

## Type 1 — Schedule Evaluator

```csharp
Func<IFacultySeason, int>
```

Evaluates the overall quality of the completed schedule. Higher values indicate better schedules. Used *after* candidate schedules are generated to compare and rank them.

**Applications:**
- Assessing gaps in student and teacher timetables
- Counting free days
- Penalising excessive daily workload (4+ periods)

Detailed examples: [Evaluation Examples](../script-examples/evaluation-examples.md)

---

## Type 2 — Day/Period Estimator

```csharp
Func<IScheduleSlot, IAssignedSlots, int>
```

Evaluates how well a lesson fits a specific time slot given the already-assigned lessons. Higher values indicate a better fit.

**Applications:**
- Preferring lectures in the first half of the day
- Prioritising certain weekdays for certain groups
- Compactness for teachers and students (adjacent periods)
- Even workload distribution per day

---

## Type 3 — Day/Period Validator

```csharp
Func<IScheduleSlot, IAssignedSlots, bool>
```

Checks whether a lesson can be placed in a slot given the current schedule state.
- `true` — allowed
- `false` — not allowed

Multiple type 3 scripts work as sequential filters — a lesson may be placed only if all validators return `true`.

**Applications:**
- Verifying no conflict with other lessons
- Ensuring sufficient travel time between rooms for a teacher
- Complex constraints based on the existing schedule

---

## Type 4 — Lesson Priority Estimator

```csharp
Func<ISlotEstimation, int>
```

Determines which lesson to add to the schedule next. Higher values indicate higher scheduling priority.

**Applications:**
- Prioritising lessons with fewer available time slots
- Prioritising important subjects
- Prioritising lessons with more student groups
- Prioritising longer series

---

## Script Interaction During Schedule Generation

| Phase | Script types used |
|-------|--------------------|
| Domain reduction | −1 and 11 |
| Lesson selection | 4 |
| Slot selection | 2 |
| Slot validation | 3 |
| Quality evaluation | 1 |

---

## Further Reading

- [Algorithm Overview](./index.md)
- [Data Models](./models.md)
- [Schedule Estimation](./estimation.md)
- [Constraints](./constraints.md)
- [Using AI to Write Scripts](./ai-scripts.md)
