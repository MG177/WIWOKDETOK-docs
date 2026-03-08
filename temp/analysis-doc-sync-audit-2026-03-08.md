# Documentation Sync Audit

Audit date: `2026-03-08`

Scope:

- `docs/PRD.md`
- `docs/README.md`
- `docs/features/feat-promise-tracker.md`
- `docs/features/feat-bang-jaga.md`
- `docs/features/feat-walk-o-meter.md`
- current implementation in `src/`

Method:

- Compared feature docs and PRD against current UI, server actions, route handlers, and shared types.
- Treated both "doc ahead of code" and "code ahead of docs" as mismatches.
- Included deferred notification work already moved into `backlog.md`.

---

## Executive Summary

The repo is partially synced, but there are still many concrete mismatches between implementation and documentation.

Most remaining drift clusters around:

- Promise Tracker moderation/submission details
- Bang Jaga auth/routing/RAG/session behavior
- Walk-o-Meter verification, realtime, privacy, and region handling

The largest concentration of drift is now in `docs/features/feat-bang-jaga.md` and `docs/features/feat-walk-o-meter.md`, where the docs describe a more complete system than the current code implements.

---

## Findings

### A. Cross-Cutting / PRD / Global Docs

1. `doc ahead of code`
   `docs/PRD.md` says the app uses `Web Push API (PWA) + Supabase Realtime` for push notifications, but the current repo only shows cron/email-style notification plumbing in `src/app/api/cron/notifications/route.ts` and `src/lib/emails.ts`. No concrete Web Push implementation was found in `src/`.

2. `doc ahead of code`
   `docs/PRD.md` lists `/chat` and `/chat/:sessionId` as `[AUTH]`, but current chat routes are still guest-reachable in code and do not enforce the documented auth-only route behavior.
   References: `docs/PRD.md`, `src/app/chat/page.tsx`, `src/app/chat/[sessionId]/page.tsx`

3. `doc ahead of code`
   `docs/PRD.md` refers to `docs/feature-profile.md` for profile details, but the repo still uses the older top-level naming there while the rest of the docs were normalized to `docs/features/feat-*.md`. This is not a runtime bug, but it is a documentation structure inconsistency.
   References: `docs/PRD.md`, `docs/feature-profile.md`, `docs/README.md`

### B. Promise Tracker (`F-001`)

4. `code ahead of docs`
   The actual page heading is still `Promise Ledger`, while the PRD and feature docs brand the feature as `The Talk Ledger`.
   References: `docs/PRD.md`, `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

5. `doc ahead of code`
   The docs say search/filter supports `region, politician, year, status`. The UI now exposes region/year/status controls, but the search text backend still only searches `quote`, `politician_name`, and `watchdog_commentary`. It does not search `politician_role` or `region_id`, despite the placeholder saying `Search Regions, Politicians, or Promises...`.
   References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`, `src/app/actions/promises.ts`

6. `doc ahead of code`
   The docs say guests tapping `Submit New Promise` should get the same auth modal. Current code opens `SubmitPromiseModal` for everyone and only fails later in `submitNewPromise()` with an error string if the user is not authenticated. There is no auth-guard modal for this action.
   References: `docs/features/feat-promise-tracker.md`, `docs/PRD.md`, `src/app/promise-tracker/page.tsx`, `src/components/SubmitPromiseModal.tsx`, `src/app/actions/promises.ts`

7. `doc ahead of code`
   `AC-07` says promise flags trigger an admin alert at `>=10` flags, but current code only inserts/deletes `promise_reactions`. There is no alerting or threshold handler for promise flags.
   References: `docs/features/feat-promise-tracker.md`, `src/app/actions/promises.ts`

8. `doc ahead of code`
   `AC-09` says user-submitted promises enter a pending moderation queue and notify the submitter on approval/rejection. Current code inserts `status: 'pending'`, but there is no approval flow, approval/rejection UI, or submitter notification path connected to promise submissions.
   References: `docs/features/feat-promise-tracker.md`, `src/app/actions/promises.ts`, `src/components/SubmitPromiseModal.tsx`

