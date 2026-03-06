# Profile & Gamification Feature Specification

## Overview
The **Profile Page** incentivizes civic participation through a robust gamification system while serving as the central hub for account management and preferences.

## Key Capabilities
1. **Gamification (Watchdog Hierarchy):** 
   - Users earn XP for interacting with the platform (comments, reports, likes, verifications).
   - Progression through 7 Watchdog Levels: *Warga Baru* → *Pengamat* → *Detektif Sipil* → *Polisi Moral* → *Pahlawan Desa* → *Hakim Rakyat* → *Environmental Guardian*.
2. **Verified Citizen Badge:** A dynamically awarded badge signaling trustworthiness (requires phone verification + account age + 3 reports).
3. **Preference Center:** Configurable alerts and toggles.
   - Notification Channels: Email, SMS, Push.
   - Frequency: Daily vs. Weekly digest.
   - Regions of Interest: Configured provinces/regencies triggering specific alerts.
4. **Account Controls:** Direct Supabase Auth integration for email and password updates.

## Data Integration
- **XP Calculation:** Derived dynamically from `app/actions/profile.ts` by counting rows in `reports` and `comments`.
- **User Preferences:** Stored securely in `user_preferences` table.
