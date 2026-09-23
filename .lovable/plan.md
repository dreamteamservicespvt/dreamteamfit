# Rebuild Fitness brand and quality pass

## Goal
Apply the official Rebuild Fitness identity across the existing application, remove remaining demo business data, connect navigation counts to Firestore, and resolve responsive issues without changing the established architecture or feature scope.

## Implementation
1. **Official branding**
   - Store the uploaded logo as the canonical application asset and derive a correctly padded favicon from it.
   - Replace all FORGE, Ironline Fitness, Elevate, placeholder logo, metadata, settings, upload-folder, and confirmation-copy references with REBUILD FITNESS.
   - Reuse the exact logo with preserved proportions in login, desktop sidebar, mobile drawer/header, and settings.

2. **Black and yellow design tokens**
   - Update centralized light and dark theme tokens to vivid Rebuild yellow, deep charcoal, white, and neutral gray.
   - Keep green, red, orange, and blue only for semantic statuses.
   - Preserve existing layout and component structure while ensuring accessible text, button, focus, badge, and selected-state contrast.

3. **Real navigation and dashboard data**
   - Remove hardcoded inquiry and follow-up badges.
   - Add one shared real-time inquiry counter source: actionable inquiries exclude converted/lost; due follow-ups also require a follow-up date on or before today. Hide zero badges.
   - Remove fake notifications and unsupported quick actions rather than implying unavailable features work.
   - Keep dashboard structure, using Firestore clients/inquiries/memberships/packages for supported metrics and recent activity. Show explicit unavailable states for billing and attendance.
   - Ensure activity is derived only from persisted records; show a useful empty state otherwise.

4. **Supported global search**
   - Connect the existing top search to real Clients, Inquiries, and Packages data.
   - Show only matching persisted records, with keyboard-friendly navigation and a clear no-results state.

5. **Responsive polish**
   - Audit login, shell, navigation, dashboard, package/inquiry/client lists, client profile, forms, filters, dialogs, drawers, empty/loading/error states, and menus.
   - Preserve desktop tables and mobile cards, stack filter/action rows where necessary, enforce minimum touch targets, and remove horizontal overflow at 360–1920+ widths.

6. **Verification**
   - Confirm Firestore rules remain authenticated-only.
   - Use the provided test account only in the browser test; never store or display it.
   - Verify protected routes, login, persistence after refresh, navigation, logout, real badges, core data screens, client membership, and light/dark/system modes.
   - Inspect desktop 1280/1440/1920, tablet 768/1024, and mobile 360/390/414 for overflow, clipping, overlap, logo integrity, and contrast.

## Out of scope
No Billing, WhatsApp, biometric/attendance integration, or redesign of the existing product structure.
