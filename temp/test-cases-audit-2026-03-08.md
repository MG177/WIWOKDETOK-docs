# Test Cases for Doc-Sync Audit (2026-03-08)

This document contains test cases derived from the 54 discrepancies identified in the `analysis-doc-sync-audit-2026-03-08.md` report. These tests are intended to verify the alignment between the documentation (PRD and feature specs) and the current code implementation.

---

## A. Cross-Cutting / PRD / Global Docs

### TC-01: Push Notifications (Web Push API)
- **Description**: Verify push notifications use Web Push API and Supabase Realtime.
- **Preconditions**: Authenticated user.
- **Steps**: Trigger a notification event (e.g., a followed promise is updated).
- **Expected Result**: A Web Push notification is received in the browser.
- **Actual Result**: Relies on cron/email-style notifications; Web Push is unimplemented.

### TC-02: Route Guards for Chat
- **Description**: Verify that `/chat` and `/chat/:sessionId` restrict anomalous guest access.
- **Preconditions**: Unauthenticated (Guest) user.
- **Steps**: Navigate directly to `/chat` and `/chat/123`.
- **Expected Result**: User is redirected to an authentication prompt/modal.
- **Actual Result**: Chat routes are guest-reachable.

### TC-03: Documentation Structure (Profile)
- **Description**: Verify `docs/feature-profile.md` naming convention.
- **Preconditions**: Access to repository files.
- **Steps**: Check `docs/` hierarchy.
- **Expected Result**: Profile doc is named `docs/features/feat-profile.md` to match others.
- **Actual Result**: File is named `docs/feature-profile.md`.

---

## B. Promise Tracker (`F-001`)

### TC-04: Tracker Heading Branding
- **Description**: Verify the main heading of the Promise Tracker page.
- **Preconditions**: Navigate to the Promise Tracker feature.
- **Steps**: Observe the main header text.
- **Expected Result**: Header reads "The Talk Ledger".
- **Actual Result**: Header reads "Promise Ledger".

### TC-05: Deep Search/Filter Capabilities
- **Description**: Verify that search queries apply across all documented fields.
- **Preconditions**: Navigate to Promise Tracker.
- **Steps**: Enter a search query matching a `politician_role` or `region_id`.
- **Expected Result**: Promises matching the role or region string are returned.
- **Actual Result**: Search only targets `quote`, `politician_name`, and `watchdog_commentary`.

### TC-06: Guest "Submit New Promise" Flow
- **Description**: Verify the authentication guard on the "Submit New Promise" button.
- **Preconditions**: Unauthenticated user on Promise Tracker.
- **Steps**: Click "Submit New Promise".
- **Expected Result**: An Auth modal is presented immediately.
- **Actual Result**: Opens submission modal; fails only on final submission step.

### TC-07: Promise Flag Admin Alert
- **Description**: Verify that accumulating >=10 flags on a promise alerts an admin.
- **Preconditions**: A single promise.
- **Steps**: Have 10 distinct test users flag the promise as BS.
- **Expected Result**: An admin alert or notification is triggered in the system.
- **Actual Result**: Only inserts/deletes `promise_reactions`; no threshold handler.

### TC-08: Moderate User-Submitted Promises
- **Description**: Verify pending queue and submission notifications.
- **Preconditions**: User submits a new promise successfully.
- **Steps**: Admin approves or rejects the pending promise in the database.
- **Expected Result**: The submitting user receives an approval/rejection notification.
- **Actual Result**: Status is set to 'pending', but no UI or notification flow exists.

### TC-09: Pending Submission Banner
- **Description**: Verify that a pending promise displays an "Under review" banner.
- **Preconditions**: User has a promise in the 'pending' state.
- **Steps**: View the promise in the UI.
- **Expected Result**: A banner reads: "Under review — we'll notify you when it's live."
- **Actual Result**: No such banner exists.

### TC-10: Comment Pagination
- **Description**: Verify that comments load in batches of 20.
- **Preconditions**: A promise has > 20 associated comments.
- **Steps**: Open the comment section for the promise.
- **Expected Result**: Only the first 20 comments are rendered, with a "Load more" option.
- **Actual Result**: All comments are fetched and rendered at once.

