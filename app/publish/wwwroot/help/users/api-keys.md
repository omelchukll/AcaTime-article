# API Key Management

API keys grant an external schedule-calculation application access to the data of a specific academic semester. Through these keys the external app can fetch the required data and save the computed schedule back to the system.

## Key Points

- **API keys are tied to a specific semester, not to the whole faculty.**
- Each key is bound to one semester at creation time.
- Only Faculty Administrators can create and manage keys.
- Keys can be activated or deactivated at any time.
- A newly created key is active by default.

## Accessing API Key Management

1. Log in as a Faculty Administrator
2. Open the **Administration** menu
3. Select **API Keys**

## Creating a Key

1. Select a semester from the list
2. Click **Create API Key**
3. Enter a description (e.g. "Schedule Calculator v2")
4. Click **Create**

> **Save the full key immediately** — it is displayed only once. Afterwards only the first 5 characters are shown.

## Key List Columns

| Column | Description |
|--------|-------------|
| Description | Purpose of the key |
| API Key | First 5 characters (masked) |
| Status | Active / Inactive |
| Created | Creation date |
| Last Used | Last access date |
| Actions | Available operations |

## Key Operations

### View Full Key
Click the eye icon to temporarily reveal the full key in a popup.

### Copy Key
Click the copy icon to copy the full key to the clipboard.

### Change Status
Click the edit icon → toggle **Active** → **Save**.

## API Endpoints

### Fetch Schedule Data

```
POST /api/ScheduleData
```

```json
{ "apiKey": "your_api_key" }
```

Returns: semester info, groups and subgroups, teachers, subjects, classrooms, evaluation functions and constraints.

### Save Computed Schedule

```
POST /api/ScheduleData/save
```

```json
{
  "apiKey": "your_api_key",
  "slots": [...],
  "score": 0,
  "algorithmName": "AlgorithmName"
}
```

## Security Guidelines

- Store keys securely
- Deactivate keys that are no longer in use
- Review the active key list regularly
- Never transmit keys over unsecured channels

## Technical Notes

- Keys are 48 characters long
- The system updates the last-used date on every access
- Inactive keys are rejected

## Validation on Save

When saving a computed schedule the system checks:
- All subject IDs in the slots exist in the system
- The number of slots per subject matches the configured value

If any check fails the API returns a detailed error message.
