# Architecture

## Components

| Layer | Technology | Role |
|---|---|---|
| Front end | Power Apps canvas app | UI, validation, filtering, navigation |
| Storage | SharePoint lists | Source of truth for all data |
| Logic | Power Automate | All create/update operations + scheduled alerts |
| Identity | Microsoft 365 / Entra ID | `User()` for greeting, admin check and "Modified By" |

## Data flow

**Read.** Screens read SharePoint lists directly (`VisaRecords`, `VisaRequests`, `StaffRegistry`, `VisaCategories`, `Nationalite`, `Department`). Filtering, search and sorting run in the app.

**Write.** The app never uses `Patch`. Every write calls an instant flow with `'FlowName'.Run(...)`, wrapped in `IfError(...)`, followed by a success or error `Notify`.

**Automation.** Two scheduled flows keep statuses correct and notify people:
- `VA-VisaExpiry Check` reads the SharePoint `DaysToExpiry` computed column and updates the visa status.
- `Send Emails` sends the alert e-mail after a visa expires.

## Request → visa lifecycle

```
Initiated → Documents Collected → Submitted to Embassy → Approved → Passport Returned
                                                    └→ Rejected / Cancelled
```

On an *Approved* request, **Convert to visa** runs `Creerunvisa` (status *Active*) and then `Modifierunedemande` to set the request to *Passport Returned*.

## Navigation and state

- Five screens share one left-hand menu driven by the `NavigationMenu` table (rows 1–5). Row 5 (Admin) is hidden unless `varIsAdminAuthorized` is true.
- The hamburger icon toggles `IsExpand`, which collapses the menu and shifts control positions.
- Dashboard tiles set a filter variable (`varVisaFilter` or `varRequestFilter`) and navigate, so the target list opens pre-filtered.

See [screens.md](screens.md) for the variable reference.

## Design decisions

- **Flows for writes**: centralises validation and audit (`Modified`, `Modified By`) on the SharePoint side.
- **Computed `DaysToExpiry` in SharePoint**: avoids date maths inside the flow.
- **Dates as `yyyy-mm-dd` text** when passed to flows, to avoid locale issues.
- **French Power Fx locale** in Studio: `;` separates arguments and `;;` chains actions. Exported YAML uses the English form.

## Not in the export

`App.OnStart` and the definition of `NavigationMenu` / `IsExpand` initial values are not part of the screen export. Document them here once confirmed.
