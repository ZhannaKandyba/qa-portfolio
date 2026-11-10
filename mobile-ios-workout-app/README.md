Гарт: журнал тренувань — iOS Beta Testing
Manual QA of Гарт, a Ukrainian-language gym tracker for iOS, tested as a beta tester via TestFlight during active development.
Testing is done with the developer's permission to publish results in this portfolio.
About the app
A workout journal with voice control, built for Ukrainian-speaking athletes.
Key features:
Ukrainian UI
Voice logging from the lock screen: a Control Center / lock screen control (like the flashlight button) or a widget; works with the iPhone microphone and AirPods
AI parsing of speech: phrases like "bench press 100 for 5, three sets" are turned into journal entries automatically
Gamification: XP, levels, progress
Local groups with friends: see each other's progress and compete
Tech stack (per the developer):
Native iOS app: Swift, Xcode
Backend: Python
Speech recognition: Apple Speech framework (Whisper models were also evaluated)
Speech analysis: Claude API (Haiku 5.5) + rule-based logic
Scope
In scope: voice logging, lock screen control, workout journal, XP and groups.
Out of scope: backend, performance, security, Android.
Details: test-plan.md.
Test environment
Item	Value
App version / builds	1.0 (builds 16–19)
Distribution	TestFlight
Device	iPhone 17 Pro
iOS version	26.x (cycle 1), 27.2 (from 7 Nov 2026)
Audio	iPhone mic, AirPods 4 (ANC)
Network	Wi-Fi, mobile data, offline
Testing approach
Exploratory sessions, including real workouts in the gym
Checklist-based smoke testing for every new build
Test cases for key flows (voice logging, journal, XP)
Regression and fix verification across builds
UX feedback from the perspective of an experienced athlete (CMS in powerlifting)
Contents
File / folder	Description
test-plan.md	Scope, approach, risks
test-cases.md	26 test cases: voice recognition, rest timer, workout plan, iOS-specific checks
test-cycles/	One report per test cycle: what was tested, found, verified
bug-reports/	Bug reports with status (found → fixed → verified)
feedback-to-dev.md	UX observations and suggestions from real workouts
screenshots/	Evidence for bug reports and UX feedback
Results so far
	
Test cycles	1 (3–6 Nov 2026, 3 real gym workouts)
Bugs reported	5 (3 Major, 2 Minor)
Fixed by developer	4 of 5
Verified	1
UX suggestions	6 (3 implemented)
Highlights:
Suggested a voice flow "exercise → «1-й підхід», «2-й підхід»… → rest timer", implemented by the developer in build 17.
Found that a voice plan ("6 sets of 8 at 75") was logged as completed sets, which corrupted the journal - fixed in build 19.
Status
🟡 In progress - next: cycle 2 on build 19 / iOS 27.2 (verify fixes, lock screen control, groups)