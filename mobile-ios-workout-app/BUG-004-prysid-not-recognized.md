### BUG-004. Voice logging: "Присід" is recognized as "При", exercise is not detected

**Environment:** Гарт 1.0 (build 17 or 18, 5 Nov), TestFlight, iPhone 17 Pro, iOS 26.x, iPhone microphone and AirPods 4 (ANC)
**Preconditions:** Workout started, tab **Сьогодні**.

**Steps to reproduce:**
1. Hold the microphone button.
2. Say: "Присід 65 кілограм на 8".
3. Release the button.

**Expected result:** Set is parsed as **Присідання зі штангою, 65 kg × 8**.
**Actual result:** Transcript shows «При 65 кілограм на 8». Weight and reps are parsed correctly, but the exercise is empty: "Я не все зрозумів / Не зрозумів вправу". The user has to pick the exercise manually from search.

**Severity:** Major - voice logging is the core feature; one of the main powerlifting exercises cannot be logged by voice.
**Priority:** High
**Reproducibility:** Intermittent - happened several times during one workout (exact count not recorded), with the iPhone mic and with AirPods. To measure on build 19: 5 attempts per audio device.
**Notes:** Build 18 added more exercise names to voice; to recheck on build 19. The word is cut at speech-to-text level (Apple Speech), not by the AI parser. Developer suggested adding short aliases for exercises ("присід", "жим", "станова"). Manual search for "Присід" works.
**Attachments:** [screenshot](../screenshots/bug-004-prysid-recognized-as-pry.jpg), screen recording IMG_9477

**Status:** 🔴 Open
