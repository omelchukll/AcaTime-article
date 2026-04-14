# AcaTime — System Overview

AcaTime is a comprehensive academic timetable management system designed for higher education institutions. It automates schedule generation taking into account teacher availability, classroom resources, and all institutional requirements.

## Core Components

### Organisational Structure

- **Faculty** — the base organisational unit. Each faculty manages its data independently.
- **Academic Semester** — the scheduling period (autumn or spring semester of an academic year).
- **Educational Program** — the curriculum for a particular specialisation.
- **Year of Study** — the student cohort year (1st, 2nd, etc.).

### Participants

- **Student Group** — the primary cohort following a given educational program.
- **Subgroup** — a subset of a group for laboratory or practical sessions.
- **Teacher** — a person delivering classes in specific disciplines.

### Academic Content

- **Study Discipline** — a general course within an educational program.
- **Subject** — a specific class type within a discipline (lecture, practical, lab).
- **Class (Lesson)** — a single occurrence of a subject for a group at a given time, room, and teacher.
- **Lesson Series** — a sequence of related classes recurring at a fixed frequency.

### Resources

- **Classroom** — a room with a defined capacity.
- **Classroom Type** — a characteristic determining which subject types can be held in a room (lecture hall, computer lab, laboratory, etc.).

## Working with the System

### 1. Semester Setup

The administrator creates a new semester specifying:
- Semester name
- Start and end dates
- Maximum number of periods per day

When creating a new semester you can copy groups, scripts, and classrooms from an existing one.

### 2. Managing Core Data

**Groups and Subgroups**
- Create student groups linked to educational programs and years of study
- Split groups into subgroups by different criteria (labs, foreign language, etc.)
- Specify student counts for each group and subgroup

**Classrooms**
- Add classrooms with their capacity
- Assign one or more types to each classroom

**Teachers**
- Register faculty teachers
- Specify position/title

### 3. Planning Academic Workload

**Disciplines and Subjects**
- Create study disciplines for educational programs
- Add subjects with type (lecture, practical, lab) and lesson count
- Set classroom type requirements per subject

**Workload Distribution**
- Assign teachers to subjects
- Create group subjects for groups and subgroups
- Merge groups for joint lectures (streaming)

### 4. Automatic Schedule Generation

The algorithm considers:
- Teacher availability
- Suitable classrooms
- Avoiding gaps in student timetables
- Even distribution of classes across the week
- Custom rules and constraints

### 5. Review and Adjustment

After generation, users can view the schedule by group, teacher, or classroom.

---

See [Getting Started](./getting-started.md) for a step-by-step walkthrough.
