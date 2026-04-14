# Schedule Evaluation and Optimisation

AcaTime uses multiple estimation types to produce not just a valid schedule, but an optimal one.

## Estimation Types

### Type 1 — Schedule Evaluator

```csharp
Func<IFacultySeason, int>
```

Analyses the complete schedule and assigns a numeric score. Higher = better. Used *after* candidate schedules are generated to compare them.

**Examples:**
- Gaps (free periods between lessons) for students and teachers
- Free days
- Penalties for excessive daily load (≥4 periods)

Detailed examples: [evaluation-examples.md](../script-examples/evaluation-examples.md)

---

### Type 2 — Day/Period Estimator

```csharp
Func<IScheduleSlot, IAssignedSlots, int>
```

Rates how well a lesson fits a given time slot, considering already-assigned lessons. Higher = better fit.

**Examples:**
- Preferring lectures in the first half of the day
- Favouring certain weekdays
- Compactness for teachers and students (adjacent periods)
- Even load distribution per day

---

### Type 4 — Lesson Priority Estimator

```csharp
Func<ISlotEstimation, int>
```

Determines which lesson to schedule next. Higher = higher priority. Scheduling constrained lessons early leads to faster convergence.

**Examples:**
- Lessons with fewer available slots get higher priority
- Subject importance
- Lessons with more student groups
- Longer series get higher priority

---

## Optimisation Strategies

### Balancing Competing Criteria

Common trade-offs:
1. **Compactness vs. load distribution** — a compact timetable (no gaps) is convenient but may cluster workload
2. **Student convenience vs. teacher convenience** — optimising one may harm the other; a balance is needed

### Multi-level Optimisation

1. **Base constraints** (types −1 and 11) — fundamental rules that must never be broken
2. **Lesson prioritisation** (type 4) — schedule "hard-to-place" lessons first (long series, many groups, few available slots)
3. **Slot rating** (type 2) — choose the best time slot for each lesson
4. **Overall evaluation** (type 1) — assess and compare complete schedule variants

---

## Practical Recommendations

### Using Weights

```csharp
// Total score = sum of (criterion weight × criterion score)
int totalScore =
    10 * compactnessScore +    // High weight — compactness is important
     5 * distributionScore +   // Medium weight
     3 * resourceScore;        // Lower weight
```

### Adaptive Optimisation

1. **Initial phase** — generate a baseline schedule satisfying hard constraints
2. **Analysis phase** — identify problem areas
3. **Improvement phase** — manually adjust series and type 2/4 estimators to target problem areas

---

## Summation of Multiple Scripts

Multiple scripts of the same type can be active simultaneously. Their results are **summed**, which allows:

1. **Separation of concerns** — one small script per aspect instead of one large complex script
2. **Modularity** — enable/disable individual scripts independently
3. **Transparency** — easier to identify which component affects the final score

**Example — three type 1 scripts:**
- Student gap evaluator → −500
- Teacher free-day evaluator → +1200
- Combined score → **+700**
