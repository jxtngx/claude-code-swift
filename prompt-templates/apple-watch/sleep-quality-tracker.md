# Sleep Quality Tracker - Apple Watch User Story

## User Story (INVEST)

**As a** sleep optimizer
**I want** sleep tracking with HRV and movement analysis
**So that** I can improve sleep quality

## Facts
- Platform: watchOS 10+, Frameworks: HealthKit, CoreMotion, SwiftUI
- Sensors: Heart rate, accelerometer, No cloud services

## Constraints
- TDD, HealthKit sleep samples, HRV analysis, movement detection, battery optimization for overnight

## Goal State
Sleep tracker monitoring sleep stages, HRV, movement, with morning summary and trends

## Acceptance Criteria
1. Automatic sleep detection
2. Sleep stage estimation (light/deep/REM)
3. HRV measurement during sleep
4. Movement/restlessness tracking
5. Morning sleep score

## Technical Notes
- HKCategoryType.sleepAnalysis
- Heart rate variability during sleep
- CMMotionManager for movement
- Battery-efficient overnight monitoring
