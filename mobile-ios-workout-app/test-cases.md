# Test Cases — Voice Recognition (Гарт)

**Feature:** speech-to-text for logging sets by voice.
**Change (per developer):** Apple Speech (≈45% accuracy on 130 phrases) replaced by Whisper via Grok API free tier (≈75%) + code-based fuzzy matching of phrases to exercise names (≈86%). Measured on clean `.wav` files, not on real gym audio.

**Requirements found:**
- R1. A spoken phrase is transcribed and turned into exercise + weight + reps (+ sets).
- R2. Exercise names are matched to the exercise list even if transcribed slightly wrong (fuzzy matching).
- R3. An unknown word must not swap the exercise for a similar one (build 18: "Arnold press" is not saved as another press).
- R4. Free tier limit: 8 hours of audio processing per day.

**Techniques:** equivalence partitioning (phrase types), error guessing (noise, network, misheard words), regression (BUG-004).

**Environment:** Гарт 1.0 (build [number]), iPhone 17 Pro, iOS 27.2, iPhone mic / AirPods 4 (ANC)

---

### TC-01. Standard phrase is logged with correct exercise, weight and reps
**Priority:** High
**Preconditions:** Workout started, network on.
**Test data:** "Жим лежачи 45 на 8"
**Steps:**
1. Hold the microphone button and say the phrase.
2. Release the button.
**Expected result:**
- Transcript matches the phrase.
- Review card: Жим лежачи, 45 kg, 8 reps; no "Я не все зрозумів" warning.
- After confirmation the set appears in the journal.

### TC-02. "Присід" is recognized as Присідання (regression BUG-004)
**Priority:** High
**Preconditions:** Workout started, network on.
**Test data:** "Присід 65 кілограм на 8"
**Steps:**
1. Say the phrase 5 times with the iPhone mic.
2. Repeat 5 times with AirPods.
**Expected result:**
- Exercise is detected as Присідання зі штангою in each attempt.
- Weight 65, reps 8.
- Record the result per device, e.g. "iPhone 5/5, AirPods 4/5".

### TC-03. Short and slang exercise names are matched to the right exercise
**Priority:** High
**Preconditions:** Workout started.
**Test data:**

| Said | Expected exercise |
|---|---|
| "Станова 75 на 8" | Станова тяга |
| "Жим 45 на 8" | Жим лежачи *(assumption: default for "жим")* |
| "Тяга горизонтальна 40 на 15" | Тяга горизонтального блока |
| "Платформа 150 на 10" | Жим ногами *(assumption)* |

**Steps:**
1. Say each phrase.
**Expected result:**
- Each phrase is mapped to the expected exercise, or the app asks to choose the exercise. No wrong exercise is saved silently.

### TC-04. Unknown exercise name is not swapped for a similar exercise
**Priority:** High
**Preconditions:** Workout started.
**Test data:** "Жим Арнольда 12 на 10", a made-up name "Жим космонавта 20 на 10"
**Steps:**
1. Say each phrase.
**Expected result:**
- Жим Арнольда is saved as Жим Арнольда (if it is in the list).
- The made-up name is **not** saved as Жим лежачи or another press; the app shows "Не зрозумів вправу" and offers to choose.

### TC-05. Numbers: decimals and numbers as words
**Priority:** High
**Preconditions:** Workout started.
**Test data:** "Тяга 142,5 на 2", "Присід сто двадцять на три", "Жим 47 з половиною на 4"
**Steps:**
1. Say each phrase.
**Expected result:**
- Weights: 142.5, 120, 47.5; reps: 2, 3, 4.
- Weight and reps are not swapped.

### TC-06. Recognition in real gym conditions (noise, music)
**Priority:** Medium
**Preconditions:** In the gym, music playing, workout started.
**Test data:** phrases from TC-01–TC-05
**Steps:**
1. Say each phrase with the iPhone mic held at a normal distance.
2. Repeat with AirPods (ANC on).
**Expected result:**
- Most phrases are recognized correctly; record the result per device.
- Misrecognized phrases are shown on the review card before saving (nothing is saved without confirmation).

