# Group Subject Examples

Step-by-step examples of the most common group-subject scenarios.

---

## 1. Merging a Lecture for Multiple Groups (Streaming)

When the same subject is delivered to several groups simultaneously, use the **Merge Class** feature.

### Steps

**Create the subject for the first group:**
1. Select the semester, year, and the first group
2. Find the subject in the unsaved list
3. Click **Save** or **Choose Teacher and Save**

**Add the second group to the same class:**
1. Select the same semester and year
2. Switch to the second group
3. Find the same subject in the unsaved list
4. Click **Merge Class**
5. In the dialog select the class already created for the first group
6. Click **Select**

**Result:**
- One shared class is created for both groups
- The subject card shows all groups
- One teacher is assigned for all groups

> Any changes to the merged class (e.g. changing the teacher) apply to all groups.

---

## 2. Splitting a Group into Subgroups for Labs

### Prerequisite

Subgroups must exist in **Academic Process** → **Student Groups** before proceeding here.

**How to create subgroups (if not done yet):**
1. Go to **Student Groups**
2. Find or create the group → click **Edit**
3. Click **Add Subgroup Block** (bottom-right)
4. In the new tab:
   - Select the distribution type (e.g. "Lab Groups")
   - Click **Add Subgroup** for each subgroup
   - Enter unique name and student count for each
5. Click **Save**

### Creating the Subject with Subgroups

1. Select semester, year, and group
2. Find the subject in the unsaved list
3. Click **Split into Subgroups**
4. Select the distribution type (e.g. "Lab Groups")
5. Click **Select**

### Assigning Teachers to Subgroups

After the split, two (or more) new entries appear — one per subgroup:
1. For each subgroup click **Choose Teacher and Save**
2. Select the appropriate teacher
3. You can assign the same teacher or different teachers to each subgroup

**Result:**
- Separate classes are created for each subgroup
- Each subgroup can have a different teacher
- Subgroup classes can be scheduled at different times

---

## 3. Organising Elective Subjects

This scenario applies when students choose from several elective disciplines and each choice forms a different subgroup.

### Preparation

**Create a distribution type for electives:**
1. Go to **General** → **Subgroup Types**
2. Create a new type named "Elective Disciplines"

**Create subgroups in Student Groups:**
1. Go to **Academic Process** → **Student Groups**
2. Edit the group → click **Add Subgroup Block**
3. Select distribution type "Elective Disciplines"
4. Add one subgroup per elective (name = elective name, set student count)
5. Click **Save**

### Assigning Elective Subjects

For each elective discipline repeat:

1. Find the subject in the unsaved list
2. Click **Split into Subgroups** → select "Elective Disciplines" → **Select**
3. The system creates entries for all subgroups of this type
4. Delete the subgroups that do NOT attend this elective (click **Remove from Subgroup**)
5. Assign a teacher to the remaining subgroup

**Example:**

Group "IST-11" · 100 students · three electives:

*In Student Groups:*
- Subgroup 1 "Software Architecture" (30 students)
- Subgroup 2 "Machine Learning" (40 students)
- Subgroup 3 "Information Security" (30 students)

*In Group Subjects:*
- For "Software Architecture": split into 3 subgroups → keep subgroup 1, remove 2 and 3 → assign teacher
- For "Machine Learning": split into 3 subgroups → keep subgroup 2, remove 1 and 3 → assign teacher
- For "Information Security": split into 3 subgroups → keep subgroup 3, remove 1 and 2 → assign teacher

**Result:**
- Each elective is assigned to exactly one subgroup
- All three electives are scheduled at the same time in the timetable
