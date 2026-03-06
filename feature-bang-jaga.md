# Bang Jaga Feature Specification

## Overview
**Bang Jaga** is the vigilant app mascot and automated watchdog for WIWOKDETOK. This persona drops hard truths, contextual commentary, and gamified encouragements across the platform.

## Key Capabilities
1. **Promise Commentary:** On each promise card, Bang Jaga analyzes the quote, feasibility, and required budget, providing a no-nonsense "Bang Jaga says" snippet.
2. **Contextual Alerts:** Bang Jaga flags suspicious behavior, broken promises, or 404 source links, encouraging the community to take action.
3. **Notification Mascot:** Bang Jaga represents the notification system (e.g., "Bang Jaga Needs You", "A Promise You Follow Appears Broken").

## Data Integration
- `watchdog_commentary` field in the `promises` table.
- Notifications dispatched via Bang Jaga persona.
