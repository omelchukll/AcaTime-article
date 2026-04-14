# Rules and Scripts in the Schedule System

## Why Scripts?

Building an ideal timetable requires accounting for countless factors. Every institution has unique requirements and traditions that differ from those of others.

That is why AcaTime offers a flexible rule and script system that lets you **adapt the heuristic scheduling algorithm to your specific needs**.

**Important:** You can create and use **multiple scripts** of each type simultaneously:
- For evaluation scripts (types 1, 2, 4) — results from all scripts of the same type are **summed**, enabling a modular quality assessment.
- For constraint scripts (types −1, 11, 3) — multiple scripts act as a **cascade of filters**; a slot must pass all constraints to be included in the schedule.

## Heuristics and Search Space

A typical faculty scenario:
- 200 group subjects
- ~10 lessons per subject per semester
- 15-week semester with 20 possible periods per week (5 days × 4 periods)

Total schedule slots: **15 × 20 = 300**  
Total lessons to place: **200 × 10 = 2,000**

The number of ways to place 2,000 lessons in 300 slots is approximately **300²⁰⁰⁰**, vastly exceeding the number of atoms in the observable universe (~10⁸⁰).

A supercomputer checking a billion variants per second would need far longer than the age of the universe to try all possibilities — hence the need for smart heuristics.

## How the Algorithm Works

0. **Domain reduction** — [Slot constraints](./constraints.md) (type 11) shrink the set of valid time slots for each lesson early in the process.

1. **Lesson selection** — an [Lesson selection estimator](./estimation.md) (type 4) determines which lesson to schedule next. Choosing wisely leads to better solutions.

2. **Slot ordering** — a [Day and period estimator](./estimation.md) (type 2) ranks available time slots for the chosen lesson.

3. **Validation** — at each assignment the algorithm checks built-in hard constraints (no double-booking of teachers, groups, or rooms) plus user-defined [Day/period validators](./constraints.md) (type 3).

4. **Backtracking** — if an assignment leads to a conflict the algorithm rejects it and tries the next option; if all options are exhausted it backtracks and reconsiders earlier decisions.

## Optimising Quality

AcaTime does not just find a feasible schedule — it optimises quality using [Schedule evaluators](./estimation.md) (type 1). Examples:
- Minimising gaps in student and teacher timetables
- Even workload distribution across the week
- Preferred days and periods
- Optimal classroom use

## Rules and Execution Time

Well-configured rules improve both quality and speed:
- [Slot constraints](./constraints.md) (types −1 and 11) quickly eliminate infeasible options
- [Lesson](./estimation.md) (type 4) and [slot](./estimation.md) (type 2) estimators guide the search towards the optimum, reducing backtracking

## Further Reading

- [Data Models for Scripts](./models.md)
- [Script Types](./script-types.md)
- [Using AI to Write Scripts](./ai-scripts.md)
- [Schedule Estimation](./estimation.md)
- [Constraints](./constraints.md)
