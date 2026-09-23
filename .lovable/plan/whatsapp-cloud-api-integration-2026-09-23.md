# WhatsApp Cloud API Integration

## Scope
Add WhatsApp as a production communication provider without changing existing modules or visual design. Keep Mock mode fully usable because the WhatsApp connection was not completed.

## Implementation
1. **Data and security foundation**
   - Extend clients with explicit opt-in, WhatsApp number/status, and last-message fields; default every existing/new client to opted out.
   - Add typed `whatsappMessages` records with deterministic business-event IDs, provider IDs, lifecycle timestamps, safe error fields, and no credentials.
   - Add authenticated staff read access and controlled request creation in Firestore rules; public users receive no WhatsApp access.
   - Add reusable India-aware phone normalization with configurable default country code and strict validation.

2. **Secure provider and backend**
   - Keep the browser-safe provider interface and Mock provider; move the real WhatsApp provider into the isolated Firebase Functions package.
   - Configure `WHATSAPP_ACCESS_TOKEN`, phone-number ID, business-account ID, verify token, Graph API version, and template names only as server-side function secrets/configuration.
   - Add authenticated Firebase callable functions for connection testing, test sends, and controlled retries. Verify the caller’s Firebase identity before every action.
   - Use approved template messages for invoice, renewal, birthday, and follow-up workflows. Text sending exists only as a server capability for valid customer-service conversations, not as a generic bulk UI.
   - Translate missing configuration, invalid numbers, templates, rate limits, network failures, and provider errors into safe staff-facing messages.

3. **Idempotency and automation**
   - Use deterministic IDs for invoice, renewal, birthday, and follow-up messages; successful events cannot send twice.
   - Route the existing renewal and birthday engines through the selected communication mode (`mock` or `whatsapp`), enforce client opt-in, and preserve Mock behavior.
   - Update notification/activity records from real send results; failed WhatsApp configuration records a useful failure instead of crashing daily automation.

4. **Webhook and delivery tracking**
   - Add a server-only webhook handler with verification-token handshake and signed callback validation.
   - Persist every callback before processing, deduplicate deliveries, and update `queued → sent → delivered → read` without allowing older callbacks to downgrade status.
   - Store Meta failure details safely and reconcile callbacks that arrive before the outbound provider ID is saved.
   - Keep webhook deployment/configuration isolated from the frontend and document the final callback URL/setup needed after deployment.

5. **Existing UI integrations**
   - Add Settings → WhatsApp with connection state, non-secret identifiers/version, communication mode, centralized template names/languages, backend connection test, and authenticated test message.
   - Add explicit client opt-in management, normalized WhatsApp number, status, last message, and defined-template send action on the client profile.
   - Add “Send via WhatsApp” to invoice actions and invoice-success handling using the existing public invoice URL.
   - Add approved-template WhatsApp action to follow-ups.
   - Extend Notifications with responsive desktop/mobile WhatsApp logs and real statuses; add real dashboard metrics only when WhatsApp records exist, otherwise show “WhatsApp not connected.”

6. **Verification**
   - Verify typecheck/build, authenticated access, mock invoice/renewal/birthday/follow-up workflows, opt-out blocking, deterministic duplicate prevention, controlled retry, responsive light/dark layouts, and no browser-visible secrets.
   - Real Meta delivery, provider message IDs, and live webhook callbacks remain pending until a WhatsApp Business connection/credentials and Firebase Functions deployment access are supplied.
