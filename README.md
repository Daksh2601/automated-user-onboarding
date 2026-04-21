# Project 11 — Automated User Onboarding & Offboarding

## Overview

PowerShell automation that eliminates manual Active Directory user provisioning. HR drops a CSV file in a shared folder, and the script automatically creates accounts, assigns groups, generates credentials, and handles offboarding — reducing a 30-45 minute manual process to under 5 seconds per employee.

## How It Works

```
HR drops CSV in Incoming folder
         ↓
Scheduled Task runs hourly
         ↓
Script reads CSV entries
         ↓
┌─── Onboard ───┐    ┌─── Offboard ───┐
│ Create AD user │    │ Disable account │
│ Add to group   │    │ Remove groups   │
│ Create home dir│    │ Move to Disabled│
│ Gen password   │    │ Users OU        │
│ Save creds file│    └─────────────────┘
└────────────────┘
         ↓
CSV archived with timestamp
All actions logged
```

## CSV Format (What HR Fills Out)

```csv
Action,FirstName,LastName,Department,JobTitle
Onboard,Tom,Bradley,IT Department,Network Engineer
Onboard,Sofia,Martinez,HR Department,Recruiter
Offboard,James,Wilson,HR Department,
```

## What the Script Does

### Onboarding (4 Steps)
1. Creates AD account in correct OU with email, title, department
2. Adds user to department security group (GRP-IT, GRP-HR, etc.)
3. Creates home directory at C:\Users\HomeDirectories\username
4. Generates random secure password and saves credentials file

### Offboarding (3 Steps)
1. Disables the AD account (prevents login immediately)
2. Removes from all security groups (revokes access)
3. Moves account to Disabled Users OU (preserves for compliance)

## Project Structure

```
C:\Automation\
├── UserAutomation.ps1        # Main automation script
├── employees.csv             # Initial batch CSV
├── automation_log.txt        # Full audit trail
├── Incoming\                 # HR drops new CSVs here
├── Processed\                # Archived CSVs with timestamps
│   └── 2026-04-21_02-46-47_new_hires_april.csv
└── NewCredentials\           # Auto-generated credential files
    ├── tom.bradley.txt
    └── sofia.martinez.txt
```

## Key Features

| Feature | Description |
|---|---|
| Auto-Password Generation | Random 12-char passwords (upper, lower, numbers, special) |
| Duplicate Prevention | Checks if user exists before creating |
| Folder Watching | Scans Incoming folder for new CSV files |
| CSV Archiving | Processed files moved with timestamp |
| Audit Logging | Every action timestamped in automation_log.txt |
| Scheduled Execution | Runs hourly via Windows Task Scheduler |
| Credential Files | Saved securely for manager delivery |

## Time Savings

| Task | Manual | Automated |
|---|---|---|
| Create AD account | 5-10 min | 2 sec |
| Add to security group | 2 min | Automatic |
| Create home folder | 3 min | Automatic |
| Generate credentials | 5 min | Auto-generated |
| Disable departing user | 5-10 min | 2 sec |
| **Total per employee** | **30-45 min** | **Under 5 sec** |

## Results

| Operation | User | Department | Result |
|---|---|---|---|
| Onboard | Kevin Hart | IT | ✅ SUCCESS |
| Onboard | Jessica Lee | HR | ✅ SUCCESS |
| Onboard | Ryan Patel | Finance | ✅ SUCCESS |
| Onboard | Natasha Kim | Management | ✅ SUCCESS |
| Onboard | Tom Bradley | IT | ✅ SUCCESS (auto-password) |
| Onboard | Sofia Martinez | HR | ✅ SUCCESS (auto-password) |
| Offboard | James Wilson | HR | ✅ Disabled + moved |
| Offboard | Lisa Anderson | Finance | ✅ Disabled + moved |

## Technologies

- PowerShell 5.1
- Active Directory Module
- Windows Server 2022
- Windows Task Scheduler
- Microsoft Azure (IaaS)

## Author

**Daksh Patel** — CCNA Certified
- GitHub: [github.com/Daksh2601](https://github.com/Daksh2601)
- LinkedIn: [linkedin.com/in/pateldaksh](https://linkedin.com/in/pateldaksh)
- Portfolio: [daksh2601.github.io/Portfolio](https://daksh2601.github.io/Portfolio)
