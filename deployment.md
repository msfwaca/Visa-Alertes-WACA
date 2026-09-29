# Deployment and configuration

## Prerequisites

- Power Apps and Power Automate licences for the maintainers and users.
- A SharePoint site with the lists from [data-model.md](data-model.md).
- Access to the environment where the solution will be imported.

## Steps

1. **Create the lists** and choice values exactly as documented (names and the trailing space in `Departement ` matter).
2. **Import the solution** (app + flows) into the target environment.
3. **Re-point connections**: SharePoint site URL, Outlook/O365 connection used by `Send Emails`.
4. **Check flow parameters** against [flows.md](flows.md).
5. **Set admin access** (below).
6. **Test** with one visa, one request and one staff member, then share the app.

## Admin access

`HomeScreen.OnVisible` sets `varIsAdminAuthorized` from a hard-coded list of e-mail addresses:

```
Set(varIsAdminAuthorized,
    Lower(User().Email) = "admin1@example.org" ||
    Lower(User().Email) = "admin2@example.org"
)
```

Do **not** commit real addresses to a public repository. Better options: a SharePoint `Admins` list or an Entra security group.

`AdminScreen.OnVisible` also sets `varIsAdminAuthorized` to `true`, so the menu check is the only real gate. Move the check into the screen if stronger protection is needed.

## Source control

Unpack the app with the Power Platform CLI and commit the result:

```
pac canvas unpack --msapp VisaAlertes.msapp --sources src
```

Review `src/` diffs on every change and never commit connection secrets.

## Maintenance checklist

- Add a visa type or country: add a row to `VisaCategories` / `Nationalite`.
- Add a department: add a row to `Department`.
- Add an officer: extend the `AssignedOfficerText` choice.
- Watch the ~100-record limit of the status flow.