### TC-11: Region-Only Empty State
- **Description**: Verify empty state UX when filtering by a region with no promises.
- **Preconditions**: Filter by a valid region that has 0 tracked promises.
- **Steps**: Observe the empty state prompt.
- **Expected Result**: Reads "No promises tracked in this region yet." + "[Explore National]" button.
- **Actual Result**: Generic "No promises found matching your filters." + "Clear Filters" button.

### TC-12: Load More Button Spinner
- **Description**: Verify UI feedback when fetching more promises.
- **Preconditions**: A view with paginated promises.
- **Steps**: Tap the "Load More" button.
- **Expected Result**: A spinner appears on the button while fetching.
- **Actual Result**: Button text changes to "Loading..." but no spinner is rendered.

### TC-13: Flag as BS Backend Rule
- **Description**: Verify the flag rule handles increments optimally.
- **Preconditions**: Authenticated user.
- **Steps**: Flag a promise as BS.
- **Expected Result**: Database increments flag count and flags >=10 alert admin (duplicate of TC-07 context).
- **Actual Result**: Rule is unimplemented on the backend.

### TC-14: Comment Moderation Auto-Hide
- **Description**: Verify that comment flags decrement visibility.
- **Preconditions**: A comment has >= 5 flags.
- **Steps**: View the comment thread.
- **Expected Result**: System automatically hides it and increments flags organically via user actions.
- **Actual Result**: UI renders the hidden state, but no backend flow increments flags or auto-hides.

### TC-15: Confirm Unfollow Tooltip
- **Description**: Verify unfollow confirmation UI.
- **Preconditions**: User follows a promise.
- **Steps**: Tap "Following" button.
- **Expected Result**: A true tooltip component appears to confirm unfollow.
- **Actual Result**: Shows a transient inline pill instead of a tooltip.

### TC-16: Source-404 UX Combination
- **Description**: Verify 404 source fallback UI styling.
- **Preconditions**: A promise has a dead/404 source link.
- **Steps**: View the promise.
- **Expected Result**: Only the footnote is shown as per documentation.
- **Actual Result**: Both a "Source Unavailable" badge and footnote are shown (Code ahead of docs).

### TC-17: Feed Error State (Synced)
- **Description**: Verify feed handles offline/error states gracefully.
- **Preconditions**: Cause a deliberate fetch error on the promise feed.
- **Steps**: Observe UI.
- **Expected Result/Actual**: Shows "Couldn't load promises... Retry" button. 

### TC-18: Paywalled Source (Synced)
- **Description**: Verify paywall fallback text.
- **Preconditions**: A promise source is flagged as paywalled.
- **Steps**: View the promise.
- **Expected Result/Actual**: "Source behind paywall — see AI summary below."

### TC-19: 404 Footnote Copy (Synced)
- **Description**: Verify exact text of 404 footnote.
- **Preconditions**: Promise with dead link.
- **Steps**: View footnote.
- **Expected Result/Actual**: Reads "Original source no longer available."

### TC-20: Follow Notifications (Deferred)
- **Description**: Deferred to backlog.md. No immediate action.

---

## C. Bang Jaga (`F-002`)

### TC-21: Strict Auth for Bang Jaga
- **Description**: Verify Bang Jaga restricts access entirely for guests.
- **Preconditions**: Unauthenticated User.
- **Steps**: Navigate to `/chat`.
- **Expected Result**: Redirected to auth modal, then deep-linked back to `/chat` post-login.
- **Actual Result**: `/chat` is completely guest-reachable.

### TC-22: Session Routing Initialization
- **Description**: Verify `/chat` vs `/chat/:sessionId` behavior.
- **Preconditions**: Authenticated user with existing sessions.
- **Steps**: Navigate to `/chat` manually.
- **Expected Result**: Starts a fresh, new session.
- **Actual Result**: Auto-loads the most recent existing session. `/chat/:sessionId` is a non-functional placeholder.

