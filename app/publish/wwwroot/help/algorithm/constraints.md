# Constraints in the Schedule System

Constraints define which combinations of lessons, teachers, groups, classrooms, and time slots are permissible in the schedule. They are the key element for producing a realistic and practical timetable.

## Types of Constraints

### 1. Hard System Constraints

Built into the system and cannot be changed:
- **No overlap for groups** — a group cannot attend two lessons at the same time
- **Unique teacher** — a teacher cannot deliver two lessons at the same time
- **Classroom availability** — a room cannot host more than one lesson at the same time

### 2. User-Defined Constraints (Scripts)

Created by users to tailor the system to the institution's specific needs.

---

#### Slot Constraint (type 11)

```csharp
Func<IScheduleSlot, bool>
```

Determines whether a lesson can be placed in a specific time slot. Applied early in optimisation, significantly reducing the search space.

Scripts of type 11 work in tandem with slot selectors (type −1):
1. The type −1 script runs first and selects a subset of slots to constrain
2. For each selected slot the type 11 script tests every possible date/period combination
3. Type 11 returns `true` for allowed values and `false` for rejected ones
4. Only date/period combinations that return `true` are available for scheduling

**Examples:**
- A specific teacher may only teach on Thursdays → type −1 selects that teacher's slots; type 11 checks the day of week
- No lessons on weekends → type −1 selects all slots; type 11 rejects Saturday and Sunday
- 4th-year bachelor lessons must finish before 20 April

---

#### Day/Period Validator (type 3)

```csharp
Func<IScheduleSlot, IAssignedSlots, bool>
```

Checks whether a lesson can be placed in a specific slot *given the already-assigned lessons*. Enables complex constraints that depend on the current schedule state.

**Examples:**
- No more than 3 periods per day for a teacher or group
- No multi-hour lab after a lecture by the same teacher on the same day

---

## How to Create Effective Constraints

### Step 1 — Define the Goal

- What problem are you solving?
- Which situations must be avoided?
- What outcome do you expect?

### Step 2 — Choose the Constraint Type

| Type | Use when |
|------|----------|
| Slot constraint (11) | The rule does not depend on other already-assigned lessons |
| Day/period validator (3) | The rule depends on the current state of the schedule |

### Step 3 — Develop the Logic

1. Identify the data you need (groups, teachers, subjects, etc.)
2. Build the condition logic
3. Make sure the script returns the correct boolean:
   - `true` — the lesson is allowed in this slot
   - `false` — the lesson is not allowed in this slot

### Step 4 — Test

Disable other constraints, generate a schedule, and verify that the constraint behaves as expected.

---

## Balancing Constraints and Optimisation

1. **Too many hard constraints** may make it impossible for the algorithm to find any valid schedule
2. **Too few constraints** may produce a technically valid but practically unacceptable schedule

**Recommendations:**
- Use hard constraints only for requirements that must never be violated
- Use soft constraints (evaluation scripts) for desirable but non-critical characteristics
- Add constraints incrementally and verify the algorithm can still find a solution

---

## Examples

Detailed constraint script examples: [Constraint Examples](../script-examples/group-slot-constraint-examples.md)

---

## AI Assistance

For complex constraints you can use the [integrated AI service](./ai-scripts.md) to generate a baseline script that you can then refine.

## Further Reading

- [Script Types](./script-types.md)
- [Data Models](./models.md)
- [Using AI to Write Scripts](./ai-scripts.md)
