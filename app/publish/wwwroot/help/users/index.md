# User Management

## User Roles

| Role | Description |
|------|-------------|
| **System Administrator** | Manages faculties and all users; full system access |
| **Faculty Administrator** | Manages faculty settings and users; creates schedules |
| **Operator** | Edits academic process data; limited access |

## User List Page

Requires System Administrator or Faculty Administrator role. The page shows:
- List of users (filtered by faculty for system admins)
- Add user button
- Per-user actions: edit, change password, delete

## Creating a User

1. Click **Add User**
2. Fill in the dialog:
   - **Login** — unique identifier (cannot be changed after creation)
   - **Full Name**
   - **Email**
   - **Role**
   - **Faculty** (not required for system admins)
   - **Password** (minimum 6 characters)
   - **Confirm Password**
3. Click **Save**

> Faculty Administrators can only create users for their own faculty and cannot assign the System Administrator role.

## Editing a User

1. Click the edit icon or double-click the user row
2. Modify the required fields
3. Click **Save**

## Blocking a User

1. Open the user for editing
2. Check **Blocked**
3. Save

Blocked users cannot log in until unblocked.

## Changing a Password

1. Click the change-password icon next to the user
2. Enter the new password
3. Click **Save**

## Deleting a User

> **Warning:** Deletion is irreversible.

1. Click the delete icon next to the user
2. Confirm in the dialog

Only System Administrators can delete users.

## Security Rules

- Faculty Administrators see and manage only their faculty's users
- Faculty Administrators cannot create System Administrators
- Password minimum length: 6 characters
- Login cannot be changed after creation
- Deletion is restricted to System Administrators
