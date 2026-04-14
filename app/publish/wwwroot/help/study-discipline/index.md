# Study Disciplines

## Overview

**Study Disciplines** are the backbone of schedule generation. They contain information about subjects, their types, lesson counts, and classroom requirements.

The system uses a two-level hierarchy:
- **Study Discipline** — a general course taught for one or more educational programs
- **Subject** — a specific delivery type within a discipline (lecture, practical, lab, etc.)

## Access

Faculty Administrators only.

**Navigation:** Main menu → **Academic Process** → **Study Disciplines**

## Viewing Disciplines

Select filters:
1. **Academic Semester** — required
2. **Year of Study** — optional
3. **Educational Program** — optional

| Column | Description |
|--------|-------------|
| Name | Discipline name |
| Subject Types | Types present (lec, lab, prac, …) |
| Educational Programs | Programs for which it is taught |
| Teacher Count | Number of assigned teachers |
| Actions | Edit or delete |

## Creating a Discipline

1. Select the semester
2. Click **Add Discipline**
3. On the **General** tab:
   - Enter the discipline name
   - Select educational programs
   - Check the subject types to include (lecture, practical, etc.)
   - For each selected type enter the lesson count

4. Switch to individual subject-type tabs for additional settings
5. Click **Create**

## Subject Settings

For each subject type you can configure:

### Basic Parameters
- **Lesson count** — total lessons of this type per group

### Classroom Parameters
- **No classroom required** — the subject needs no room
- **Override classroom types** — specify explicit classroom types with priority values (lower priority = more likely to get that room type)

> If neither option is set, the system uses the classroom type linked to the subject type in the reference dictionaries (**General** → **Subject Types**).

### Lesson Series

A **lesson series** is a sequence of related lessons from the same group subject recurring on the same weekday and period with a fixed frequency.

For each series you can set:
- **Series number** — identifier
- **Lesson count** — lessons in this series (must sum to the total for the subject type)
- **Frequency** — *Weekly* or *Biweekly* (every other week)
- **Start in any week** — allow the series to start in any week; otherwise it starts in the first available week

> If no series are defined manually, the algorithm automatically determines the optimal split based on the number of weeks and lessons. Define series manually only when the automatic split does not meet your specific requirements.

## Example Configuration

**Discipline:** Programming  
**Programs:** Computer Science (1st year)

- **Lecture** — 15 lessons, weekly series of 15
- **Lab** — 30 lessons, two weekly series of 15 each; classroom type: Computer Lab (priority 10)

## Merging Disciplines

To merge two disciplines into one:

1. Select both disciplines using the checkboxes in the first column
2. Click **Merge Selected Disciplines**
3. The system combines unique subject types and educational programs

> Useful when a discipline was accidentally created twice or when two program-specific disciplines need to be unified.

## Editing a Discipline

1. Click the edit (pencil) icon or double-click the row
2. Modify the name, programs, or subject settings
3. Click **Save**

## Deleting a Discipline

1. Click the delete (trash) icon
2. Confirm

> A discipline can only be deleted if no lessons from its subjects are in any schedule.

## Relationships

| Related entity | Relationship |
|---------------|--------------|
| Educational Programs | Disciplines are created for programs |
| Subject Types | Templates for lecture, practical, lab, etc. |
| Teachers | Assigned to subjects in the Teachers section |
| Groups | Determined via educational programs |
| Schedule | Generated from disciplines, subjects, and teacher assignments |

## Recommendations

1. Use meaningful names matching the official curriculum
2. Set accurate lesson counts for each subject type
3. Use series to logically split lessons — this produces more balanced schedules
4. Specify classroom type requirements precisely
5. Verify program links — especially when merging streams
