# Data Models for Scripts

AcaTime exposes a set of interfaces that provide access to academic process data. These models are used in estimation and validation scripts.

## Core Models

### IFacultySeason

Represents the faculty academic season — the root data container.

```csharp
public interface IFacultySeason
{
    /// <summary>Season identifier.</summary>
    long Id { get; }

    /// <summary>Faculty and season name.</summary>
    string Name { get; }

    /// <summary>Subjects assigned to groups.</summary>
    IReadOnlyList<IGroupSubject> GroupSubjects { get; }

    /// <summary>Semester start date.</summary>
    DateTime BeginSeason { get; }

    /// <summary>Semester end date.</summary>
    DateTime EndSeason { get; }

    /// <summary>Maximum periods per day.</summary>
    int MaxLessonsPerDay { get; }
}
```

### IStudentLessonGroup

Represents a student group (or subgroup) for a lesson.

```csharp
public interface IStudentLessonGroup
{
    long Id { get; }
    string Name { get; }
    string? SubgroupName { get; }
    long? SubgroupId { get; }
    long? SubgroupVariantId { get; }
    string? SubgroupVariantName { get; }
    int SubgroupCount { get; }
    long CourseYearId { get; }
    string CourseYearName { get; }
    long EducationalProgramId { get; }
    string EducationalProgramName { get; }
}
```

### IGroupSubject

Represents a subject delivered to a specific group.

```csharp
public interface IGroupSubject
{
    long Id { get; }
    ITeacher Teacher { get; }
    ISubject Subject { get; }
    IFacultySeason Faculty { get; }
    IReadOnlyList<IStudentLessonGroup> Groups { get; }
    IReadOnlyList<IScheduleSlot> ScheduleSlots { get; }
}
```

### ISubject

Represents a teaching subject.

```csharp
public interface ISubject
{
    long Id { get; }
    string Name { get; }
    int NumberOfLessons { get; }
    long DisciplineId { get; }
    string DisciplineName { get; }
    long SubjectTypeId { get; }
    string SubjectTypeName { get; }
    string SubjectTypeShortName { get; }
    /// <summary>Manually defined lesson series (if any).</summary>
    IReadOnlyList<ISubjectSeriesDto> DefinedSeries { get; }
}
```

### ITeacher

```csharp
public interface ITeacher
{
    long Id { get; }
    string Name { get; }
    string Position { get; }
}
```

---

## Schedule Models

### IScheduleSlot

Represents a single schedule slot — a lesson at a specific time.

```csharp
public interface IScheduleSlot
{
    long Id { get; }
    /// <summary>Sequential lesson number within the subject.</summary>
    int LessonNumber { get; }
    /// <summary>Date of the lesson.</summary>
    DateTime Date { get; }
    /// <summary>Period number (1-based).</summary>
    int PairNumber { get; }
    IGroupSubject GroupSubject { get; }
    /// <summary>Number of lessons in the same series on the same weekday and period.</summary>
    int LessonSeriesLength { get; }
}
```

### IAssignedSlots

A collection of already-assigned slots with lookup helpers.

```csharp
public interface IAssignedSlots
{
    IReadOnlyList<IScheduleSlot> Slots { get; }
    IReadOnlyList<IScheduleSlot> GetSlotsByTeacher(long teacherId);
    IReadOnlyList<IScheduleSlot> GetSlotsByGroup(long groupId);
    IReadOnlyList<IScheduleSlot> GetSlotsByTeacherAndDate(long teacherId, DateTime date);
    IReadOnlyList<IScheduleSlot> GetSlotsByGroupAndDate(long groupId, DateTime date);
}
```

### ISlotEstimation

Information for evaluating lesson scheduling priority.

```csharp
public interface ISlotEstimation
{
    /// <summary>Number of available time slots (not filtered out by constraints).</summary>
    int AvailableDomains { get; }
    bool EndsOnIncompleteWeek { get; }
    int GroupCount { get; }
    int LessonSeriesLength { get; }
    IGroupSubject GroupSubject { get; }
}
```

### ISubjectSeriesDto

Represents a lesson series for a subject.

```csharp
public interface ISubjectSeriesDto
{
    int NumberOfLessons { get; }
    int SeriesNumber { get; }
    SubjectSeriesSplitType SplitType { get; }
    bool StartInAnyWeek { get; }
}
```

### SubjectSeriesSplitType

```csharp
public enum SubjectSeriesSplitType
{
    Weekly   = 1,   // Every week
    Biweekly = 2    // Every other week
}
```