9. `doc ahead of code`
   The feature doc says pending submissions show a banner like `"Under review — we'll notify you when it's live."` There is no such banner anywhere in the current Promise Tracker UI.
   References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`, `src/components/SubmitPromiseModal.tsx`

10. `doc ahead of code`
    The docs say comments are `paginated at 20 per load`. `CommentThread` fetches all comments for a promise in one request and renders them directly with no pagination or load-more behavior.
    References: `docs/features/feat-promise-tracker.md`, `src/components/CommentThread.tsx`, `src/app/actions/promises.ts`

11. `doc ahead of code`
    The docs define a special empty state for region-only cases: `"No promises tracked in this region yet."` + `[Explore National]`. Current code only has one generic empty state: `"No promises found matching your filters."` + `Clear Filters`.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

12. `minor doc ahead of code`
    The docs say `Load More button tapped | Spinner on button`. Current implementation swaps button text to `Loading...`; it does not render a spinner.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

13. `doc ahead of code`
    The docs say authenticated users can `Flag as BS`, and current button text now matches that, but the deeper rule `"at ≥10 flags an admin alert is triggered"` is still unimplemented.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`, `src/app/actions/promises.ts`

14. `mostly synced, but partial`
    Comment moderation display now matches the docs better, because hidden comments or comments with `flag_count >= 5` render `"This comment has been removed for review."` However, the code still does not implement any flow that actually increments comment flags or auto-hides them.
    References: `docs/features/feat-promise-tracker.md`, `src/components/CommentThread.tsx`, `src/lib/types.ts`

15. `mostly synced, but partial`
    The docs say `Following` should support confirm-unfollow behavior. Current code now implements a second-tap confirmation hint, but it does not render a true tooltip component, only a transient inline pill.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

16. `code ahead of docs`
    The docs frame the source-404 behavior as a footnote, but current code now shows both a `Source Unavailable` badge and the footnote. This is acceptable UX-wise, but it is a docs mismatch.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

17. `synced`
    Feed error state now matches the docs: `"Couldn't load promises. Check your connection."` + `Retry`.
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

18. `synced`
    Paywalled source copy now matches the docs: `"Source behind paywall — see AI summary below."`
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

19. `synced`
    404 source footnote now matches the docs: `"Original source no longer available."`
    References: `docs/features/feat-promise-tracker.md`, `src/app/promise-tracker/page.tsx`

20. `synced`
    The notification feature for followed promises is now explicitly deferred into `backlog.md`.
    References: `backlog.md`, `docs/PRD.md`, `docs/features/feat-promise-tracker.md`

### C. Bang Jaga (`F-002`)

21. `critical, doc ahead of code`
    The docs say Bang Jaga is auth-only and guests are redirected to auth/deep-linked back. Current `/chat` and `/chat/[sessionId]` are guest-reachable and render chat UI instead of enforcing the documented auth-only route behavior.
    References: `docs/features/feat-bang-jaga.md`, `docs/PRD.md`, `src/app/chat/page.tsx`, `src/app/chat/[sessionId]/page.tsx`, `src/context/AuthGuardContext.tsx`, `src/components/AuthGuardModal.tsx`

22. `critical, doc ahead of code`
    The documented route model says `/chat` is a new session and `/chat/:sessionId` resumes a named session. Current `/chat` auto-loads the most recent existing session, the session list does not navigate to `/chat/[sessionId]`, and `/chat/[sessionId]` is only a placeholder screen.
    References: `docs/features/feat-bang-jaga.md`, `docs/PRD.md`, `src/app/chat/page.tsx`, `src/app/chat/[sessionId]/page.tsx`

23. `critical, doc ahead of code`
    Per-user session persistence is only partially guaranteed in the action layer. `getSessions()` is user-scoped, but `getSessionMessages(sessionId)` and `sendMessage(sessionId, ...)` do not explicitly verify session ownership.
    References: `docs/features/feat-bang-jaga.md`, `src/app/actions/chat.ts`

