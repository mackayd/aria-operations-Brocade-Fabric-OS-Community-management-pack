# Brocade Fabric OS Community Management Pack for VMware Aria Operations

Community VMware Aria Operations management pack for monitoring Dell Connectrix DS-6620B/Brocade G620 and Dell Connectrix DS-7720B/Brocade G720 Fibre Channel switches directly through the Fabric OS REST API.

Built with Aria Management Pack Builder, this pack collects fabric, switch, port, connected-endpoint, transceiver, sensor, fan, and power-supply resources without requiring SANnav. It includes Fabric OS session authentication, 35 numeric metrics, 62 domain properties, eight topology relationships, five event-based alert definitions, and ten interactive Brocade SAN dashboards.


> This is an independent community management pack, not an official Dell, Broadcom, or VMware-supported product. Validate it in a non-production environment before wider deployment. Read the validation status and known limitations before installing it.

## Release

| Item | Value |
| --- | --- |
| Management pack | Brocade Fabric OS Aria Operations Community Management Pack |
| PAK version | `1.0.0.8` |
| MP Builder design version | `1.0.0` |
| Repository maintainer | [Drew Mackay / mackayd](https://github.com/mackayd) |
| Author recorded in Builder export | `Drew Mackay` |
| Source type | Aria Management Pack Builder HTTP adapter |
| Builder used | Version `1.1.2`, build `22157532` |
| Aria Operations reference | `8.18.3`, build `24521408` |
| API | Fabric OS JSON REST API under `/rest` |
| Authentication | Fabric OS REST login/session authorization/logout |
| Domain resource kinds | 8, in addition to the adapter instance |
| Numeric metric definitions | 35 |
| Domain properties | 62 |
| Explicit domain relationships | 8 |
| Event-based alert definitions | 5 |
| Dashboards | 10 shared Brocade SAN dashboards |
| Suggested starting interval | 5 minutes; tune after observing switch and collector load |

Release `1.0.0.8` promotes three live-tested operational dashboards—port-performance rankings, guided troubleshooting, and alert-driven topology—into the production dashboard set. Development-only names and the `Brocade-dev-dashboards` folder are not included in the packaged content.

## Contents

- [Repository Contents](#repository-contents)
- [Release Artifacts](#release-artifacts)
- [Requirements and Compatibility](#requirements-and-compatibility)
- [Installing the PAK](#installing-the-pak)
- [Adapter Instance Settings](#adapter-instance-settings)
- [What It Monitors](#what-it-monitors)
- [Topology Model](#topology-model)
- [Alerts](#alerts)
- [Included Dashboards](#included-dashboards)
- [Fabric OS REST Collection](#fabric-os-rest-collection)
- [Importing the MP Builder Project](#importing-the-mp-builder-project)
- [Dashboard Development Workflow](#dashboard-development-workflow)
- [Validation Status](#validation-status)
- [Known Limitations](#known-limitations)
- [Troubleshooting](#troubleshooting)
- [Repository Security Notes](#repository-security-notes)

## Repository Contents

```text
.
├── readme.md
├── PAK-Release
│   └── brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak
└── mp-builder
    ├── brocade-fabricos-ariaops-mpb-project-1.0.0.json
    └── brocade-fabricos-ariaops-dashboards-1.0.0.8.json
```

This repository contains the deployable management pack and its editable exports. It does not install or configure Fabric OS, provision SAN fabrics, create switch users, deploy SANnav, or provide a VMware Aria Operations appliance.

## Release Artifacts

| Artifact | Purpose |
| --- | --- |
| [brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak](PAK-Release/brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak) | Install into Aria Operations. Contains the adapter, resource definitions, relationships, event mappings, and ten dashboards. |
| [brocade-fabricos-ariaops-mpb-project-1.0.0.json](mp-builder/brocade-fabricos-ariaops-mpb-project-1.0.0.json) | Import into Management Pack Builder to inspect or edit the complete HTTP-adapter design. All ten production dashboards are embedded as custom content. |
| [brocade-fabricos-ariaops-dashboards-1.0.0.8.json](mp-builder/brocade-fabricos-ariaops-dashboards-1.0.0.8.json) | Separate Aria dashboard export for dashboard-only inspection, import, recovery, and development. It is not an adapter project or a replacement for the PAK. |

Only one PAK variant is supplied. The repository does not include a separate custom-icon build.

Dashboard owner UUIDs are environment-specific Aria metadata. The standalone dashboard export and MP Builder content can therefore carry different `userId` values while retaining the same production dashboard IDs, names, widgets, interactions, and metric configuration.

### SHA-256 Checksums

| File | SHA-256 |
| --- | --- |
| `brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak` | `B26F48662FC56DF8FD873ADE4FCF2C9EF90BD92CD78E5CC0FF14D049266EDA18` |
| `brocade-fabricos-ariaops-mpb-project-1.0.0.json` | `613D95392F0DD8F8EC5B148D25A185A1983CB534CA58C929769CBDF12B1F97E7` |
| `brocade-fabricos-ariaops-dashboards-1.0.0.8.json` | `E236A237FD8F8ED114A27110511AC4C593F3AC6540CCEB38E47164F0371D9379` |

To verify all supplied artifacts in PowerShell, run this from the repository root:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath `
  '.\PAK-Release\brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak', `
  '.\mp-builder\brocade-fabricos-ariaops-mpb-project-1.0.0.json', `
  '.\mp-builder\brocade-fabricos-ariaops-dashboards-1.0.0.8.json'
```

These checksums identify the supplied files; they are not a vendor signature or certification.

## Requirements and Compatibility

### Reference Environment

| Component | Version used during development | Qualification |
| --- | --- | --- |
| VMware Aria Operations | `8.18.3`, build `24521408` | Collector installation, collection, topology, metrics, events, and dashboard behaviour tested in the lab. |
| Aria Management Pack Builder | `1.1.2`, build `22157532` | Used to maintain the design, embed the production dashboard content, and build PAK `1.0.0.8`. |
| Dell Connectrix DS-6620B / Brocade G620 profile | Switch type `162.0`, emulated FOS `v9.2.2c1` | Emulator and document validated; physical switch validation pending. |
| Dell Connectrix DS-7720B / Brocade G720 profile | Switch type `181.5`, emulated FOS `v10.0.1` | Emulator and document validated; physical switch validation pending. |

These are recorded development versions, not a guarantee of compatibility with every older or newer Aria Operations or Fabric OS release.

### Supported API Boundary

- Fabric OS 9.0.x–9.2.x JSON REST is the common target for G620 and G720.
- Fabric OS 10.0.x JSON REST is a G720 target. DS-6620B/G620 cannot load Fabric OS 10.
- Fabric OS 8.2 REST payloads are XML. This release does not map the FOS 8.2 XML response model and therefore does not claim FOS 8.2 collection support.
- The design requests `application/yang-data+json` and the normal kebab-case Fabric OS field names.
- Optional or licence-dependent data can be absent. Name Server, media diagnostics, and some MAPS/system-resource fields must be checked on the intended firmware and hardware.

### Aria Operations

- An Aria Operations environment in which you are permitted to install community management packs.
- A collector or cloud proxy that can resolve and reach each switch management endpoint.
- Permission to create adapter accounts and credentials and to view collected inventory, alerts, metrics, and dashboards.
- Management Pack Builder only if you plan to inspect, modify, or rebuild the design; it is not required to run an installed PAK.

### Fabric OS and Network Access

- An existing DS-6620B/G620 or DS-7720B/G720 switch with JSON REST available.
- HTTPS reachability from the selected Aria Operations collector to the switch, normally TCP `443`.
- Working DNS and a switch certificate suitable for the hostname or IP entered in the adapter account.
- A dedicated Fabric OS identity that can log into REST and read the running-status endpoints listed below.

Fabric OS exposes several REST-capable roles, including `user`, `operator`, `switchadmin`, and `admin`. The exact minimum role for every selected endpoint has not been proven on physical hardware. Begin with a dedicated least-privilege monitoring identity and increase permissions only when a documented endpoint returns an authorization failure.

## Installing the PAK

1. Download the PAK from `PAK-Release` and verify its SHA-256 checksum.
2. Sign in to Aria Operations with management-pack installation permissions.
3. Open the integrations repository, typically **Administration > Integrations > Repository** in Aria Operations 8.x.
4. Add or upload `brocade-fabricos-ariaops-community-management-pack-1.0.0.8.pak`.
5. Review the package details, packaged terms, unsigned-package warning, and your organisation's policy before proceeding.
6. Complete installation and wait for the repository workflow to finish.
7. Open the integration accounts page and add one account per directly reachable Fabric OS switch.
8. Configure the switch hostname, credential, collector, TLS policy, and collection interval, then run **Test Connection**.
9. Save the account, allow collection to run, and validate Inventory, Relationships, Metrics, Alerts, and Dashboards.

Menu labels vary between Operations releases. Broadcom documents the administrative PAK workflow in [KB 395605](https://knowledge.broadcom.com/external/article/395605). If your appliance rejects an unsigned community PAK, review the exact error and your organisation's policy before changing signature enforcement; see [Broadcom's unsigned-management-pack guidance](https://knowledge.broadcom.com/external/article/409626).

### Upgrading an Existing Installation

- Export any dashboards you edited locally before upgrading. Packaged content replacement can affect local modifications.
- Record the current account settings, collector assignment, collection interval, and credential selection.
- Install the newer PAK as an upgrade to the existing solution rather than creating duplicate accounts for the same switch.
- Confirm that existing accounts still collect and that objects, history, topology, and credentials remain intact.
- Review all ten dashboards after the upgrade, including object-selection interactions.

The adapter kind in this release is:

```text
mpb_brocade_fabric_os_aria_operations_community_management_pack
```

The installed collector baseline `1.0.0.6` was validated in the supplied lab. The final `1.0.0.8` PAK was built and passed archive, version, dashboard-equality, checksum, and credential scans, but a final install/upgrade of the packaged `1.0.0.8` bundle was not performed. Include both a fresh installation and an in-place upgrade in your acceptance testing.

## Adapter Instance Settings

Create one adapter account for each directly reachable switch management endpoint.

| Setting | Recommended starting value | Guidance |
| --- | --- | --- |
| Name | A unique fabric/switch account name | Make the monitored switch and environment obvious. |
| Hostname | Switch management FQDN or IP | Do not include `https://` or a REST path. |
| Port | `443` | Match the switch HTTPS listener. |
| SSL/TLS | Verify | Use a certificate whose trust chain and SAN match the configured hostname. Relax verification only for an explicitly accepted lab case. |
| API Base Path | Blank | This field exists for lab transport; leave it blank for a physical switch. |
| Connection timeout | `30` seconds | Increase only after confirming a genuine latency or switch-response issue. |
| Concurrent requests | `4` | Reduce for a constrained control processor; assess load before increasing. |
| Maximum retries | `2` | Avoid aggressive retry storms against a busy switch. |
| Minimum event severity | Warning | Review against your alerting policy. |
| Collection interval | `5` minutes | Start here and tune after observing scale, duration, and control-processor load. |

The `API Base Path` field was used by the development lab to expose two emulator profiles through one bridge. Values such as `apps/g620` and `apps/g720` are lab-only and must not be copied to a physical switch account.

### Credentials

| Field | Value |
| --- | --- |
| Username | Dedicated Fabric OS REST monitoring identity |
| Password | That identity's password, stored in an Aria Operations credential object |

The project export leaves credential values empty. Enter credentials through Management Pack Builder only for a temporary test session, or through the Aria Operations credential interface when creating an adapter account. Do not add credentials to the repository JSON.

### Test Connection and First Collection

After a successful connection test:

1. Save the account and confirm collection is enabled.
2. Allow at least one complete collection interval for discovery and additional intervals for chart history.
3. Check that the account reports successful collection without persistent authentication, TLS, parsing, or timeout errors.
4. Confirm that switch model, firmware, WWN, fabric, port, and hardware counts are plausible.
5. Check a representative port for line speed, traffic, error, and congestion metrics with current timestamps.
6. Confirm that ports, fans, sensors, power supplies, and transceivers are related to the expected parent switch.
7. Open the dashboards and select an object wherever a widget expects input.

A successful connection test proves basic connectivity and authentication; it does not prove every endpoint, relationship, metric, alert, or dashboard.

## What It Monitors

### Resource Kinds

| Resource | Primary identity | Expected parent/context |
| --- | --- | --- |
| Adapter account | Hostname plus API base path | None |
| Fibre Channel fabric | Fabric-friendly name | Adapter account; shared across switch accounts when identity matches |
| Fibre Channel switch | Switch WWN | Fabric and adapter account |
| Fibre Channel port | Parent-switch WWN plus port WWN | Switch and adapter account |
| Power supply | Parent-switch WWN plus serial | Switch and adapter account |
| Fan | Parent-switch WWN plus serial | Switch and adapter account |
| Environmental sensor | Parent-switch WWN plus slot/index/category | Switch and adapter account |
| FC transceiver | Parent-switch WWN plus serial | Switch, matching port, and adapter account |
| Connected FC endpoint | Port WWN | Matching switch port and adapter account |

The lab validated a mixed synthetic fabric in which one fabric resource was shared by the G620 and G720 adapter accounts.

### Numeric Metrics

| Resource | Metrics | Units |
| --- | --- | --- |
| Port traffic | Line speed, in/out octets, in/out frames, in/out bit rate, in/out frame rate | bit/s, byte, count, frames/s |
| Port integrity and congestion | CRC errors, invalid transmission words, invalid ordered sets, link failures, loss of signal/sync, Class 3 discards, PCS block errors, FEC uncorrected, BB-credit zero, input-buffer-full, state transitions, congestion primitives | count |
| Power supply | Power production, input voltage, temperature, power usage | W, V, °C |
| Fan | Speed, power consumption | rpm, W |
| Sensor | Temperature and Fahrenheit temperature | °C, °F |
| Transceiver | Nominal bit rate, temperature, voltage, transmit power, receive power | Mbit/s, °C, V, dBm |

Port byte, frame, error, failure, credit, buffer-full, and discard values are cumulative counters where Fabric OS supplies counters. Use their change over time rather than interpreting the absolute total as a current rate. `Port In Rate`, `Port Out Rate`, and the frame-rate metrics are the instantaneous rate signals.

### Properties

- **Fabric:** fabric identity and local member WWN.
- **Switch:** WWN, display and fabric names, model, firmware, domain ID, FCID, VF ID, principal/enabled flags, operational status, chassis identity, and serial.
- **Port:** parent switch, WWN, slot/number, display name, type, protocol speed, enabled/operational/physical/health states, POD licence state, and neighbour identity.
- **Power supply and fan:** parent switch, serial, unit, part number, state, source, and airflow where applicable.
- **Sensor:** parent switch, ID, slot, index, category, and state.
- **Transceiver:** parent switch, serial, port, identifier, connector, vendor, and part number.
- **Endpoint:** port and node WWNs, fabric-port WWN, symbolic names, port ID, and port type.

## Topology Model

The management pack defines these domain relationships:

1. Fabric contains switch.
2. Switch contains port.
3. Switch contains power supply.
4. Switch contains fan.
5. Switch contains environmental sensor.
6. Switch contains transceiver.
7. Port contains its matching transceiver.
8. Port connects a Name Server endpoint.

Component identity includes the parent-switch WWN because native component serial/index values are not assumed to be globally unique. ISL neighbour properties are collected, but the pack does not create a cross-account switch-to-switch relationship because the evaluated responses do not expose a sufficiently complete peer resource identity in every case.

## Alerts

All five supplied definitions are event based and use Fabric OS state rather than invented universal performance thresholds.

| Alert | Resource | Warning input | Critical input |
| --- | --- | --- | --- |
| Fabric OS Switch Operational State | Switch | `undefined`, `testing` | `disabled` |
| Fabric OS Port Health | Port | `marginal` | `faulty` |
| Fabric OS Power Supply Health | Power supply | — | `faulty` |
| Fabric OS Fan Health | Fan | — | `faulty` |
| Fabric OS Environmental Sensor Health | Sensor | `marginal` | `faulty` |

Healthy states are discarded. Operators can add policy symptoms for utilization, error, or congestion rates after observing real baselines for their fabrics.

## Included Dashboards

All dashboards are packaged under **Brocade Fabric OS**.

| Dashboard | Primary use |
| --- | --- |
| Brocade SAN Overview | Health and navigation across all Brocade resource kinds |
| Brocade Fabric and Switch Summary | Fabric membership, switch health, and switch operational context |
| Brocade Port Traffic | FC bit rates, frame rates, and cumulative traffic totals |
| Brocade Port Errors and Congestion | CRC/FEC/PCS errors, link stability, credit starvation, buffer-full events, and Class 3 discards |
| Brocade Port Performance Top 10 | Port inventory plus top-ten traffic, link, error, congestion, discard, buffer, and FEC rankings |
| Brocade Hardware Environment | Sensors, optics, power supplies, and fans |
| Brocade Alerts and Health | Object selection, active alerts, related objects, and health/risk history |
| Brocade Alerts and Topology | Active Brocade alerts driving topology, health history, and alert volume |
| Brocade Health Investigation | Object-first investigation with alerts, topology, metric selection, and charts |
| Troubleshooting Brocade SAN | Numbered workflow through health, alerts, related-object health, and a SAN scoreboard |

The port-performance dashboard uses a resource table plus eight Pareto rankings. Troubleshooting uses a selected SAN object to drive health, alerts, related objects, and a consolidated scoreboard. Alerts and Topology uses an active alert as the provider for the selected object's topology and health widgets.

If a dashboard appears empty, first select a compatible object, allow multiple collection cycles to populate history, and confirm that the selected resource kind supplies the requested metric. A blank active-alert list can be correct when no Brocade alert is active.

### Dashboard-Only Import

The separate dashboard JSON can be imported without replacing the adapter:

1. Open **Operations > Dashboards > Manage**.
2. Open the more-actions menu and select **Import**.
3. Choose `mp-builder/brocade-fabricos-ariaops-dashboards-1.0.0.8.json`.
4. Choose **Overwrite** when replacing dashboards from this management pack, or **Rename** for a side-by-side test.
5. Confirm all ten definitions under **All > Brocade Fabric OS**.

Dashboard-only import changes dashboard content only. It does not install or upgrade the adapter, change its version, or create switch accounts.

## Fabric OS REST Collection

Each normal collection session performs a bounded login, eleven read-only GET requests, and logout:

| Endpoint | Purpose |
| --- | --- |
| `POST /rest/login` | Establish session authorization |
| `GET /rest/running/brocade-fibrechannel-switch/fibrechannel-switch` | Local switch identity and state |
| `GET /rest/running/brocade-fabric/fabric-switch` | Fabric membership |
| `GET /rest/running/brocade-interface/fibrechannel` | FC port identity and state |
| `GET /rest/running/brocade-interface/fibrechannel-statistics` | Port traffic, error, and congestion counters |
| `GET /rest/running/brocade-name-server/fibrechannel-name-server` | Connected FC endpoints |
| `GET /rest/running/brocade-fru/power-supply` | Power supplies |
| `GET /rest/running/brocade-fru/fan` | Fans |
| `GET /rest/running/brocade-fru/sensor` | Environmental sensors |
| `GET /rest/running/brocade-media/media-rdp` | Transceiver diagnostics |
| `GET /rest/running/brocade-maps/switch-status-policy-report` | MAPS health roll-up |
| `GET /rest/running/brocade-maps/system-resources` | CPU, memory, flash, and kernel-resource information used by the design |
| `POST /rest/logout` | Release the session |

Login uses the configured Basic credential. Fabric OS returns a session authorization value, which is used for the data requests and logout. The project export contains no username, password, session authorization value, certificate, or browser state.

## Importing the MP Builder Project

1. Sign in to a compatible Aria Management Pack Builder instance.
2. From the designs page, import `mp-builder/brocade-fabricos-ariaops-mpb-project-1.0.0.json`.
3. Review the design name, source settings, eleven requests, eight object definitions, eight relationships, five event mappings, and ten content entries.
4. Configure temporary test credentials and a non-production G620/G720 endpoint or emulator.
5. Test the login/session flow and each request before running a complete collection preview.
6. Confirm resource identities, metrics, relationships, and event mappings before building a new PAK.
7. Export the design again after any accepted change and verify that credential values remain null before committing it.

### Version Numbers and Rebuild Warning

The editable design version is `1.0.0`. MP Builder combines that design version with its build counter to name generated packages. The supplied export records build number `9` because the stored counter advanced after build `1.0.0.8` was produced. A new build from the export will therefore not be byte-for-byte identical to the supplied `1.0.0.8` PAK and may receive the next build suffix.

The distributed PAK is also the result of verified post-build corrections required by the tested topology:

- `API Base Path` participates in adapter-instance identity so two logical lab targets on one host/port remain distinct.
- The adapter-instance identifier is informational for fabric identity so one fabric can be shared across switch accounts.
- Relationship submission runs every collection rather than every eighth collection.
- The dashboard payload is normalized into both the root and nested PAK locations.

Treat the supplied PAK as the authoritative release artifact. If you rebuild the project, revalidate those identity, relationship, dashboard, version, and archive conditions before distributing the result.

## Dashboard Development Workflow

The ten production dashboards are embedded in both the MP Builder project export and the PAK. The separate dashboard JSON is the safer starting point for dashboard-only changes:

1. Import or clone dashboards into a clearly named development folder.
2. Use temporary development names so production dashboards are not overwritten while testing.
3. Validate every widget against live collected Brocade resource kinds and metrics.
4. Verify interactions after selecting fabric, switch, port, optic, sensor, fan, PSU, and endpoint objects as applicable.
5. Export the accepted definitions and remove development-only names and folders before packaging.
6. Confirm that the MP Builder project content and standalone dashboard export contain the same production definitions.

## Validation Status

| Check | Result |
| --- | --- |
| G620 emulator login, collection, and logout | PASS |
| G720 emulator login, collection, and logout | PASS |
| Simultaneous mixed-model fabric topology | PASS |
| Installed collector baseline `1.0.0.6` | PASS |
| G620 port-fault alert activation and recovery | PASS |
| G720 temperature-fault alert activation and recovery | PASS |
| Original eight-dashboard import and live interactions | PASS |
| Three new `1.0.0.8` operational dashboards in the development folder | PASS |
| MP Builder design content: ten dashboards and zero content/design errors | PASS |
| MP Builder build `1.0.0.8` | PASS |
| Final PAK CRC, version metadata, root/nested dashboard equality, and checksums | PASS |
| Supplied-secret and private-key scan | PASS, zero findings |
| Final `1.0.0.8` production PAK install/upgrade | NOT PERFORMED |
| Packaged ten-dashboard production JSON import | NOT PERFORMED |
| Physical G620/G720 validation | NOT PERFORMED |

The lab installed collector baseline discovered 78 G620-profile resources and 79 G720-profile resources. It produced one shared fabric, two switches, 128 ports, four connected endpoints, six transceivers, four environmental sensors, four power supplies, and five fans across the two accounts. These are deterministic emulator results, not expected counts for every physical fabric.

Evidence classifications used by the project:

- **EMULATOR-VALIDATED:** demonstrated through the deterministic emulator, MP Builder, and installed Aria Operations lab.
- **DOCUMENT-VALIDATED:** derived from the cited Dell, Broadcom, and VMware/Broadcom material.
- **REQUIRES REAL HARDWARE VALIDATION:** exact physical-switch responses, privileges, TLS behaviour, scale, timing, and production load.

## Known Limitations

- No physical DS-6620B/G620 or DS-7720B/G720 was available during development.
- FOS 8.2 XML REST responses are not supported by this JSON design.
- G720 switch types `181.0` and `181.5` have documented layout differences; the emulator validates the `181.5` interpretation.
- Optional and licence-dependent Fabric OS fields can be absent and require validation on the intended FOS build and hardware.
- Name Server endpoints are Fibre Channel login objects, not authoritative vSphere inventory correlation.
- ISL neighbour properties are collected, but a cross-account switch-to-switch relationship is not created.
- The PAK is unsigned and intended for community/lab acceptance before production use.
- Counter dashboards display cumulative counts; use change over time rather than the absolute value as the operational signal.
- The supplied Aria Operations 8.18.3 lab ignored `activeOnly=true` in its alerts API; inspect alert status when automating active-alert queries.
- During earlier `1.0.0.7` testing, the lab Repository uploader failed at 100% for both the new PAK and an untouched installed-package control. Dashboard JSON import succeeded. The final `1.0.0.8` PAK was not resubmitted to that uploader.
- The packaged `1.0.0.8` production dashboard JSON was not re-imported after the accepted development dashboards were promoted.

## Troubleshooting

### Test Connection Fails

- Confirm Fabric OS REST is enabled and reachable from the selected collector.
- Verify the hostname, TCP port, DNS resolution, and certificate SAN/trust.
- Confirm the credential can perform `POST /rest/login` and read the selected `/rest/running` endpoints.
- Leave `API Base Path` blank for a physical switch.
- Check whether Fabric OS session limits have been exhausted by other clients.

### No Objects or Stale Metrics

- Check the adapter account state and last collection heartbeat.
- Confirm the switch returns JSON `application/yang-data+json`; FOS 8.2 XML payloads are not mapped.
- Review collection duration, timeout, retry, and concurrency settings.
- Allow more than one complete collection interval after installation or account creation.
- Validate a representative endpoint outside Aria Operations with an approved read-only tool before changing the management pack.

### Topology Is Incomplete

- Confirm the current PAK is installed and not a raw, uncorrected Builder build.
- Confirm component resources contain `parent_switch_wwn` and their switch exposes the matching WWN.
- Allow at least two complete collections after account creation or upgrade.
- Check optional media and Name Server endpoints before assuming a missing transceiver or endpoint relationship is a matching failure.

### Alerts Do Not Clear

- Verify that the source Fabric OS state has returned to a healthy value.
- Allow the configured event cancellation cycle to complete.
- Inspect the alert `status`; the reference lab's API ignored `activeOnly=true`.

### Dashboards Are Empty

- Confirm the dashboard is under **Brocade Fabric OS** and was not imported under an unexpected renamed folder.
- Select an object where the widget is provider driven.
- Confirm the object kind has the requested metric and that history has been collected.
- Treat an empty active-alert list as valid when the environment has no active Brocade alerts.
- If using dashboard-only recovery, verify all ten definitions were imported and resolve overwrite/rename conflicts deliberately.

## Repository Security Notes

- Use a dedicated least-privilege Fabric OS REST credential.
- Prefer certificate verification and a switch certificate whose SAN matches the configured hostname.
- Keep switch, Aria Operations, and Management Pack Builder credentials outside the repository and shell history.
- The MP Builder export contains null credential values.
- The release artifacts were scanned for the supplied lab passwords, private-key markers, and nested archive content with zero findings.
- Verify SHA-256 values before installation; the community PAK is unsigned.
- Do not publish captures containing real WWNs, hostnames, IP addresses, session headers, or credentials without appropriate anonymization and review.

No passwords, session tokens, private keys, sensitive certificates, or browser/session stores are included in the three repository artifacts.

## Primary Documentation

- [Dell DS-6620B manuals](https://www.dell.com/support/product-details/en-us/product/connectrix-ds-6620b/resources/manuals)
- [Dell DS-7720B manuals](https://www.dell.com/support/product-details/en-us/product/connectrix-ds-7720b/resources/manuals)
- [Dell Connectrix B-Series Fabric OS 9.2.2c1 release notes](https://dl.dell.com/content/manual10732247-dell-connectrix-b-series-fabric-os-9-2-2c1-release-notes.pdf?language=en-us)
- [Dell Connectrix B-Series Fabric OS 10.0.1 release notes](https://dl.dell.com/content/manual9376958-dell-connectrix-b-series-fabric-os-10-0-1-release-notes.pdf?language=en-us)
- [Fabric OS 9.2.x REST API Reference](https://dl.dell.com/content/manual44115188-fabric-os-9-2-x-rest-api-reference-guide.pdf?language=en-us)
- [Fabric OS 10.0.0 REST API Reference](https://dl.dell.com/content/manual83856208-dell-connectrix-fabric-os-10-0-0-rest-api-reference-guide.pdf?language=en-us)
- [Broadcom Fabric OS documentation index](https://knowledge.broadcom.com/external/article/267270/fos-fabric-os-downloads-and-documentatio.html)

## Community Support and Contributions

Use the repository issue tracker for reproducible problems, enhancement proposals, and physical-hardware validation evidence. Include the Aria Operations version, PAK version, switch model/type, Fabric OS version, affected resource kind, and sanitized logs or payload shape. Never attach credentials, authorization headers, unredacted support bundles, or identifiable production captures.

Contributions should preserve the single-design support model for G620 and G720, avoid undocumented assumptions, keep credentials null in exported JSON, and clearly separate document, emulator, and real-hardware evidence.
