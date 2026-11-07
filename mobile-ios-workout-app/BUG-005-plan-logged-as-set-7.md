### BUG-005. Voice logging: "6 sets of 8" is logged as set #7, next "другий підхід" adds set #8

**Environment:** Гарт 1.0 (build 17 or 18, 6 Nov), TestFlight, iPhone 17 Pro, iOS 26.x
**Preconditions:** Workout started. Previous workout has "Станова тяга 75 × 8" logged (5 sets).

**Steps to reproduce:**
1. Hold the microphone button and say: "Станова тяга, 6 підходів по 8 повторень, 75 кг".
2. Confirm.
3. After the first real set, say: "Другий підхід".

**Expected result:** The phrase is treated as a **plan** for today (6 × 8 × 75 kg, 0 of 6 done). "Другий підхід" fills set #2 of that plan and starts the rest timer.
**Actual result:** After step 1 the card immediately shows **Підхід 7: 75 × 8**. After step 3 one more set is added (**Підхід 8**). The journal gets sets that were not done.

**Severity:** Major - wrong data in the journal, which breaks progress, XP and ranks.
**Priority:** High
**Reproducibility:** 1/1
**Notes:** Developer is adding a new "План на сьогодні" flow: plan saved with a toggle "Вже зроблено", "перший/другий підхід" fill the next plan slot, a repeated set number shows an "already logged" alert.
**Attachments:** [screenshot](../screenshots/bug-005-set-7-instead-of-plan.jpg)

**Status:** ✅ Fixed in build 19 (voice plan: "deadlift 6 sets of 8 at 75" sets a plan), ⏳ to verify on build 19
