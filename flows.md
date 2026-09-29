# Power Automate flows

## Instant flows (called from the app)

Parameters are positional and must stay in this order.

| Flow | Called from | Parameters |
|---|---|---|
| `Creerunvisa` | Visas, Requests (convert) | VisaNumber, Status, Notes, StaffID, VisaTypeID, IssuingCountryID, IssueDate, ExpiryDate |
| `Modifierunvisa` | Visas | VisaNumber, StaffID, VisaTypeID, IssuingCountryID, IssueDate, ExpiryDate, Status, Notes, item ID |
| `Creerunedemande` | Requests | RequestID, StaffID, VisaTypeID, RequestDate, Status, Officer, Priority, Notes, SubmissionDate, ApprovalDate, PassportReturnDate, DestinationCountryID |
| `Modifierunedemande` | Requests | Same as create + item ID |
| `AjouterPersonnel` | Staff | Name, EmployeeNumber, NationalityID, DepartmentID, Email |
| `ModifierPersonnel` | Staff | Same as add + item ID |

Conventions: dates are sent as `yyyy-mm-dd` text; empty optional text is sent as `" "` or `""`; lookups are sent as numeric IDs.

## Scheduled flows

| Flow | Purpose |
|---|---|
| `VA-VisaExpiry Check` | Reads the SharePoint `DaysToExpiry` column and updates each visa's status (*Active → Expiring Soon → Expired*). |
| `Send Emails` | Sends the alert e-mail; runs after the visa expiry check. |

Notes:
- `DaysToExpiry` is computed by SharePoint, not in the flow.
- The flow handles about 100 records per run; raise the pagination threshold if the list grows.
- To complete: trigger schedule, thresholds and e-mail recipients.

## Error handling

The app wraps each call in `IfError`. On failure the user sees a French error toast and the form stays open. Flow run history is the place to debug.