### TC-07. No internet: voice input fails clearly, no data is lost
**Priority:** High
**Preconditions:** Workout started. Turn on Airplane mode (Wi-Fi off).
**Test data:** "Жим лежачи 45 на 8"
**Steps:**
1. Say the phrase.
2. Turn the network back on and try again.
**Expected result:**
- A clear message that there is no connection *(assumption: exact behavior not specified)*.
- Manual input (keyboard) is offered and works.
- After the network is back, voice logging works again; no duplicate set appears.

### TC-08. Long or complex phrase is not merged or cut
**Priority:** Medium
**Preconditions:** Workout started.
**Test data:** "Віджимання на брусах плюс 20 на 8, три підходи"
**Steps:**
1. Say the phrase at normal speed.
2. Say it fast, without pauses.
**Expected result:**
- Exercise: Віджимання на брусах, extra weight 20 kg, 8 reps, 3 sets.
- Words are not glued or dropped; if they are, the review card highlights what to fix.

---

## Questions to the team
1. Which STT is used in the current TestFlight build: Grok free tier, Soniox or Apple Speech?
2. What happens when the 8 h/day free tier limit is reached: fallback to Apple Speech, an error, something else?
3. Expected behavior offline: is there a fallback to on-device recognition?
4. Is there a target accuracy for release (e.g. ≥ 85% on real gym audio)?
5. Can testers see the 130-phrase set to add gym-specific phrases?

---

# Test Cases — Rest Timer, Voice Set Flow, Workout Plan (builds 17–19)

**Requirements found (from developer notes and release notes):**
- R5. Rest timer is on by default and starts after a set is saved; ~3 min for heavy exercises; length configurable in Settings; an alarm plays when time is up.
- R6. If timer auto-start is off, a **Rest** button is shown.
- R7. Voice set flow: say the exercise ("жим лежачи") → it becomes the focus card → "1-й підхід", "2-й підхід"… log sets with weight/reps from last time or from the coach plan. First time for an exercise: say numbers ("40 на 12").
- R8. Voice plan: "станова 6 підходів по 8 на 75" opens **План на сьогодні** with an **Вже зроблено** switch; after saving: focus card "План: 75 × 8" and dashed rows under the exercise.
- R9. "Перший підхід", a tap on "План: 75 × 8" or **Ще раз** fill the next slot; header shows "Підходів: 1 з 6".
- R10. A set number is an address: "другий підхід 7 повторень" edits set #2; a bare "2-й підхід" when #2 is logged shows an "уже записаний" alert.
- R11. A forgotten workout closes itself 2 h after the last set.

**Techniques:** state transitions (plan slots, timer), boundary values (2 h auto-close, number of plan slots), use cases / E2E (workout flow), regression (BUG-001, BUG-003, BUG-005).

### TC-09. Rest timer starts automatically after a set (regression BUG-001)
**Priority:** High
**Preconditions:** Fresh install or default settings; workout started.
**Test data:** "Станова тяга 75 на 8"
**Steps:**
1. Log the set by voice and confirm.
2. Wait until the timer ends.
**Expected result:**
- Rest timer starts right after saving and is clearly visible on the exercise card.
- Default duration ≈ 3 min for a heavy exercise.
- An alarm plays when the time is up.

### TC-10. Rest timer can be adjusted and stopped
**Priority:** Medium
**Preconditions:** Rest timer is running.
**Steps:**
1. Tap **+15**.
2. Tap **−15** twice.
3. Tap **×**.
**Expected result:**
- Time changes by +15 s / −15 s each tap.
- **×** stops the timer; no alarm plays.

### TC-11. "N-й підхід" logs a set with weight and reps from last time
**Priority:** High
**Preconditions:** Last workout has "Жим лежачи 45 × 8". Workout started, no plan for today.
**Steps:**
1. Say "Жим лежачи".
2. Say "1-й підхід".
3. Say "2-й підхід".
**Expected result:**
- After step 1 Жим лежачи becomes the focus card.
- Steps 2–3 log sets #1 and #2 as 45 × 8; rest timer starts after each set.