### TC-23: Session Ownership Security
- **Description**: Verify session endpoints authorize the current user ownership.
- **Preconditions**: User A owns Session X. User B is authenticated.
- **Steps**: User B attempts a `sendMessage` or `getSessionMessages` request targeting Session X.
- **Expected Result**: Request is rejected (unauthorized).
- **Actual Result**: Ownership is not verified; request may succeed.

### TC-24: True RAG Legal Search
- **Description**: Verify Bang Jaga leverages "Search Laws" mode with citations.
- **Preconditions**: Active chat session.
- **Steps**: Ask for the law regarding public infrastructure.
- **Expected Result**: Triggers true RAG retrieval, outputs citations, and shows a Legal Context panel.
- **Actual Result**: Fake/simulated retrieval; no Legal Context panel rendered.

### TC-25: RAG Law-to-Law Feedback
- **Description**: Verify rule corrections capture proper `userSelectedLawId`.
- **Preconditions**: Active chat session returning a legal citation.
- **Steps**: Submit feedback correcting the citation to a different law.
- **Expected Result**: Saves `originalLawId` and `userSelectedLawId` to `rag_feedback`.
- **Actual Result**: Records free-text against message content instead.

### TC-26: Multimodal Image Context in RAG
- **Description**: Verify Gemini receives actual image parts for analysis.
- **Preconditions**: Active chat session.
- **Steps**: Upload a photo of a broken road and ask "Does this violate regulation X?"
- **Expected Result**: Image is passed via Gemini Multimodal API parts.
- **Actual Result**: Gemini is only told in text that "1 image attachment" was uploaded.

### TC-27: Region-Scoped Retrieval
- **Description**: Verify RAG search factors in the user's region.
- **Preconditions**: User profile set to West Java. Let active chat query local bylaws.
- **Steps**: Check the retrieval call to the database.
- **Expected Result**: Passes region parameter to narrow down legal embeddings.
- **Actual Result**: Region is ignored during retrieval.

### TC-28: Independent Document Streaming
- **Description**: Verify chat SSE stream and `generateSuratPengaduan` stream operate correctly.
- **Preconditions**: Chat session reaching a document generation phase.
- **Steps**: Monitor network traffic during document production.
- **Expected Result**: Chat streams via SSE. Document generation uses a separate debounced stream.
- **Actual Result**: Server returns a monolithic block; client simulates typewriter effect.

### TC-29: Preview & PDF Template Parity
- **Description**: Verify the generated complaint looks identical in UI Preview and PDF export.
- **Preconditions**: Generate a complaint in Bang Jaga.
- **Steps**: Compare the UI preview block against the downloaded PDF.
- **Expected Result**: Exact 1:1 parity (from the same canonical payload).
- **Actual Result**: Discrepancies exist due to separate template wrappers.

### TC-30: Quick-Copy WA Toast
- **Description**: Verify WhatsApp sharing triggers a confirmation instead of redirect.
- **Preconditions**: Generated complaint available.
- **Steps**: Click "Quick-Copy WA".
- **Expected Result**: Copies text to clipboard and shows "Copied for WhatsApp!" toast.
- **Actual Result**: Opens a new tab to `wa.me` with no copy toast.

### TC-31: AI Session Titles & History Groups
- **Description**: Verify chat history sidebar organization.
- **Preconditions**: Multiple past sessions spanning days/weeks.
- **Steps**: Open session sidebar.
- **Expected Result**: Titles are context-aware (AI-generated) and grouped by "TODAY", "PREVIOUS 7 DAYS".
- **Actual Result**: Titles are static timestamps; list is flat.

### TC-32: Strict Attachment Limits
- **Description**: Verify attachment validation boundaries.
- **Preconditions**: Active chat session.
- **Steps**: Attempt to upload 6 files, or a PDF, or files aggregating >20MB per session.
- **Expected Result**: Rejected based on doc rules (max 5 files/session, image only, 20MB total limits).
- **Actual Result**: Limits are per-batch, accepts any image MIME type, no session-total cap enforced.

