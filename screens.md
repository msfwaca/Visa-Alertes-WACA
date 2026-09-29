# Screens

The app has five screens sharing one header (`Visa Alertes WaCA`) and one collapsible menu.

## HomeScreen – "Status des Visas"

Dashboard. Greets the user (`Salut, <name> 👋`) and shows today's date in French.

- **Visa tiles**: Active, Expiring soon, Expired. Counts come from `CountRows(Filter(VisaRecords, Status.Value = ...))`. The *Expiring soon* tile also requires `ExpiryDate <= Today()+30`.
- **Request tiles**: Initiated, Submitted to Embassy, Documents Collected, Passport Returned.
- Each tile has a transparent button that sets `varVisaFilter` / `varRequestFilter` and navigates to the matching list.
- `OnVisible` resets filters and error flags and computes `varIsAdminAuthorized`.

## VisaRecordsScreen

List and form for visas.

- **List**: search box (visa number, staff, type, status, country) + status dropdown (*Tous, Actif, Expire Bientot, Expiré*). Sorted by ID descending.
- **Form** (overlay, `varShowForm`): staff, visa type, issuing country, visa number, issue date, expiry date, status, notes.
- **Save**: validates required fields and visa-number uniqueness (new only), then runs `Creerunvisa` (mode `New`) or `Modifierunvisa` (mode `Edit`). In edit mode, blank dropdowns fall back to the selected record's values.
- Can open directly in `New` mode with data from a request (`varConvertFromRequest`).

## RequestTrackingScreen – "Suivi des demandes"

- **List**: search + status dropdown; coloured status and priority badges.
- **Form**: auto-generated ID (`REQ-<year>-<seq>`, sequence = highest of the current year + 1, 3 digits), staff, visa type, destination, request date, status, officer, priority, notes, submission / approval / passport-return dates.
- **Validation**: all core fields required; approval date cannot be before request date.
- **Save**: `Creerunedemande` or `Modifierunedemande`.
- **Convert to visa** (visible when status is *Approved*): needs visa number (alphanumeric, unique), issue date ≤ today, expiry date > today and > issue date, destination. Runs `Creerunvisa` then sets the request to *Passport Returned*.

## StaffDirectoryScreen – "Personnel"

- **List**: search + column filter (*Tous, Nom, Département, Nationalité, Email, Numéro de passeport, ID employé*).
- **Form**: name, e-mail, nationality, department, passport number (optional), employee ID (`varNextStaffID` = next number when adding).
- **Save**: requires name, valid e-mail, nationality and department; runs `AjouterPersonnel` or `ModifierPersonnel`.

## AdminScreen – "Historique des modifications"

Restricted. Refreshes `VisaRecords`, `VisaRequests` and `StaffRegistry` on open, then shows a read-only history sorted by `Modified` descending, with three tabs (`varAdminTab`): **Visas**, **Demandes**, **Personnel**. Columns: identifier, staff/department, type/status, modified by, modification date.

## Variable reference

| Variable | Purpose |
|---|---|
| `_selectedTutorial` | Current menu row `{Row, Title}` (drives highlight) |
| `IsExpand` | Menu collapsed (true) or expanded |
| `varIsAdminAuthorized` | Shows the Admin menu entry |
| `varVisaFilter` / `varRequestFilter` | Pre-filter set by dashboard tiles |
| `varFormMode` | `New`, `Edit`, `Add` or empty |
| `varShowForm` / `IsGalleryVisible` | Visa/staff form visibility / request form visibility |
| `varSelectedVisa` / `varSelectedRequest` / `varSelectedStaff` | Record being edited |
| `varRequestNumericID` / `varStaffNumericID` | SharePoint item ID passed to update flows |
| `varNextStaffID` | Suggested next employee ID |
| `varShowErrors` / `varShowConvertErrors` | Show inline validation messages |
| `varSaveSuccess` / `varConvertSuccess` | Result of the last flow call |
| `varConvertFromRequest` / `varConvertRequest` | Convert-to-visa handoff |
| `varAdminTab` | Active tab on the Admin screen |
