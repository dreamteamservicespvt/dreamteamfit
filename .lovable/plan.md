# Real-World Gym Workflow Update

Extends the existing Firebase app. No rebuild; all existing records are kept.

## Phase 1 — Data foundation
- New collections: `ptPackages`, `trainers`, `ptAssignments`, `payments`, `enrollments`, `trainerPayouts`, `manualIncome`, `importBatches`.
- Gym packages gain `category` (Strength + Cardio / Strength Only / Cardio Only / Custom); admins set names and prices.
- Trainer share (percentage or fixed) set per trainer, with an optional override per assignment. A snapshot is stored on each payment and assignment (ptPrice, share type/value, trainer amount, gym amount).
- Enrollment states: draft → payment_completed → biometric_pending → active (or cancelled).

## Phase 2 — New Member enrollment wizard
- One guided modal (a sheet on mobile) with 7 steps: Client → Package → PT (off by default; PT package and trainer required when on) → Payment → Invoice → Biometric → Complete.
- Going back keeps what you entered. Checkout summary shows every line, discount, tax, total, paid, balance and method.
- Confirming creates, in one step: client, membership (biometric_pending), PT assignment, one payment, invoice, trainer payout record. The PDF and public link are created automatically.
- Invoice step: Send via WhatsApp (configured provider), Open WhatsApp (manual share link) and Copy link. The phone number comes from the client form.
- Biometric step: pick device, see the assigned ID, then "Register First Thumb". The member is activated only when the real adapter confirms. If no hardware is configured: "Biometric hardware integration is not configured", the member stays pending and access stays blocked. The existing mock adapter is labeled development-only and never activates members.
- A "Complete Biometric Registration" action is available on the client profile.

## Phase 3 — Billing reconciliation
- Billing uses the payment as its source of truth. Paid invoices show "Paid ₹X" with no amount field. Partial invoices offer only "Record payment" for the remaining balance, which adds a payment row instead of duplicating revenue.
- Reports, dashboard and finance all read from `payments`.

## Phase 4 — Leads & Follow-ups (merged)
- One page with tabs: All, New, Contacted, Interested, Follow-up Due, Expected to Join, Converted, Lost.
- Structured follow-up recording: customer response, next action, next call date and time, expected visit and join dates, priority, notes. Timeline entries are added, never overwritten.
- "Convert to Member" opens the enrollment wizard pre-filled with the lead's details.
- Old /inquiries and /follow-ups redirect to the new page.

## Phase 5 — Packages, Trainers, Sessions
- Packages page tabs: Gym Packages, PT Packages, Trainers (create/edit/deactivate/search/filter).
- Sessions & Classes group: PT Sessions (linked to PT assignment and trainer; statuses scheduled/completed/cancelled/no-show), Group Classes (seats available, overbooking blocked), Bookings.

## Phase 6 — Finance
- Expenses page tabs: Income (system-generated from payments plus clearly labeled manual entries), Expenses, Trainer Payouts (pending/paid/cancelled).
- Summary: Gross Collections, Gym Income (membership plus the gym's PT share), Trainer Payable, Expenses, Net Gym Income. Trainer share is never counted as gym income.

## Phase 7 — Import & Export
- Settings → Data Import & Migration: upload CSV/XLSX, pick type (Packages, Trainers, Clients, Memberships, Expenses), map columns with auto-suggestions, preview and validate. Duplicate clients (matched by phone) get Skip/Update/Create New. You can download an error report, then confirm to import. Each import records a batch audit entry.
- Export Clients, Memberships, Invoices, Payments, Expenses and Attendance to CSV/XLSX.

## Phase 8 — Navigation, dashboard, quick actions
- Sidebar in the exact requested order, with a collapsible "Sessions & Classes" group. Management: Settings.
- Quick actions: New Member (wizard), Create Inquiry, Create Follow-up, Create Bill, Book PT/Class, Add Expense.
- Dashboard metrics all come from live data: collections, outstanding, PT sessions and pending biometrics.

## Verification
- Typecheck, then Playwright at 360/390/768/1280 for the wizard, billing reopen, leads, import and finance. Test cases 1–7 are run with the mock adapter to confirm no false activation.

## Technical notes
- `xlsx` (SheetJS) is used for import and export. Multi-document writes use Firestore transactions or batches. Rules are extended for the new collections (authenticated staff).
- Real eSSL/ZKTeco enrollment needs a local bridge service or device SDK. It stays blocked until that hardware configuration is supplied.
