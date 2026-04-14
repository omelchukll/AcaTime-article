# Teachers

## Overview

The **Teachers** section manages teacher records and their subject workload. Correct teacher setup is an important prerequisite for automatic schedule generation, as the algorithm considers each teacher's assigned lessons.

## Access

Faculty Administrators only.

**Navigation:** Main menu → **Academic Process** → **Teachers**

## Teacher List

Select filters:
1. **Academic Semester** — required
2. **Educational Program** — optional (teachers for a specific program)
3. **Subject** — optional (teachers for a specific subject)

| Column | Description |
|--------|-------------|
| Full Name | Teacher's name |
| Position | Professor, Associate Professor, Assistant, etc. |
| Subject Count | Number of assigned subjects |
| Actions | Edit or delete |

> Hovering over the Subject Count value shows a tooltip listing the assigned subjects.

## Creating a Teacher

1. Select the semester
2. Click **Add Teacher**
3. On the **Basic Info** tab:
   - **Name** — full name
   - **Position** — academic title / position
4. Switch to the **Subjects** tab → click **Add Subject**
5. In the subject selection dialog choose the required subjects
6. For each subject enter the lesson count
7. Click **Save**

## Editing a Teacher

1. Click the edit (pencil) icon or double-click the row
2. On the **Basic Info** tab: change name or position
3. On the **Subjects** tab: add, remove, or update lesson counts
4. Click **Save**

### Adding Subjects to a Teacher

1. Open the teacher edit dialog
2. Go to the **Subjects** tab
3. Click **Add Subject**
4. Select subjects from the list
5. Click **Select**
6. Set the lesson count for each subject
7. Click **Save**

### Removing a Subject from a Teacher

1. Open the teacher edit dialog
2. Go to the **Subjects** tab
3. Click the trash icon next to the subject
4. Click **Save**

## Deleting a Teacher

1. Click the delete (trash) icon in the Actions column
2. Confirm

> A teacher can only be deleted if no schedule lessons are linked to them. Remove or reassign all related lessons first.

## Relationships

| Related entity | Relationship |
|---------------|--------------|
| Study Disciplines / Subjects | Subjects assigned to teachers |
| Educational Programs | Linked indirectly via subjects |
| Schedule | Teacher assignments are used during generation |
