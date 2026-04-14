# Academic Semesters

The **Academic Semesters** (Educational Seasons) section is the central component of AcaTime. It lets you create and manage the scheduling periods for a faculty.

## Overview

A semester is the main scheduling unit. The following are linked to a semester:
- Student groups
- Teachers and their workload
- Study disciplines
- Classrooms
- Scheduling rules and constraints

## Access

Faculty Administrators only.

**Navigation:** Main menu → **General** → **Academic Semester**

## Semester List

| Column | Description |
|--------|-------------|
| Period Name | Semester name (e.g. "Autumn 2023–2024") |
| Start Date | Semester start |
| End Date | Semester end |
| Max Periods/Day | Maximum allowed periods per day |
| Actions | Edit or delete |

## Creating a Semester

1. Click **Add Semester** on the toolbar
2. On the **Main Data** tab fill in:
   - **Period Name**
   - **Start Date**
   - **End Date**
   - **Max Periods per Day**

3. Optionally switch to the **Import** tab:

   **a) Import from file**
   - Click **Choose File** to upload a ZIP archive with import data
   - See [Importing Data from the "Triton" System](./import.md) for details

   **b) Copy from another semester**
   - Select an existing semester
   - Choose what to copy:
     - **Import Groups** — groups and subgroups
     - **Import Scripts** — scheduling rules and constraints
     - **Import Classrooms** — classrooms and their types

4. Click **Save**

> If you choose file import, the **Import Groups** option is disabled because groups will be imported from the file.

## Editing a Semester

1. Double-click a row in the table
2. Modify the required fields (Period Name, dates, max periods/day)
3. Click **Save**

> The **Import** tab is not available when editing — import is only possible during creation.

## Deleting a Semester

1. Find the semester in the table
2. Click **Delete** in the Actions column
3. Confirm

> **Warning:** Deleting a semester permanently removes all associated data: schedule, groups, teachers, disciplines, classrooms, rules, and API keys. This action is irreversible.

## Copying Data Between Semesters

When creating a new semester you can copy from an existing one:

1. **Copy Groups** — groups, subgroups, and their educational program links
2. **Copy Scripts** — scheduling rules and constraints
3. **Copy Classrooms** — classrooms and their types

## Technical Notes

- A new semester is automatically linked to the current user's faculty
- Semesters are listed in descending ID order (newest first)
- File import is transactional — either all data is imported or nothing is changed
- During import, educational programs are matched by name and year using the Levenshtein algorithm

## Additional Resources

- [Importing Data from the "Triton" System](./import.md)
