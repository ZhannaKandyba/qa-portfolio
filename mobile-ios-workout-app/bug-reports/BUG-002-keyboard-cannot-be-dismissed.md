### BUG-002. Profile settings: numeric keyboard cannot be dismissed after entering birth year

**Environment:** Гарт 1.0 (build ≤16, first build, 3 Nov), TestFlight, iPhone 17 Pro, iOS 26.x
**Preconditions:** User is on **Профіль → Налаштування**.

**Steps to reproduce:**
1. Tap the **Рік народження** field.
2. Enter a year with the numeric keyboard.
3. Try to close the keyboard: tap outside the field, scroll down.

**Expected result:** The keyboard closes (tap outside / "Готово" button), the rest of the settings screen is reachable.
**Actual result:** The numeric keyboard has no "Done" key and does not close on tap outside. The lower part of the screen (**Іконка застосунку** and below) is covered. The only way out is **Зберегти** and opening the editor again.

**Severity:** Minor - there is a workaround (Save and re-open), but settings below the keyboard are unreachable.
**Priority:** Medium
**Reproducibility:** 1/1
**Notes:** Likely affects the **Вага тіла** field too (same numeric keyboard), not checked yet.
**Attachments:** [screenshot](../screenshots/bug-002-keyboard-cannot-be-dismissed.jpg)

**Status:** ✅ Fixed in build 17 (a "Готово" button was added), ⏳ to verify on build 19
