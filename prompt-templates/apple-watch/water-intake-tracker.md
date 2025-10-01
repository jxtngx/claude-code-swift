# Water Intake Tracker - Apple Watch User Story

## User Story (INVEST)

**As a** health-conscious individual
**I want** to log water intake on my Apple Watch
**So that** I can stay hydrated with quick logging and reminders

## Facts
- Platform: watchOS 10+, Frameworks: HealthKit, SwiftUI, UserNotifications
- Storage: HealthKit samples, No cloud services

## Constraints
- TDD, HealthKit water samples, local notifications, complications, quick logging

## Goal State
Water tracker with one-tap logging, daily goals, hourly reminders, progress complication, HealthKit integration

## Acceptance Criteria
1. One-tap to log 250ml serving
2. Complication shows daily progress
3. Hourly reminders between 8am-8pm
4. Goal customization
5. Weekly statistics view

## Technical Notes
- HKQuantityType.dietaryWater
- UNUserNotificationCenter for reminders
- Complications for progress ring
