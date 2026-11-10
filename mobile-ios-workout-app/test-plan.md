# Test Plan — Гарт: журнал тренувань (iOS Beta)

## 1. Objective

Evaluate the quality and usability of the beta app during real workouts, with focus on its unique features: voice logging from the lock screen, AI parsing of speech, gamification and friend groups.

## 2. Scope

### In scope
| # | Area | What is checked |
|---|---|---|
| 1 | Voice logging | Speech → text → AI parsing → journal entry; accuracy of exercise, weight, reps, sets |
| 2 | Lock screen control | Control Center button, lock screen widget, iPhone mic vs AirPods |
| 3 | Workout journal | Creating, editing, deleting entries; history |
| 4 | Gamification | XP calculation, levels, progress display |
| 5 | Groups | Creating a group, inviting friends, viewing progress, competition, sync between devices |
| 6 | Localization | Ukrainian texts, plural forms, truncated text, typos |
| 7 | iOS specifics | Permissions, background mode, interruptions, offline, dark mode, Dynamic Type |
| 8 | UX | Real-workout usability: what helps, what gets in the way |

### Out of scope
- Backend and API testing directly (no access)
- Performance / load testing
- Security testing
- Android (not available)

## 3. Approach

- **Exploratory testing**: time-boxed sessions (30–45 min) per area, plus real gym workouts
- **Checklist-based smoke testing** for every new TestFlight build
- **Test cases** for key flows: voice logging, journal, XP
- **Regression and fix verification** for each new build
- **AI output testing**: a fixed set of voice phrases is repeated on every build to compare parsing results

## 4. Voice input test data (examples)

| Category | Example phrase |
|---|---|
| Standard | "Жим лежачи 100 на 5, три підходи" |
| Numbers as words | "Присід сто двадцять на три" |
| Decimals | "Тяга 142,5 на 2" |
| Slang | "Жим сотка на п'ять" |
| Surzhyk / mixed | "Становая 180 на 1" |
| Correction | "Жим 100 на 5... ні, 105" |
| Incomplete | "Жим лежачи" (no weight/reps) |
| Unknown exercise | Made-up or rare exercise name |
| Bodyweight | "Підтягування 10 разів" |
| Extra weight | "Віджимання на брусах плюс 20 на 8" |
| Noise | Any phrase with gym noise / music in AirPods |

## 5. Risks

| Risk | Why it matters |
|---|---|
| Wrong AI parsing | Wrong data in the journal breaks progress and XP |
| No internet in the gym | AI parsing needs a network; behavior offline must be clear |
| Lock screen control fails | Core differentiator of the app |
| AirPods disconnect mid-phrase | Common real-life situation |
| XP can be exploited | Unfair competition in groups |
| Group sync delays | Friends see outdated progress |

## 6. Entry / exit criteria

**Entry:** new build available in TestFlight, release notes ("What to Test") read.
**Exit for a build:** smoke checklist passed, new features explored, bugs reported, previous fixes verified.

## 7. Deliverables

- Smoke checklist
- Test cases
- Bug reports (with build number, device, iOS version, steps, screenshots/screen recording)
- Test cycle report per build
- UX feedback for the developer
- Final summary

## 8. Bug reporting

- Channel: direct messages with the developer + TestFlight screenshot feedback; bug reports are documented in [bug-reports/](bug-reports/)
- Severity: Critical / Major / Minor / Trivial
- Each report includes: build number, device, iOS version, network, audio device (iPhone mic / AirPods)
