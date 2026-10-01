# HabitPulse product review

HabitPulse keeps the daily action as the center of gravity. Common product principles informed the choices here: make check-in immediate, let schedules match real lives, review patterns rather than reward perfection, make progress explainable, and let users leave or pause without losing history. The UI, copy and progress language are original to HabitPulse.

## Current product

| Area | Decision and current behavior |
| --- | --- |
| Simplicity | Today is the landing view; add, check in, edit and focus are one interaction away. Templates are optional drafts. |
| Identity and goals | Each habit links a user-written identity, one of five pillars and a personal direction. This remains a short relationship line, not a second planning system. |
| Scheduling | Daily, weekday and interval schedules; targets are numeric. Unscheduled, pre-creation and paused days are not missed days. |
| Routines and stacking | Named routines show only members scheduled today. A stack suggestion navigates to its next habit but never records progress automatically. |
| Consistency | The 30-day score is completed scheduled habit-days divided by scheduled habit-days. Its formula is shown. Streaks remain secondary context. |
| Recovery | A few recent missed check-ins reveal a quiet return prompt. Lower targets, schedule changes, pause and archive remain user choices. History is retained. |
| Reflection | Mood and notes are optional. Weekly review offers one suggested adjustment and saves the user’s own notes locally. A seven-day introduction is based on the local start date. |
| Coaching | Insights are deterministic, based on completion and at least three timestamped check-ins for time-of-day observations. No generative AI or certainty claims. |
| Focus and space | Focus time is explicitly saved; it contributes minutes only to minute-based goals. Six original atmospheres, density/intensity controls, pause and reduced motion are user-controlled. |
| Privacy | Local-first, no telemetry, exportable JSON/CSV, validated JSON restore and confirmed local deletion. Cloud sync is not represented as active. |
| Accessibility and performance | Semantic controls, keyboard-visible focus, responsive heatmap scroller, reduced-motion support, small bounded starfield, and offline shell. |
| Monetization and social | No active paywall, public profile, social graph or subscription. Pro messaging is an inquiry route only. |

## Not connected yet

Supabase auth, username availability, RLS, admin tier changes and email functions have implementation foundations but are disabled until a project is configured and deployed. The local app does not yet sync accounts across devices. Calendar, Apple Health, Health Connect, wearable data, native widgets, push reminders, accountability sharing, CSV import, and paid purchases are not implemented. No analytics events or private habit content leave the browser.

## Release gates

Before a public store release: configure Supabase and email secrets; implement and security-test cloud sync and account deletion; run database migrations against a staging project; test import/export round trips and service-worker upgrades; validate screen-reader and keyboard flows; test current iOS/Android install behavior; publish owner-reviewed privacy, subscription and support terms; and complete store-specific policy review. The current workspace preview is not a store-certified release.
