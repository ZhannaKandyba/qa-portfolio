Гарт: журнал тренувань — iOS Beta Testing

Manual QA of Гарт, a Ukrainian-language gym tracker for iOS, tested as a beta tester via TestFlight during active development. Testing is done with the developer's permission to publish results in this portfolio.

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

In scope: voice logging, lock screen control, workout journal, XP and groups. Out of scope: backend, performance, security, Android. Details: test-plan.md.

Test environment
Item	Value
App version / build	1.0 (build TBD)
Distribution	TestFlight
Device	iPhone 17 Pro
iOS version	TBD
Audio	iPhone mic, AirPods TBD model
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
checklist.md	Smoke checklist for each build
test-cases.md	Test cases for key flows
test-cycles/	One file per build: what was tested, found, verified
bug-reports/	Bug reports
screenshots/	Evidence
feedback-to-dev.md	UX observations and suggestions
summary.md	Results, risks, recommendations