### TC-12. First time for an exercise: set without numbers is not saved with made-up values
**Priority:** Medium
**Preconditions:** The exercise was never logged before (e.g. "Жим Арнольда"); no coach plan.
**Steps:**
1. Say "Жим Арнольда".
2. Say "1-й підхід".
3. Say "12 на 10".
**Expected result:**
- Step 2: the app asks for weight and reps *(assumption: exact behavior not specified)*; no set with empty or invented values is saved.
- Step 3: set #1 is saved as 12 × 10.

### TC-13. Voice plan opens "План на сьогодні" instead of logging sets (regression BUG-005)
**Priority:** High
**Preconditions:** Build 19+, workout started.
**Test data:** "Станова 6 підходів по 8 на 75"
**Steps:**
1. Say the phrase.
2. Leave **Вже зроблено** off and save.
**Expected result:**
- Step 1: **План на сьогодні** opens with the switch **Вже зроблено** (off).
- Step 2: focus card "План: 75 × 8", 6 dashed rows under Станова тяга, header "Підходів: 0 з 6".
- **No** sets are logged; no "Підхід 7".

### TC-14. Plan slots are filled one by one by voice, tap and "Ще раз"
**Priority:** High
**Preconditions:** Plan from TC-13 saved (0 of 6).
**Steps:**
1. Say "Перший підхід".
2. Tap "План: 75 × 8".
3. Tap **Ще раз**.
4. Repeat until 6 of 6.
5. Say "Ще один підхід" once more.
**Expected result:**
- Each action fills the next dashed row with 75 × 8; header: 1 з 6 → 2 з 6 → 3 з 6 … 6 з 6.
- Rest timer starts after each set.
- Step 5: a 7th set is added outside the plan or the app asks *(assumption: behavior after the plan is complete not specified)*.

### TC-15. Set number edits the existing set and does not duplicate it
**Priority:** High
**Preconditions:** Sets #1 and #2 logged as 75 × 8.
**Steps:**
1. Say "Другий підхід 7 повторень".
2. Say "2-й підхід".
**Expected result:**
- Step 1: set #2 changes to 75 × 7; the number of sets stays 2.
- Step 2: alert "уже записаний"; no new set is added.

### TC-16. Forgotten workout closes 2 h after the last set (regression BUG-003)
**Priority:** Medium
**Preconditions:** Workout started, one set logged; note the time of the last set.
**Steps:**
1. Do not tap **Завершити**. Open the app ~1 h 55 min after the last set.
2. Open the app again ~2 h 05 min after the last set.
**Expected result:**
- Step 1: workout is still running.
- Step 2: workout is finished; duration ends at the last set (or +2 h) *(assumption: which end time is saved not specified)*; no "5:13:21"-like duration.

## Questions to the team (builds 17–19)
6. What happens after the plan is complete (7th set on a 6-set plan)?
7. What is saved as the end time of an auto-closed workout: time of last set or last set + 2 h?
8. "1-й підхід" for a new exercise without numbers: what should the app do?

---

# Test Cases — iOS-specific checks

**Techniques:** error guessing, interruption testing, state persistence.
**Note:** expected results marked *(assumption)* follow common iOS practice and need confirmation from the developer.

### TC-17. Microphone / speech permission denied
**Priority:** High
**Preconditions:** Settings → Гарт → Microphone **off**.
**Steps:**
1. Open the app and tap the microphone button.
2. Log a set with the keyboard button.
**Expected result:**
- A clear message that microphone access is needed, with a way to open Settings *(assumption)*.
- No crash, no endless "Слухаю…".
- Manual input works.

