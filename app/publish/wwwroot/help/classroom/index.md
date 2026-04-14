# Classrooms

## Overview

The **Classrooms** section manages information about teaching rooms. Correct classroom configuration is essential for schedule generation, as it directly affects lesson placement.

## Access

Faculty Administrators only.

**Navigation:** Main menu → **Academic Process** → **Classrooms**

## Classroom List

The table columns are:
- **Name** — room name or number
- **Capacity** — maximum number of students
- **Classroom Type** — type (lecture hall, lab, etc.)
- **Description** — additional notes
- **Actions** — edit or delete

## Adding a Classroom

1. Click **Add Classroom** on the toolbar
2. Fill in the dialog:
   - **Name** — unique name or number
   - **Capacity** — maximum student count
   - **Classroom Type** — select from the list
   - **Type Priority** — numeric priority value (see below)
   - **Description** — optional notes
3. Click **Save**

## Editing a Classroom

1. Click the edit (pencil) icon in the Actions column
2. Modify the fields
3. Click **Save**

## Deleting a Classroom

1. Click the delete (trash) icon in the Actions column
2. Confirm

> A classroom can only be deleted if it is not used in any schedule.

---

## Classroom Type Priorities

Type priorities are a key feature of AcaTime's scheduling algorithm.

### How Priorities Work

1. Each classroom is assigned a type (e.g. "lecture hall", "laboratory", "general")
2. Each type assignment has a numeric priority value
3. During scheduling the system prefers classrooms with a **lower** priority number of the required type

### Priority Strategies

**Basic approach:**
- Set priority **1** for the primary type of a room
- Set priority **2+** for secondary types if the room can serve multiple purposes

**Marking undesirable rooms:**
- Add a "Undesirable" classroom type with priority **1**
- Set the room's actual type with priority **2**
- The scheduler will use such a room only as a last resort

### Configuration Recommendations

1. **Avoid over-specialising types** — using broad types gives the algorithm more flexibility
2. **Use realistic capacity values** — the system automatically matches large lectures to large rooms
3. **Use priorities to guide optimisation** — priority 1 for preferred rooms, higher values for acceptable alternatives

## Interaction with Other Components

- **Subject Types** — you specify which classroom types are suitable for each subject type
- **Scheduling Algorithm** — considers capacity, types, and priorities
- **Constraints** — additional rules can be defined for classroom usage
