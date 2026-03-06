# Walk-o-Meter Feature Specification

## Overview
The **Walk-o-Meter** evaluates whether a politician's actions align with their promises. It aggregates user reports, photographic evidence, and community voting into an actionable trust score.

## Key Capabilities
1. **Interactive Tracking (Map & Feed):** Users browse promises via a geographic map of Indonesia or an Evidence Feed log.
2. **Evidence Submission:** Citizens upload photos and descriptions of real-world implementation (or lack thereof) via the "Submit Report" button.
3. **Trust Tiers:**
   - **Community Claim:** A user submits a claim without secondary verification.
   - **Ground Truth:** A claim that has been verified by the community as factual and accurate.
4. **Data Saver Mode:** For users in low-bandwidth regions, Walk-o-Meter includes a toggle to disable heavy map tiles and hide images.

## Data Model (Supabase)
- **Table:** `reports`
- **Fields:** `id`, `user_id`, `user_name`, `location_label`, `latitude`, `longitude`, `description`, `photo_url`, `tags`, `report_type` (e.g. `promise_verification`), `trust_tier`, `created_at`