### TC-33: Offline/Unavailable Fallback
- **Description**: Verify UX when Gemini or network is unavailable.
- **Preconditions**: Turn off network connection or simulate API failure.
- **Steps**: Attempt to send a message.
- **Expected Result**: Offline banner appears, send is disabled, specific retry state is shown.
- **Actual Result**: Fails silently or shows generic error; no banner.

### TC-34: Hallucination Fallback
- **Description**: Verify Bang Jaga declines to answer if no legal grounding exists.
- **Preconditions**: Ask an obscure, non-legal question.
- **Steps**: Evaluate Ai response.
- **Expected Result**: Explicitly states no laws match the query; refuses to invent statutes.
- **Actual Result**: Generates convincing but fake statutory text.

### TC-35: Complaint Artifact Persistence
- **Description**: Verify `ExportPdf(sessionId)` retrieves the saved artifact.
- **Preconditions**: Generate a complaint, leave page, come back to `/chat/:sessionId`.
- **Steps**: Attempt to export or share the complaint.
- **Expected Result**: Fetches from persistent `generated_documents` schema linked to `sessionId`.
- **Actual Result**: Ignored; `shareComplaintToMap` drops the `sessionId`.

### TC-36: Bang Jaga Environment Variables
- **Description**: Verify documentation accurately reflects codebase env vars.
- **Expected Result**: Docs and Code align on `NEXT_PUBLIC_SUPABASE_URL`.
- **Actual Result**: Docs refer to backend-only `SUPABASE_URL`.

### TC-37: Feature Flag Validation
- **Description**: Verify `FEATURE_BANG_JAGA_SAVE_HISTORY` flag behavior.
- **Expected Result**: Flag governs history persistence.
- **Actual Result**: Flag is unused in current code.

---

## D. Walk-o-Meter (`F-003`)

### TC-38: Report Score Recalculation
- **Description**: Verify that a new report dynamically alters the promise score.
- **Preconditions**: Promise X has a score. Submit a new verified report with a photo.
- **Steps**: Monitor the `recalculatePromiseScores()` effect on the Promise table.
- **Expected Result**: Integrates `official_progress_score` and weights verified citizens higher.
- **Actual Result**: Does not recalculate immediately; photo boolean logic is buggy (`|| true`); ignores official score.

### TC-39: Single Editable Vote per Promise
- **Description**: Verify users can only vote once per promise, editable for 24 hours.
- **Preconditions**: User submits a report on Promise X.
- **Steps**: Submit *another* report on Promise X.
- **Expected Result**: Edits the previous report (if <24h) or rejects it.
- **Actual Result**: Inserts infinite duplicate reports per user.

### TC-40: Vote Starting Point
- **Description**: Verify verification entry point resides on the promise details page.
- **Preconditions**: Viewing a promise trace.
- **Steps**: Look for the Walk-o-meter submission button.
- **Expected Result**: Evidence submission originates here.
- **Actual Result**: Missing entirely from `src/app/promise-tracker/page.tsx`.

### TC-41: Community Trust (Verify Evidence)
- **Description**: Verify community verification operates on the *reports*, not the promise.
- **Preconditions**: See an existing report on the `/feed`.
- **Steps**: Click "Verify Evidence".
- **Expected Result**: Increments `verification_count` affecting `trust_tier` of the target report.
- **Actual Result**: Opens original submission modal targeting the *promise_id*, creating a sibling report.

### TC-42: Guest Read-Only Maps/Feeds
- **Description**: Verify report/verify buttons are hidden from guests.
- **Preconditions**: Unauthenticated user.
- **Steps**: Visit `/map` and `/feed`.
- **Expected Result**: "Submit Report" and "Verify Evidence" buttons are invisible.
- **Actual Result**: Buttons are visible; failing only on action execution.

### TC-43: Realtime Polling UI
- **Description**: Verify live data updates.
- **Preconditions**: Map is open.
- **Steps**: A background process or another user submits a report nearby.
- **Expected Result**: A pill appears: "New posts available" (Realtime/polling).
- **Actual Result**: Page is entirely static post-load.

