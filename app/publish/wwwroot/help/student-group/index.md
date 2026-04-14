# Student Groups

## Overview

The **Student Groups** section manages teaching groups and their subgroups. Correct group setup is essential for schedule generation — groups are the primary "consumers" of lessons.

The system supports flexible subgroup splitting, which is important for practicals, labs, and other class types delivered to part of a group.

## Access

Faculty Administrators only.

**Navigation:** Main menu → **Academic Process** → **Groups**

## Viewing Groups

Select filters:
1. **Academic Semester** — required
2. **Year of Study** — optional
3. **Educational Program** — optional

The table columns are:
- **Group Name**
- **Year of Study**
- **Educational Program**
- **Actions** — edit or delete

## Creating a Group

1. Select the semester
2. Click **Add Group**
3. Fill in the dialog:
   - **Group Name** — unique name
   - **Number of Students** — total headcount
   - **Year of Study**
   - **Educational Program**

4. Optionally configure subgroup splits:
   - Click **Add Subgroup Block**
   - Select a distribution type (e.g. "Foreign Language", "Lab Groups")
   - Add subgroups within the block:
     - Unique subgroup name
     - Number of students

5. Click **Save**

## Subgroups and Distribution Types

### Distribution Types

Defines the reason for splitting. Examples:
- Foreign language (English, German, French)
- Specialisation track
- Lab / practical sessions

### Subgroup Blocks

Each block corresponds to one distribution type and contains the concrete subgroups. A group can have multiple blocks with different types.

### Subgroups

Each subgroup has:
- Name
- Student count

**Example:**
```
Group "M-123" (30 students)
├─ Foreign Language split:
│   ├─ English (15 students)
│   ├─ German (10 students)
│   └─ French (5 students)
└─ Lab split:
    ├─ Lab-1 (15 students)
    └─ Lab-2 (15 students)
```

## Subgroup Requirements

1. **Unique distribution types per group** — no two blocks can have the same type
2. **Unique subgroup names within a block**
3. **Student counts** — the sum in subgroups need not equal the total group size (students may belong to subgroups of different types simultaneously)

## Editing a Group

1. Click the edit (pencil) icon or double-click the group row
2. Modify the required fields, add/remove blocks or subgroups
3. Click **Save**

## Deleting a Group

1. Click the delete (trash) icon
2. Confirm

> A group can only be deleted if no lessons are linked to it in the schedule.

## Recommendations

1. Use a consistent naming convention for groups
2. Specify accurate student counts — this affects classroom selection
3. Create meaningful subgroup splits reflecting real needs
4. Use clear subgroup names to simplify schedule management
5. Verify that subgroup student totals are close to the full group size

## Examples

### Example 1 — Lab Subgroups

Group "B-101" · 28 students · 1st year · Biology:
- Lab split:
  - Lab-1 (14 students)
  - Lab-2 (14 students)

### Example 2 — Multiple Split Types

Group "CS-201" · 24 students · 2nd year · Computer Science:
- Foreign Language split:
  - English-1 (12 students)
  - English-2 (12 students)
- Programming split:
  - Web (8 students)
  - Mobile (8 students)
  - Desktop (8 students)
