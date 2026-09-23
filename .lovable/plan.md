# Expenses, Reports & Profit/Loss Foundation

## Scope
Add Expenses and turn the existing Reports destination into a real analytics dashboard without redesigning Rebuild Fitness or implementing Billing, Attendance, exports, or automation.

## Expense data and integrity
- Add the authenticated `/expenses` collection with typed categories, payment methods, creator identity, timestamps, and validated positive amounts.
- Derive `createdBy` from the signed-in staff account rather than accepting editable identity input.
- Add realtime create, edit, delete, and audit events. Expense deletion remains confirmation-gated; audit history records the action without retaining invented financial values.
- Compute today, week, month, and year totals only from expense documents.

## Expenses experience
- Add a responsive Expenses page with summary cards, search, date/category/payment filters, desktop table, mobile cards, action menu, details drawer, and create/edit form.
- Use existing currency formatting, Rebuild theme tokens, dialogs, tables, empty/loading/error states, and mobile sheet behavior.
- Keep all filter controls usable at phone widths without page-level overflow.

## Reports foundation
- Replace the placeholder Reports page with sections for Revenue, Expenses, Profit/Loss, Memberships, Clients, Inquiries, Attendance, Bookings, Workout Plans, and Diet Plans.
- Add Today, This Week, This Month, This Year, and Custom date periods using shared date-range helpers.
- Build an expense category chart from real Firestore totals and real metric grids for memberships, inquiries, bookings, and assignment data.
- Show explicit unavailable states for Billing, Profit/Loss, and Attendance; never estimate revenue or profit.
- Return report sections through reusable, export-ready metric and row structures so CSV/PDF adapters can be added later without rewriting calculations.

## Dashboard and activity
- Add Today's Expenses and Monthly Expenses from realtime Firestore documents.
- Add Profit/Loss as unavailable until Billing exists.
- Surface genuine expense created, edited, and removed audit events in Recent Activity.
- Add Expenses to navigation and Add Expense to existing quick-action entry points.

## Verification
- Typecheck and confirm generated routing.
- Test expense create/edit/delete, live totals, search and filters, report calculations, dashboard integration, and refresh persistence with the available staff account.
- Check desktop/mobile and light/dark layouts for overflow, clipped controls, and accurate empty states.