24. `high, doc ahead of code`
    The docs describe true RAG-grounded legal search with citations, `Search Laws` behavior, and a Legal Context panel. Current UI/API do not implement `searchLaws()`, do not expose a dedicated Search Laws mode, and do not render the documented Legal Context panel.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/components/ChatMessage.tsx`, `src/app/actions/chat.ts`

25. `high, doc ahead of code`
    The docs define `rag_feedback` as law-to-law correction (`originalLawId`, `userSelectedLawId`). Current implementation records free-text corrections against a `message_id` and message content instead.
    References: `docs/features/feat-bang-jaga.md`, `src/components/ChatMessage.tsx`, `src/app/actions/chat.ts`

26. `high, doc ahead of code`
    The docs say uploaded images are used as multimodal context. Current code uploads files but only tells Gemini that the user sent `"N image attachments"` in text form. No actual image bytes/parts are sent to Gemini.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/app/actions/chat.ts`

27. `high, doc ahead of code`
    Region-scoped legal retrieval is documented, but current regulation matching does not pass a region parameter into retrieval. Bang Jaga cannot actually narrow RAG by the user’s area as documented.
    References: `docs/features/feat-bang-jaga.md`, `docs/PRD.md`, `src/app/actions/chat.ts`

28. `high, doc ahead of code`
    The docs describe SSE chat streaming plus a separate/debounced `generateSuratPengaduan(sessionId)` stream. Current code gets one complete assistant response from `sendMessage()` and only simulates streaming client-side with a typewriter effect. Document preview is derived from hidden markers in that same response.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/app/actions/chat.ts`, `src/lib/complaint-utils.ts`

29. `high, doc ahead of code`
    The docs say preview and PDF always match because they come from the same canonical complaint payload. Current code uses the same `ComplaintDraft` object, but the preview and PDF renderer still have different surrounding templates/layout text, so the user-visible output is not actually identical.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/app/api/export-pdf/route.ts`

