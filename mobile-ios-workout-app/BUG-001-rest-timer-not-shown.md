### BUG-001. Workout: rest timer does not start after a set is logged

**Environment:** Гарт 1.0 (build ≤16, first build, 3 Nov), TestFlight, iPhone 17 Pro, iOS 26.x
**Preconditions:** Fresh install from TestFlight, default settings, no changes in Settings.

**Steps to reproduce:**
1. Open the app, tab **Сьогодні**.
2. Start a workout and log a set (e.g. "жим лежачи 45 на 8") by voice or keyboard.
3. Confirm the set.

**Expected result:** A rest timer between sets starts automatically (default ~3 min for heavy exercises) and is clearly visible; an alarm plays when it ends.
**Actual result:** No rest timer appears. Only the total workout time (small text in the "Тренування йде" card) keeps counting. In Settings the rest timer turned out to be **off**.

**Severity:** Major - the rest timer is the main value of the app during a workout; without it the user has to track rest elsewhere.
**Priority:** High
**Reproducibility:** 1/1 (first workout on the initial build)
**Notes:** The developer confirmed the timer should be on by default; it got disabled by a settings change. Rest timer is now on by default with a hint on the exercise card.
**Attachments:** [screenshot](../screenshots/bug-001-rest-timer-missing.jpg)

**Status:** ✅ Fixed in build 17, ✅ Verified on 6 Nov 2026 (rest timer 2:56 visible after a set)
