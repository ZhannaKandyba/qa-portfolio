# Test cycle 1 — 3–6 Nov 2026

**Builds:** 1.0 (≤16, initial, 3 Nov) → 1.0 (17, fixes for BUG-001–003, coach, voice set flow) → 1.0 (18, more exercise names)
**Device:** iPhone 17 Pro, iOS 26.x; iPhone mic and AirPods 4 (ANC)
**Type:** Exploratory testing during 3 real gym workouts (3, 5, 6 Nov)

## Covered
- Starting / finishing a workout
- Voice logging of sets (iPhone mic, AirPods), manual correction, exercise search
- Rest timer and total workout time
- Profile settings (data for ranks)
- Pro features: coach targets, exercise rank (E–S)

## Not covered yet
- Lock screen / Control Center control, widget
- Groups with friends
- Offline mode
- Journal history, editing and deleting sets

## Results
| ID | Title | Severity | Status |
|---|---|---|---|
| [BUG-001](../bug-reports/BUG-001-rest-timer-not-shown.md) | Rest timer does not start | Major | ✅ Verified |
| [BUG-002](../bug-reports/BUG-002-keyboard-cannot-be-dismissed.md) | Keyboard cannot be dismissed | Minor | Fixed in 17, to verify |
| [BUG-003](../bug-reports/BUG-003-workout-never-ends.md) | Forgotten workout runs for hours | Minor | Fixed in 17, to verify |
| [BUG-004](../bug-reports/BUG-004-prysid-not-recognized.md) | "Присід" recognized as "При" | Major | 🔴 Open |
| [BUG-005](../bug-reports/BUG-005-plan-logged-as-set-7.md) | Plan logged as set #7 | Major | Fixed in 19, to verify |

UX feedback: 6 items, see [feedback-to-dev.md](../feedback-to-dev.md).

## Next cycle
- Build 19 on iOS 27.2 (updated 7 Nov): verify BUG-002, BUG-003, BUG-005; recheck BUG-004
- Test the new "План на сьогодні" flow
- Start lock screen control and groups
