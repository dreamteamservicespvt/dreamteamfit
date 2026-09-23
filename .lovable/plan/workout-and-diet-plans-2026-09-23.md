# Workout and Diet Plans

## Goal
Add complete Firestore-backed Workout Plans and Diet Plans modules to the existing Rebuild Fitness application, including client assignments, history, dashboard counts, and responsive workflows without redesigning existing screens.

## Implementation
1. **Typed data and security**
   - Add workout plan, diet plan, workout assignment, and diet assignment domain types and collection constants.
   - Add shared Zod validation used by forms and service boundaries, with length, numeric, date, status, and reference validation.
   - Extend the existing authenticated-only Firestore rules to the four new collections without weakening current access.

2. **Workout Plans**
   - Build a responsive page using existing cards, filters, sheets, dialogs, loading/error/empty states, and confirmation patterns.
   - Implement real-time search/listing, create, edit, view, activate/deactivate, and safe delete.
   - Prevent deletion when historical workout assignments reference the plan.

3. **Diet Plans**
   - Build the matching responsive page in the existing visual language.
   - Implement real-time search/listing, create, edit, view, activate/deactivate, and safe delete.
   - Prevent deletion when historical diet assignments reference the plan.

4. **Client assignments and history**
   - Add Workout and Diet tabs to the existing client profile.
   - Add assignment dialogs using only active plans, validated dates and notes, and immutable plan snapshots.
   - Auto-calculate workout end dates from duration; allow explicit diet end dates.
   - Preserve all prior assignments, close the previous active assignment when a replacement becomes active, and show current plus history with cancel/complete actions.

5. **Navigation and dashboard**
   - Add both modules to the existing desktop/mobile drawer navigation and supported global search.
   - Add working dashboard quick actions that open each creation workflow.
   - Add real-time “Workout Plans Assigned” and “Diet Plans Assigned” counts derived from active assignments, plus real assignment activity.

6. **Verification**
   - Run type and runtime checks.
   - Verify create/edit/toggle/delete safety, assignment snapshots/history, refresh persistence, dashboard updates, authenticated access, light/dark mode, and no overflow across mobile and desktop widths.

## Out of scope
No Billing, Attendance, biometric hardware, WhatsApp, or automation work.
