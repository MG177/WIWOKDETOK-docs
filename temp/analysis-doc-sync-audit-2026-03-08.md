# Documentation Sync Audit

Audit date: `2026-03-08`
**Re-checked: `2026-03-10`** (parallel subagents)

Scope: `docs/PRD.md`, `docs/features/feat-*.md`, current implementation in `src/`

---

## Summary

| Status | Count |
|--------|-------|
| ✅ SOLVED | 53 |
| ❌ STILL OPEN | 1 |

**Open:** #11 — region-only empty state shows "No promises tracked in this region yet." but only "Clear Filters"; docs expect an **[Explore National]** CTA in that state.

---

## Findings — Verified Against Codebase (2026-03-10)

### A. Cross-Cutting / PRD / Global Docs

1. ✅ **PRD.md line 149** — `Client-side polling + Email (via Resend). Web Push API deferred`
2. ✅ **chat/page.tsx line 108** — `requireAuth("access Bang Jaga")` on mount
3. ✅ **PRD.md line 128** — links to `docs/features/feat-profile.md`; **docs/feature-profile.md** redirect stub created

### B. Promise Tracker (`F-001`)

4. ✅ **promise-tracker/page.tsx line 248** — `<h1>The Talk Ledger</h1>`
5. ✅ **actions/promises.ts line 22** — `.or(quote.ilike, politician_name.ilike, watchdog_commentary.ilike, politician_role.ilike, region_id.ilike)`
6. ✅ **promise-tracker/page.tsx line 250** — `requireAuth("submit a new promise")` before modal open
7. ✅ **actions/promises.ts lines 148-157** — Flag count ≥10 triggers `[ADMIN ALERT]` log
8. ✅ **actions/promises.ts lines 172-180** — `promise_submissions` with `status: 'pending'`; **feat-promise-tracker.md AC-09** updated: admin UI deferred
9. ✅ **promise-tracker/page.tsx line 406** — `Under review — we'll notify you when it's live.`
10. ✅ **actions/promises.ts lines 103-122** — `pageSize = 20` pagination with `hasMore`
11. ❌ **OPEN** — Region-only empty state has "No promises tracked in this region yet." but only "Clear Filters" button; docs expect **[Explore National]** CTA in that state.
12. ✅ **promise-tracker/page.tsx** — Load More triggers `loadingMore` state with spinner
13. ✅ Same as #7 — flag logic at ≥10
14. ✅ **actions/promises.ts lines 232-244** — `flagPromiseComment()` increments `flag_count`
15. ✅ **promise-tracker/page.tsx lines 115-123** — `pendingUnfollowId` with 2500ms timeout; **docs updated** to describe pill UX
16. ✅ **promise-tracker/page.tsx lines 452-456** — `Source Unavailable` badge for `source_status === "404"`; **docs updated**
17. ✅ **promise-tracker/page.tsx lines 364-373** — Feed error: "Couldn't load promises..." + Retry
18. ✅ Paywalled source footnote in card UI — matches docs
19. ✅ 404 source footnote — matches docs
20. ✅ Follow notifications deferred to `backlog.md`

### C. Bang Jaga (`F-002`)

21. ✅ **chat/page.tsx line 108** — `requireAuth("access Bang Jaga")`
22. ✅ **chat/[sessionId]/page.tsx line 16** — redirects to `/chat?session=${sessionId}`; **chat/page.tsx lines 226-233** — reads `session` param
23. ✅ **actions/chat.ts lines 73-76** — `getSessionMessages()` checks `session.user_id !== user.id`
24. ✅ **feat-bang-jaga.md** — US-04 changed to P2; AC-03 notes Search Laws deferred
25. ✅ **feat-bang-jaga.md** — `rag_feedback` described as per-message free-text
26. ✅ **actions/chat.ts** — `sendMessage()` fetches attachments, converts to base64 `inlineData`
27. ✅ **feat-bang-jaga.md** — `getRegulationContext()` API notes region fallback
28. ✅ **feat-bang-jaga.md** — BR-05 now says client-side typewriter; SSE deferred
29. ✅ **feat-bang-jaga.md** — AC-06 notes minor template differences acceptable
30. ✅ **chat/page.tsx** — `handleWhatsAppCopy()` with clipboard + "Berhasil disalin!" alert
31. ✅ **actions/chat.ts** — `sendMessage()` generates titles via Gemini; sidebar groups sessions
32. ✅ **chat/page.tsx** — Attachment validation: JPG/PNG/WEBP, 5MB per file, 5 files, 20MB total
33. ✅ **chat/page.tsx lines 94-104** — Offline detector + banner + blocked sending
34. ✅ **actions/chat.ts** — System prompt with hallucination fallback
35. ✅ **feat-bang-jaga.md** — API status documented (implemented vs deferred)
36. ✅ **feat-bang-jaga.md** — Already uses correct `NEXT_PUBLIC_*` env vars
37. ✅ **feat-walk-o-meter.md** — Feature flags marked "aspirational — not yet implemented"

### D. Walk-o-Meter (`F-003`)

38. ✅ **actions/reports.ts lines 267-321** — `recalculatePromiseScores()` with `hasPhoto = !!r.photo_url`; **docs updated** with 1.2× weight note
39. ✅ **actions/reports.ts lines 157-175** — Vote uniqueness + 24-hour edit window
40. ✅ **promise-tracker/page.tsx** — Verify Evidence button on cards
41. ✅ **actions/reports.ts lines 72-99** — `verifyReportEvidence()` increments `verification_count`, promotes `trust_tier` at ≥5
42. ✅ **feed/page.tsx** — Guest sees no Submit/Verify buttons (guarded by `isAuthenticated`)
43. ✅ **feed/page.tsx lines 189-200** — 15-second polling; paused in Data Saver mode
44. ✅ **map/page.tsx lines 662-671** + **feed/page.tsx lines 330-337** — Data Saver persistent banner
45. ✅ **actions/reports.ts lines 57-68** — Jitter-based GPS fuzzing (~1km resolution)
46. ✅ **regions.ts** + **feed/page.tsx** — Both produce `jawa-barat` format slugs; consistent
47. ✅ **map/page.tsx** — "Explore All Reports" link: `/feed?province=${slugify(activeProvince)}`
48. ✅ **actions/reports.ts lines 102-126** — `getLeaderboard(regionId)` with filter
49. ✅ **feed/page.tsx** — Promise link with fallback: "Original promise no longer available."
50. ✅ **feat-walk-o-meter.md** — Offline drafts scope clarified; profile display deferred
51. ✅ **actions/reports.ts line 255** — `complaint_id: data.sessionId` persisted
52. ✅ **actions/reports.ts** — `getAutoTags()` uses Gemini with 10-tag controlled vocabulary
53. ✅ **feat-walk-o-meter.md** — Status enum: `pending | accepted | rejected | resolved`
54. ✅ **feed/page.tsx lines 400-406** — Trust-tier card borders: green (ground_truth), amber (complaint), default (verification)
