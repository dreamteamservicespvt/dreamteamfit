# Billing, POS & Invoice System

## Scope
Build the real billing workflow without changing existing modules or visual design. Firestore remains the source of truth; invoices are permanent financial records and no destructive invoice deletion will be added.

## Data and integrity
- Add typed invoice, line-item, payment, public-invoice, and business-billing-settings models with strict validation and centralized currency/tax calculations.
- Add realtime invoice subscriptions for all invoices and per-client history.
- Create readable `INV-{year}-{sequence}` numbers with a Firestore transaction on the existing settings/counter path, preventing duplicate numbers and duplicate submission.
- For package sales, create the membership and invoice together in the transaction, preserving package/client snapshots and closing prior active membership according to the existing history-safe behavior.
- Generate a long cryptographically random token and write a separate `/publicInvoices/{token}` projection containing only customer-facing fields. Internal invoice IDs, staff identity, internal notes, and other management data stay private.
- Extend Firestore rules so authenticated staff manage invoices/settings/public projections, while anonymous users may fetch one public invoice by its unpredictable token but cannot list records or access internal collections.

## Billing and POS experience
- Replace the Billing placeholder with a responsive invoice workspace: collection summaries, recent invoices, search by invoice/client/phone, date/status/payment filters, desktop table, mobile cards, and safe view/download/copy/print actions.
- Add a responsive Create Bill flow for client selection, package or custom line items, quantity, discount, configured tax, payment, dates, notes, live totals, membership option, and an explicit final confirmation.
- Add controlled invoice correction for non-identity fields while retaining invoice history; payment status and balances remain derived and validated.
- Add live business billing settings for name, logo, address, phone, email, GSTIN, tax toggle/rate, invoice prefix, and INR currency default. Unconfigured fields remain blank rather than invented.

## PDF and public invoice
- Generate a real A4 PDF with a Worker/browser-compatible JavaScript PDF library, branded logo, pagination-safe item rows, totals, payment details, and footer.
- Upload PDF bytes to Firebase Storage, save the stable download URL on both the private invoice and public projection, and support retry if upload fails after invoice creation.
- Build the public `/invoice/$token` route outside the authenticated shell with mobile-first printable light presentation, PDF download, and print actions.
- Add reusable `getInvoicePublicUrl(invoice)` for copy-link now and future WhatsApp integration, with no WhatsApp calls or credentials.

## Existing-product integration
- Client Billing tab: totals, latest invoice, history, view/download/copy actions, and Create Bill prefilled with that client.
- Membership dialog: optional “Generate Invoice” path that opens billing with the selected client/package instead of duplicating client details.
- Dashboard: today’s collection, month collection, total collected revenue, outstanding amount, Profit/Loss from collected revenue minus expenses, and invoice activity.
- Reports: period-filtered Gross Sales, Amount Collected, Outstanding, Refunded, and real Profit/Loss; preserve export-ready report structures.
- Add invoice results to global search and make Create Bill functional in dashboard/topbar quick actions.

## Verification
- Typecheck and inspect build/runtime logs.
- Authenticated end-to-end test: package membership + invoice creation, discount/tax behavior, recorded payment, number/token uniqueness, realtime Billing/Dashboard/Reports/Client updates, copy/print/download, and refresh persistence.
- Anonymous/private-browser test of the public token page and PDF download; verify internal data is absent.
- Visually inspect the generated PDF page-by-page for clipping, margins, typography, logo, totals, and multi-page behavior.
- Verify 360/390/414/768/1280 layouts, no horizontal overflow, and staff light/dark themes while public invoices remain print-clean.
