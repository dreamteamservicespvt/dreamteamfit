# Bookings, PT Sessions & Group Classes

## Scope
Add only the scheduling foundation requested, preserving all existing Rebuild Fitness modules, branding, themes, shell, and responsive patterns. Firestore remains the source of truth and all new records remain staff-authenticated.

## Data foundation
- Add typed `Booking`, `GroupClass`, and `ClassEnrollment` models with the requested statuses and snapshot fields.
- Add Zod schemas with trimmed text, length limits, valid date/time formats, positive capacity, and end-after-start validation.
- Add `bookings`, `groupClasses`, and `classEnrollments` services with realtime subscriptions and authenticated Firestore rules.
- Use validated trainer name entry because no trainer directory exists yet. Derive a normalized stable `trainerId` from the entered name and preserve `trainerNameSnapshot`.
- Keep booking and enrollment history: cancellation and attendance outcomes update status rather than deleting records.

## Booking integrity
- Create and edit PT/general/group-class bookings through one responsive form.
- Before saving PT bookings, query same-date scheduled bookings and reject overlapping trainer or client time ranges.
- Recheck conflicts inside a Firestore transaction before writing to reduce race conditions.
- For group-class scheduling, the class record owns its schedule and capacity; enrollments own seat usage. The Bookings agenda combines ordinary bookings with scheduled group classes without inventing counts.

## Pages and workflows
- Add `/bookings` with Today, Upcoming, Calendar, and List views. Desktop gets a calendar/list hybrid; mobile gets an agenda/card layout.
- Add `/pt-sessions`, sourced only from `/bookings` where `bookingType = pt`, with requested filters and status/edit actions.
- Add `/group-classes` with realtime cards/tables, create/edit/cancel/view/manage-member actions, booked and remaining seat counts.
- Add class details as a responsive drawer with member phone data, enrollment statuses, enroll/cancel/attended/no-show actions.
- Capacity-safe enrollment uses a Firestore transaction, blocks duplicate active enrollment and full classes, and immediately reflects cancellation.

## Existing product integration
- Add Bookings, PT Sessions, and Group Classes to existing navigation and global search where appropriate.
- Add functional Create Booking actions in Dashboard and Quick Add.
- Add a Bookings tab to Client Profile showing upcoming/past bookings, PT sessions, group-class enrollments, and Create Booking.
- Add real dashboard counts for scheduled PT sessions, scheduled group classes, and today's schedule plus a genuine empty state.

## Verification
- Typecheck and inspect generated routes without editing the generated route tree.
- Exercise create/edit/status, overlap rejection, class enrollment/capacity/cancellation, profile visibility, dashboard updates, and refresh persistence against available Firebase access.
- Check desktop and mobile layouts for overflow and touch usability in both light and dark modes.
