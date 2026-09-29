# Data model

All data sources are SharePoint lists, except `NavigationMenu` (menu definition).

## VisaRecords

| Column | Type | Notes |
|---|---|---|
| VisaNumber | Text | Unique, alphanumeric only |
| StaffMember | Lookup → StaffRegistry | |
| VisaType | Lookup → VisaCategories | |
| IssuingCountry | Lookup → Nationalite | |
| IssueDate | Date | |
| ExpiryDate | Date | |
| Status | Choice | `Active`, `Expiring Soon`, `Expired` |
| Notes | Text | |
| DaysToExpiry | Calculated | Computed by SharePoint, used by the flow |
| Modified / Modified By | System | Shown in Admin history |

## VisaRequests

| Column | Type | Notes |
|---|---|---|
| Request ID | Text | Format `REQ-YYYY-NNN`, generated in the app |
| StaffMember | Lookup → StaffRegistry | |
| VisaType | Lookup → VisaCategories | |
| PaysDestination | Lookup → Nationalite | Destination country |
| RequestDate | Date | |
| SubmissionDate | Date | Optional |
| ApprovalDate | Date | Optional, cannot precede RequestDate |
| PassportReturnDate | Date | Optional |
| RequestStatus | Choice | `Initiated`, `Documents Collected`, `Submitted to Embassy`, `Approved`, `Passport Returned`, `Rejected`, `Cancelled` |
| Priority | Choice | `Low`, `Normal`, `Urgent` |
| AssignedOfficerText | Choice | Responsible officer |
| Notes | Text | Internal notes |

## StaffRegistry

| Column | Type | Notes |
|---|---|---|
| StaffMember (Title) | Text | Full name |
| StaffID | Number | Employee ID; next value = `Max(StaffID) + 1` |
| Email | Text | Validated with `Match.Email` |
| PassportNumber | Text/Number | Optional |
| Nationality | Lookup → Nationalite | Required |
| `Departement ` | Lookup → Department | Required. **The column name ends with a space** |

## Reference lists

| List | Key column | Used for |
|---|---|---|
| VisaCategories | VisaTypeName | Visa type dropdowns |
| Nationalite | Pays | Nationality, issuing and destination country |
| Department | Title | Staff department |

## Status translations (UI ↔ data)

| Data value | UI label |
|---|---|
| Active | Actif |
| Expiring Soon | Expire bientôt |
| Expired | Expiré |
| Initiated | Initié |
| Documents Collected | Documents collectés |
| Submitted to Embassy | Soumis à l'ambassade |
| Approved | Approuvé |
| Passport Returned | Passeport rendu |
| Rejected / Cancelled | Rejeté / Annulé |
| Low / Normal / Urgent | Faible / Normal / Urgent |
