# Follow-ups & Retention Automation

## Goal
Add the retention workflow to the existing Rebuild Fitness application without redesigning or rebuilding existing modules. All records use Firestore; WhatsApp remains an unconfigured future provider.

## Build
1. **Data and safety foundation**
   - Add typed models, validation, collection registrations, Firestore mappings, authenticated rules, and reusable date helpers using `Asia/Kolkata`.
   - Add `/followups`, `/renewalNotifications`, `/birthdayNotifications`, `/notifications`, `/automationActivities`, and `/settings/automation` support.
   - Use deterministic IDs and transactions for inquiry links, renewal reminders, birthday greetings, notification projections, and automated renewal follow-ups.

2. **Follow-up CRM**
   - Replace the Follow-ups placeholder with live Today, Overdue, Upcoming, and Completed views, search, filters, mobile cards, desktop rows, loading/error/empty/offline states, and actions for Complete, Reschedule, Edit, Call, and View Client.
   - Add create/edit and outcome workflow dialogs with free-form Other outcomes, notes, last contact, next action, optional next follow-up, membership context, and preserved history/activity.
   - Link inquiry `nextFollowUpDate` to one stable follow-up record without reload duplicates; preserve records after conversion.
   - Add Client Profile → Follow-ups with upcoming, overdue, and past history plus Create Follow-up.
   - Replace the sidebar count with real pending follow-ups due today or earlier.

3. **Communication and automation engine**
   - Add central editable template defaults and a `CommunicationProvider` interface with `MockCommunicationProvider`; keep a non-operational WhatsApp adapter boundary only.
   - Implement shared `processRenewalReminders()` and `processBirthdayNotifications()` paths that create customer-facing notification logs and automation activity.
   - Renewal processing will only consider active memberships expiring exactly seven days later, skip clients with later covering memberships, and use `membershipId + type + reminderDate` uniqueness.
   - Birthday processing will match DOB month/day and use `clientId + year + type` uniqueness.
   - Mock sends will queue and persist the rendered message; no external message API or credentials.

4. **Background schedule and demo controls**
   - Add an isolated Firebase scheduled-functions package that runs both checks daily in `Asia/Kolkata`, sharing equivalent automation logic and not affecting the frontend build.
   - Add discreet signed-in manual controls in Automation Settings that invoke the same frontend service functions for demos.
   - Document deployment as pending if Firebase CLI credentials or Functions project setup are unavailable.

5. **Retention UI integrations**
   - Add Birthdays with Today/Upcoming lists, photos, age when calculable, phone, membership status, Create Follow-up, and mock Send Greeting.
   - Add Notifications/Automation with Scheduled, Queued, Sent, Failed sections and type/status/date filters.
   - Add dashboard Birthday and Renewal sections using real data, including Expiring Today, Expiring in 7 Days, and Expired drill-down lists.
   - Add real follow-up, renewal, birthday, and failure events to dashboard activity; connect navigation, quick actions, global search, and responsive layouts.

6. **Verification**
   - Typecheck and inspect preview build health.
   - Test create, due/overdue, complete, reschedule, sidebar badge, inquiry association, and client history.
   - Run renewal and birthday checks twice and verify exactly one queued mock event each.
   - Verify no WhatsApp requests or credentials, and test light/dark modes plus 360/390/414/768px and desktop layouts without overflow.

## Technical Notes
- Existing Firebase Authentication and authenticated-only Firestore access remain unchanged.
- Scheduled execution is isolated from browser code because the browser cannot provide reliable daily background processing.
- Signed-in staff can access the discreet demo triggers because the current application has no role model; no insecure client-side admin claim will be introduced.
- Existing inquiry, membership, client, billing, attendance, and other module behavior remains intact.
