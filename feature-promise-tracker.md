# Promise Tracker Feature Specification

## Overview
The **Promise Tracker** (also known as the Promise Ledger) is the core feature of WIWOKDETOK, allowing citizens to track commitments made by elected officials. It provides a source-of-truth database for holding politicians accountable.

## Key Capabilities
1. **Search & Filter:** Users can search for promises by politician name, role, region, or keywords. Filtering is supported by categories: New Promises, Progress Updates, and Fulfillments.
2. **AI Summary:** Lengthy promises include an AI-generated concise summary, breaking down the commitment into WHAT, WHEN, and BUDGET.
3. **Walk-o-Meter Integration:** Each promise displays a Walk-o-Meter score, aggregating community votes on whether the politician is "walking the talk."
4. **Community Engagement:** Users can like, follow, and flag individual promises. Following a promise triggers notifications for future updates.
5. **Commenting (Bang Jaga integration):** The official mascot, Bang Jaga, drops automated contextual commentary on each promise to explain its civic impact or point out unrealistic deadlines. Users can also discuss promises in dedicated discussion threads.

## Data Model (Supabase)
- **Table:** `promises`
- **Fields:** `id`, `politician_name`, `politician_role`, `category`, `quote`, `source_url`, `source_domain`, `date`, `summary_what`, `summary_when`, `summary_budget`, `watchdog_commentary`, `walk_o_meter_score`, `walk_o_meter_count`, `like_count`

## Security & Auth
- **Gated Actions:** Reacting (like, follow, flag) and commenting require the user to be authenticated.
- **Reporting:** Flagging triggers an internal moderation queue review.