### TC-18. Rest timer alarm fires when the app is in background or the screen is locked
**Priority:** High
**Preconditions:** Rest timer started (set 1 min in Settings to save time).
**Steps:**
1. Lock the iPhone.
2. Wait until the timer ends.
3. Repeat with the app in background (another app open).
**Expected result:**
- At 0:00 an alarm / notification is received in both cases.
- After opening the app the timer state is correct (finished, not still counting).

### TC-19. Live Activity / Dynamic Island shows the same time as the app
**Priority:** Medium
**Preconditions:** Workout started, rest timer running.
**Steps:**
1. Go to the Home screen; check the Dynamic Island and the lock screen.
2. Compare with the timer inside the app.
**Expected result:**
- Dynamic Island / lock screen show the rest timer (or workout time) with the same value as the app (±1 s).
- The activity disappears after **Завершити**.
**Notes:** on 6 Nov the Dynamic Island showed 16:47 while the app showed rest 2:56 and workout 1:36 - to check.

### TC-20. Rest alarm in Silent mode
**Priority:** Medium
**Preconditions:** Silent mode on (side switch / Action button).
**Steps:**
1. Log a set and wait until the rest timer ends.
**Expected result:**
- The user is notified (vibration and/or sound) *(assumption: expected behavior in Silent mode not specified)*.

### TC-21. Incoming call during voice recording
**Priority:** Medium
**Preconditions:** Workout started; a second phone to call from.
**Steps:**
1. Hold the mic button and start saying "Жим лежачи 45 на 8".
2. Receive a call in the middle of the phrase; decline it.
3. Record the phrase again.
**Expected result:**
- Recording stops; no partial or wrong set is saved.
- After the call, voice input works normally.

### TC-22. AirPods disconnect in the middle of a phrase
**Priority:** High
**Preconditions:** AirPods 4 connected, workout started.
**Steps:**
1. Start saying "Присід 65 на 8".
2. Take out the AirPods / put them in the case mid-phrase.
3. Say the phrase again.
**Expected result:**
- No wrong set is saved; the review card shows what was heard or an error.
- Step 3: input switches to the iPhone microphone and works.

### TC-23. App is closed during a workout - data and timer are restored
**Priority:** High
**Preconditions:** Workout started, 2 sets logged, rest timer running.
**Steps:**
1. Swipe the app away from the App Switcher.
2. Open it again.
**Expected result:**
- Workout is still running, both sets are in place.
- Rest timer continues with the correct remaining time (or is finished if the time is up).

### TC-24. Music keeps playing when using the app (build 19)
**Priority:** Medium
**Preconditions:** Music playing in Apple Music / Spotify via AirPods.
**Steps:**
1. Open Гарт.
2. Log a set by voice.
3. Wait for the rest alarm.
**Expected result:**
- Music does not stop when the app opens or after a set is recorded.
- Music is ducked (quieter) during recording/alarm and returns to normal after *(assumption)*.

### TC-25. Logging a set from the lock screen button
**Priority:** High
**Preconditions:** Hart button added to the lock screen in place of the flashlight; workout started; iPhone locked.
**Steps:**
1. On the locked screen, press the Hart button.
2. Say "Жим лежачи 45 на 8".
3. Unlock and open the app.
**Expected result:**
- Recording starts without unlocking the iPhone *(assumption: or after Face ID)*.
- The set appears in the workout; rest timer starts.

### TC-26. Large text (Dynamic Type)
**Priority:** Low
**Preconditions:** Settings → Display & Brightness → Text Size: maximum.
**Steps:**
1. Open **Сьогодні**, the review card and **Профіль → Налаштування**.
**Expected result:**
- Text is readable; no overlapping or cut-off labels and buttons.

## Additional iOS checklist (quick checks)
- [ ] Light mode: all screens readable (app looks dark-only)
- [ ] Low Power Mode: timer and alarm still work
- [ ] Notifications denied: what happens with the rest alarm
- [ ] Wi-Fi ↔ mobile data switch during recording
- [ ] App update from TestFlight: data from the previous build is kept
- [ ] Rotation: app stays in portrait / layout is correct