30. `medium, doc ahead of code`
    The docs say `Quick-Copy WA` copies a WhatsApp-formatted payload and shows `"Copied for WhatsApp!"`. Current code opens `https://wa.me/?text=...` in a new tab and shows no confirmation toast.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/lib/complaint-utils.ts`

31. `medium, doc ahead of code`
    The docs say session titles are AI-generated and history is grouped into `TODAY / PREVIOUS 7 DAYS / older months`. Current code creates static date-based titles and renders a flat sorted list.
    References: `docs/features/feat-bang-jaga.md`, `src/app/actions/chat.ts`, `src/app/chat/page.tsx`

32. `medium, doc ahead of code`
    Attachment validation does not match the docs. The docs say JPG/PNG/WEBP only, max 5 files per session, max 5MB each, max 20MB total. Current UI accepts `image/*`, enforces per-file size, and caps only the current send selection to 5. No total-session-size guard was found.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/lib/image-utils.ts`

33. `medium, doc ahead of code`
    The docs promise offline banner + disabled send and retry-style Gemini failure handling. Current code has no offline banner, no explicit offline send-block, and Gemini fallback behavior does not match the documented unavailable/retry states.
    References: `docs/features/feat-bang-jaga.md`, `src/app/chat/page.tsx`, `src/app/actions/chat.ts`

34. `medium, doc ahead of code`
    The docs say Bang Jaga should not invent regulations and should explicitly say when no match is found. Current fallback responses can still output authoritative-looking statutory text even when grounding fails.
    References: `docs/features/feat-bang-jaga.md`, `src/app/actions/chat.ts`

35. `medium, doc ahead of code`
    The docs describe `generated_documents`, `generateSuratPengaduan(sessionId)`, `exportPdf(sessionId)`, and persistent complaint artifacts. Current code does not persist generated documents in a way that matches that model, and `shareComplaintToMap()` ignores `sessionId`.
    References: `docs/features/feat-bang-jaga.md`, `src/app/actions/chat.ts`, `src/app/actions/reports.ts`, `src/lib/types.ts`, `src/app/chat/page.tsx`

36. `low, doc ahead of code`
    Bang Jaga env var docs still use `SUPABASE_URL` / `SUPABASE_ANON_KEY`, while the app uses `NEXT_PUBLIC_SUPABASE_URL` / `NEXT_PUBLIC_SUPABASE_ANON_KEY`.
    References: `docs/features/feat-bang-jaga.md`, `next.config.ts`, `src/lib/supabase.ts`, `src/lib/supabase/server.ts`

37. `low, doc ahead of code`
    Bang Jaga docs mention feature flags `FEATURE_BANG_JAGA_SAVE_HISTORY` and `FEATURE_PROMISE_LINK`, but no active use of those flags was found in current code.
    References: `docs/features/feat-bang-jaga.md`, `src/`

### D. Walk-o-Meter (`F-003`)

38. `critical, doc ahead of code`
    Score recalculation does not match the spec. `submitVerificationReport()` only inserts a report. It does not trigger immediate score recomputation, and `recalculatePromiseScores()` ignores `official_progress_score`, ignores verified-citizen weighting from the PRD, and contains `const hasPhoto = !!r.photo_url || true`, which makes photo validation ineffective.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/app/actions/reports.ts`

39. `critical, doc ahead of code`
    The documented `one vote per user per promise` rule with editable-for-24h behavior is not implemented. Current code inserts a fresh report every time and has no uniqueness/update/lock logic.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/actions/reports.ts`, `src/app/feed/page.tsx`, `src/components/VerificationModal.tsx`

40. `critical, doc ahead of code`
    The docs say the vote flow starts from a promise. Current `src/app/promise-tracker/page.tsx` has no verification entry point at all.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/promise-tracker/page.tsx`

41. `critical, doc ahead of code`
    The docs treat report trust/Ground Truth as community verification of evidence posts. Current `/feed` `Verify Evidence` flow just opens `VerificationModal` again and creates another `promise_verification` against `promise_id`, not a verification of the report itself. `verification_count` and `trust_tier` are displayed, but there is no write path that updates them.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/feed/page.tsx`, `src/components/VerificationModal.tsx`, `src/app/actions/reports.ts`

42. `doc ahead of code`
    Guest permissions are looser than documented. The docs say guests should see read-only map/feed with no vote/report buttons. Current guests still see `Submit Report` on `/map` and `/feed`, and still see `Verify Evidence` on `/feed`; only the verify action itself is auth-guarded.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`, `src/context/AuthGuardContext.tsx`

43. `doc ahead of code`
    The docs describe live updates via Realtime or polling and a `New posts available` pill. Current `/map` and `/feed` are static fetch-once pages with no polling, no realtime subscription, and no new-posts UI.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`, `src/app/actions/reports.ts`

44. `doc ahead of code`
    Data Saver behavior does not match the spec. The docs say 30% image quality, reduced-resolution tiles, paused live polling, and a persistent banner. Current code hides some images, removes the base map tile layer, has no banner, and has no polling to pause.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/app/feed/page.tsx`, `src/app/map/page.tsx`, `src/context/DataSaverContext.tsx`

45. `critical, doc ahead of code`
    Public location privacy is out of sync. The docs say public views should expose region-level location only, not raw GPS. Current `getMapReports()` returns raw `latitude` / `longitude`, `/map` renders exact pins, `/feed` shows `location_label` directly, and `ShareToMapModal` explicitly tells the user the exact pin is public.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/app/actions/reports.ts`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`, `src/components/VerificationModal.tsx`, `src/components/ShareToMapModal.tsx`

46. `high, doc ahead of code`
    Region filtering assumptions do not line up. Submission/share uses region IDs from `src/lib/regions.ts` like `west-java` and `dki-jakarta`, but `/feed` filters against slugs derived from GeoJSON names like `jawa-barat`. Many reports may not align with feed filters.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/lib/regions.ts`, `src/components/VerificationModal.tsx`, `src/app/chat/page.tsx`, `src/app/feed/page.tsx`

47. `doc ahead of code`
    The docs say `"Explore All Reports"` on `/map` should navigate to `/feed` with the current region pre-applied`. Current map-to-feed navigation does not carry current region state.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`

48. `high, doc ahead of code`
    Leaderboard behavior does not match the spec. The docs say the right sidebar shows top 3 districts in the selected City/Regency. `/map` has no leaderboard, and `/feed` uses a global `getLeaderboard()` with no region parameter.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`, `src/app/actions/reports.ts`

49. `doc ahead of code`
    The docs say verification cards should show a `"View Original Promise"` link and fallback to `"Original promise no longer available."` if the promise is gone. Current `/feed` links to generic `/promise-tracker`, not a specific promise, and if `promise_id` is null there is no fallback message.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/feed/page.tsx`, `src/app/promise-tracker/page.tsx`

50. `doc ahead of code`
    Draft/offline submission handling only partially exists. The docs say failed submissions are saved as drafts and accessible from Profile → Engagement History. Current verification flow has no draft save; Bang Jaga share-to-map uses only localStorage drafts; Profile does not surface those drafts.
    References: `docs/features/feat-walk-o-meter.md`, `docs/PRD.md`, `src/components/VerificationModal.tsx`, `src/app/chat/page.tsx`, `src/app/profile/page.tsx`, `src/app/actions/reports.ts`

51. `high, doc ahead of code`
    Complaint/report linkage does not match the data model in the docs. The docs say complaint reports use `complaint_id` and `shareComplaintToMap(complaintId, ...)`. Current code takes `sessionId`, ignores it in persistence, and `WalkOMeterReport` has no `complaint_id`.
    References: `docs/features/feat-walk-o-meter.md`, `docs/features/feat-bang-jaga.md`, `src/app/actions/reports.ts`, `src/lib/types.ts`, `src/app/chat/page.tsx`

52. `doc ahead of code`
    The docs say report tags come from a controlled vocabulary of 10 with max 3 tags. Current `getAutoTags()` simply asks Gemini for `2-3 relevant Indonesian hashtags` and blindly stores whatever JSON comes back. No controlled vocabulary enforcement was found.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/actions/reports.ts`

53. `code ahead of docs`
    `src/lib/types.ts` includes a `resolved` report status in `WalkOMeterReport.status`, but the Walk-o-Meter doc still documents `pending | accepted | rejected` only for report status.
    References: `docs/features/feat-walk-o-meter.md`, `src/lib/types.ts`

54. `doc ahead of code`
    The docs say map/feed should show both report types with distinct visual styling and pin color. The feed does render report-type distinctions, but a large part of the richer trust-tier/pin-color behavior described in the docs is not present in current map/feed UI.
    References: `docs/features/feat-walk-o-meter.md`, `src/app/map/page.tsx`, `src/app/feed/page.tsx`

---

## Items Already Explicitly Deferred

- `BL-001` in `backlog.md`
  Deferred: follow notifications for followed promises in Promise Tracker.

---

## Suggested Next Triage Order

1. Bang Jaga auth/routing/RAG contract
2. Walk-o-Meter verification model and score recalculation
3. Promise Tracker submission/moderation flow
4. Location privacy and region-model cleanup for Walk-o-Meter
5. PRD/feature-doc cleanup after behavior decisions are finalized

---

## Files Most Worth Rechecking First

- `src/app/actions/chat.ts`
- `src/app/chat/page.tsx`
- `src/app/actions/reports.ts`
- `src/app/feed/page.tsx`
- `src/app/map/page.tsx`
- `src/app/actions/promises.ts`
- `src/app/promise-tracker/page.tsx`
- `src/components/SubmitPromiseModal.tsx`
- `docs/features/feat-bang-jaga.md`
- `docs/features/feat-walk-o-meter.md`
- `docs/features/feat-promise-tracker.md`
