# Attendance & Biometric Integration

## Goal
Add real Firestore-backed attendance, biometric member setup, device management, a safe mock scanner, and hardware-ready integration boundaries without changing the existing Rebuild Fitness design or rebuilding current modules.

## What will be built

### Data foundation
- Extend clients with biometric user ID, assigned device, and `not_enrolled` / `active` / `disabled` status.
- Add typed `/attendance` records and `/biometricDevices` records with only operational identifiers and metadata—never fingerprint images or templates.
- Add validation, realtime subscriptions, client attendance history, device CRUD/status updates, and authenticated-only Firestore access.
- Enforce unique biometric IDs per device and idempotent imported events using a stable device-event reference.

### Access and hardware architecture
- Build a UI-independent access-decision service that reads authoritative membership dates and returns the required allow/block reasons.
- Define `BiometricAdapter` and `AccessController` contracts.
- Add honest eSSL and ZKTeco placeholder adapters that report “Hardware adapter not configured.”
- Add working mock adapter and mock access controller for demonstrations without claiming a physical lock or device connection.
- Add an idempotent synchronization service that normalizes adapter logs, matches biometric IDs, evaluates access, writes attendance, and updates `lastSyncAt`.

### Attendance workspace
- Replace the Attendance placeholder with live totals, today’s attendance, currently present members, access logs, full history, search, period/date filters, and device/access/event/source filters.
- Add responsive desktop tables and mobile cards with genuine empty, loading, offline, and error states.
- Add Manual Attendance and Simulate Scan workflows.
- Show clear access granted/denied results while always recording blocked attempts.

### Device management
- Add a Biometric Devices page with create/edit/view, enable/disable, status, connection metadata, last sync, and adapter-backed connection tests.
- Keep network fields optional until real hardware details are available.
- Add navigation and search access without changing the existing shell.

### Client, dashboard, and reports integration
- Add Biometric Access and Attendance sections to client profiles, including assignment/toggle controls, live access decision, last visit, total visits, and history.
- Connect dashboard metrics to real today visits, currently present, and blocked attempts; show a trend only when enough real history exists.
- Replace the Reports attendance unavailable state with real attendance totals and export-ready rows.

## Technical details
- Event records: `clientId`, client/device snapshots, biometric ID, event type, local attendance date, timestamp, source, access decision/reason, optional note, and unique raw event reference.
- Presence is derived from each member’s latest allowed event today; a later check-out removes them.
- Manual attendance remains available regardless of hardware status and is marked `source = manual`.
- Simulator supports a selected client or unknown biometric ID so active, expired/no-membership, disabled, and member-not-found cases are testable.
- Firestore writes use deterministic IDs for imported events where possible, making repeated synchronization safe.

## Verification
- Test active membership, expired/no membership, disabled biometric access, unknown biometric ID, manual attendance, duplicate event prevention, refresh persistence, device fallback messaging, client history, dashboard/report updates, desktop/tablet/mobile layouts, and light/dark themes.
- Run type checks and inspect current preview diagnostics before completion.

## Out of scope
- Real eSSL/ZKTeco SDK or protocol connection until the exact model and network details are supplied.
- Physical fingerprint enrollment, fingerprint/template storage, real EM-lock activation, WhatsApp, birthday automation, and renewal automation.
