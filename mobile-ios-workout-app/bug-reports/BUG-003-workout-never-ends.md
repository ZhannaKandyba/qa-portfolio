### BUG-003. Workout: a forgotten workout keeps running for hours

**Environment:** Гарт 1.0 (build ≤16, first build, 3 Nov), TestFlight, iPhone 17 Pro, iOS 26.x
**Preconditions:** A workout is started and at least one set is logged.

**Steps to reproduce:**
1. Log the last set of the workout.
2. Leave the gym without tapping **Завершити**.
3. Open the app a few hours later.

**Expected result:** The workout is auto-paused or auto-finished after a period of inactivity (like Apple Fitness workouts), so duration stats stay correct.
**Actual result:** The workout is still running: "Тренування йде 5:13:21". Total workout time is inflated.

**Severity:** Minor - no data loss, but workout duration and stats become wrong.
**Priority:** Medium
**Reproducibility:** 1/1
**Notes:** Real workout lasted ~2.5 h.
**Attachments:** [screenshot](../screenshots/bug-003-workout-running-5h.jpg)

**Status:** ✅ Fixed in build 17 (workout auto-closes 2 h after the last set), ⏳ to verify on build 19
