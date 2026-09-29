# Visa Alertes WaCA

A Power Apps canvas app that tracks staff visas, visa requests and expiry alerts for **MSF WaCA**. Visa data lives in SharePoint lists; Power Automate flows handle every write and the automatic status/e-mail alerts.

> UI language: French. Code, variables and docs: English.

## What it does

- **Dashboard** – live counts of active / expiring soon / expired visas and of requests per stage. Every tile is a shortcut to a pre-filtered list.
- **Visas** – search, filter, create and edit visa records.
- **Requests** – follow a visa request from *Initiated* to *Passport Returned*. An approved request can be converted into a visa in one step.
- **Staff directory** – manage the staff registry (name, department, nationality, e-mail, passport number, employee ID).
- **Admin** – change history for visas, requests and staff (restricted).
- **Automatic alerts** – scheduled flows update visa statuses and send expiry e-mails.

## Architecture at a glance

```
Power Apps canvas app ──reads──▶ SharePoint lists
        │                              ▲
        └──── .Run() ──▶ Power Automate flows ──writes──┘
                                │
              scheduled flows: status check + e-mails
```

Full detail: [docs/architecture.md](docs/architecture.md)

## Documentation

| Doc | Content |
|---|---|
| [architecture.md](docs/architecture.md) | Components, data flow, design decisions |
| [data-model.md](docs/data-model.md) | SharePoint lists, columns, choice values |
| [screens.md](docs/screens.md) | Each screen: purpose, controls, logic |
| [flows.md](docs/flows.md) | Every Power Automate flow and its parameters |
| [deployment.md](docs/deployment.md) | Setup, import, configuration, admin access |
| [user-guide.md](docs/user-guide.md) | Step-by-step usage for staff |

## Repository layout

```
visa-alertes-waca/
├── README.md
├── docs/
├── src/            # unpacked canvas app (pac canvas unpack)
└── flows/          # exported flow solution (optional)
```

## Quick start

1. Create the SharePoint lists described in [data-model.md](docs/data-model.md).
2. Import the solution containing the app and flows.
3. Update the connections and the admin list ([deployment.md](docs/deployment.md)).
4. Share the app with your users.

## Known limitations

- Admin access is a hard-coded e-mail list in `HomeScreen.OnVisible`.
- The status flow handles about 100 records per run.
- `StaffRegistry` is maintained separately from Entra ID, so new arrivals are not synced automatically.

## Roadmap

- Stop expiry e-mails to residence-card holders.
- Editable data extraction per person.
- Auto-add staff members from Entra ID and alert when a new person appears.

## Ownership

Built and maintained by the Systems Implementation Support team, MSF WaCA.