### TC-44: Data Saver Mode Implementation
- **Description**: Verify "Data Saver" reduces load costs correctly.
- **Preconditions**: Turn on Data Saver mode.
- **Steps**: Load `/map` and `/feed`.
- **Expected Result**: 30% image quality, reduced map tiles, paused polling, persistent banner.
- **Actual Result**: Just hides map layer completely; no banner.

### TC-45: Public Location Privacy
- **Description**: Verify raw GPS coordinates are blurred to public viewers.
- **Preconditions**: Inspect API payloads for `getMapReports()`.
- **Steps**: View map or feed API response.
- **Expected Result**: Returns region/sub-region poly centers (not explicit lat/long).
- **Actual Result**: Exposes exact user GPS `latitude` and `longitude`.

### TC-46: Region Slug vs ID Mismatch
- **Description**: Verify filtering cleanly hits region domains.
- **Preconditions**: Submit a report assigned to region ID `west-java`.
- **Steps**: Filter feed using slug `jawa-barat`.
- **Expected Result**: Feed shows the report.
- **Actual Result**: Filter fails due to ID vs Slug mismatches.

### TC-47: Map to Feed State Persistence
- **Description**: Verify map viewport context survives navigation to feed.
- **Preconditions**: Map zoomed to DKI Jakarta. Look around.
- **Steps**: Click "Explore All Reports".
- **Expected Result**: Navigates to `/feed` with DKI Jakarta filter pre-selected.
- **Actual Result**: Navigates to a global/unfiltered feed. 

### TC-48: Map Leaderboard Context
- **Description**: Verify regional leaderboard rendering.
- **Preconditions**: Map zoomed/focused on a city.
- **Steps**: View the right sidebar.
- **Expected Result**: Displays top 3 performing districts for that specific city.
- **Actual Result**: Uses global stats; no specific sidebar context exists on map.

### TC-49: Original Promise Fallback Link
- **Description**: Verify safe navigation for orphaned verification cards.
- **Preconditions**: Viewing a report in the feed where the parent promise was deleted.
- **Steps**: Check link to promise.
- **Expected Result**: Renders text "Original promise no longer available."
- **Actual Result**: Unconditionally links to the root `/promise-tracker`.

### TC-50: Draft Report Persistence
- **Description**: Verify drafts for offline/failed report submissions.
- **Preconditions**: Make a submission fail (e.g., offline) via the Walk-o-meter standard flow.
- **Steps**: Check Profile → Engagement History.
- **Expected Result**: The draft is saved and retrievable.
- **Actual Result**: Drafts do not exist; data is lost on standard flows.

### TC-51: Complaint to Report Linkage Model
- **Description**: Verify `complaint_id` foreign keys exist when making reports.
- **Preconditions**: Bang jaga "Share to Map" workflow.
- **Steps**: Share a complaint. Inspect the resulting `WalkOMeterReport` dataset.
- **Expected Result**: Links back via a distinct `complaint_id`.
- **Actual Result**: Feature completely ignores the linkage requirement.

### TC-52: Controlled Vocabulary for Tags
- **Description**: Verify report categorization logic.
- **Preconditions**: AI suggests tags for a report.
- **Steps**: Inspect the generated tags against the predefined list of 10 tags.
- **Expected Result**: The tags are rigidly selected only from the predefined list.
- **Actual Result**: Uses unsafe free-form JSON generation, accepting any hallucinated tags.

### TC-53: Report Resolution Status
- **Description**: Verify `resolved` status documentation vs code.
- **Preconditions**: Inspect `src/lib/types.ts` vs docs.
- **Expected Result**: Both sources align on available states (e.g. `pending | accepted | rejected`).
- **Actual Result**: Code defines a `resolved` status that doesn't exist in the specs (Code ahead of docs).

### TC-54: Visual Trust Tiers on Map/Feed
- **Description**: Verify visual indicators for report trust layers.
- **Preconditions**: View reports with different vote counts/flags on the map.
- **Steps**: Assess pin colors and layer weights.
- **Expected Result**: Distinct visual styling strictly denotes trust-tiers as specified in specs.
- **Actual Result**: A large chunk of trust-tier visual rules are missing from the current UI.
