# Stand Reminder - Apple Watch User Story

## User Story (INVEST)

**As a** desk worker
**I want** smart stand reminders based on activity
**So that** I can reduce sedentary time

## Facts
- Platform: watchOS 10+, Frameworks: CoreMotion, HealthKit, UserNotifications
- Sensors: Accelerometer, No cloud services

## Constraints
- TDD, motion detection, smart timing, respectful notifications, DND awareness

## Goal State
Stand reminder using motion activity, quiet hours, customizable frequency, stand goals, achievements

## Acceptance Criteria
1. Detects prolonged sitting
2. Sends gentle haptic reminder
3. Respects DND and sleep mode
4. Tracks daily stand hours
5. Customizable reminder frequency

## Technical Notes
- CMMotionActivityManager
- HKCategoryType.appleStandHour
- Smart notification timing
