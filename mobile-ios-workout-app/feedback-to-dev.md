# UX Feedback & Suggestions — Гарт

Feedback from real gym workouts, from the perspective of a powerlifter (CMS).
Status: ✅ accepted / implemented, 💬 discussed, ⏳ planned

| # | Observation | Suggestion | Status |
|---|---|---|---|
| UX-01 | The most common pain point in the gym: forgetting **which set I'm on** and **how long I've rested**. Total workout time matters much less. | Make the **rest timer** the central, large element; on by default. | ✅ Timer on by default, large, with a hint |
| UX-02 | Logging every set with full numbers by voice is slow during a workout. | Voice flow: name the exercise once → say "перший підхід", "другий підхід"… → each phrase logs the set and starts the rest timer → "вправу завершено" closes the exercise. Total workout time runs in the background until **Завершити**. | ✅ Implemented: weight/reps taken from last time or from the plan |
| UX-03 | After starting a workout, saying "1-й підхід" fails with "Не вдалося розібрати фразу", even though the last exercise (Жим лежачи) is shown right below. The hint "Скажи перший підхід — він з'явиться тут" suggests this should work. | Either offer the exercise from "Минулого разу" / today's plan, or change the hint to "Скажи вправу, наприклад «жим лежачи»". | 💬 Discussed (exercise must be named first) |
| UX-04 | A forgotten workout keeps counting for hours. | Auto-pause / auto-finish after inactivity, like Apple Fitness. | ✅ Auto-close 2 h after the last set |
| UX-05 | Speech recognition sometimes misses exercise names (see BUG-004). | Short aliases: "присід", "жим", "станова", "тяга". | 💬 Suggested by the developer |
| UX-06 | Apple Watch support is a natural fit for rest timer and set counting. | Sync with Apple Watch. | ⏳ Planned after release |

## Screenshots
- [UX-03 hint](screenshots/ux-01-say-first-set-hint.jpg), [UX-03 result](screenshots/ux-01-first-set-not-parsed.jpg)
