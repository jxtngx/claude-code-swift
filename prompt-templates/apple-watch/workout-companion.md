# Workout Companion - Apple Watch User Story

## User Story (INVEST)

**As a** fitness enthusiast
**I want** a workout tracker optimized for Apple Watch
**So that** I can monitor exercise metrics with real-time haptic feedback

### INVEST Checklist
- Independent workout app, negotiable workout types, valuable for fitness, estimable with HealthKit/CoreMotion, small scope, testable

## Facts
- Platform: watchOS 10+, Swift 6+
- Frameworks: HealthKit, WorkoutKit, CoreMotion, SwiftUI
- Sensors: Heart rate, accelerometer, GPS
- Storage: Local HealthKit database
- No cloud services

## Constraints
- TDD, on-device metrics, HealthKit integration, haptic zones, battery optimization, Always-On display support

## Goal State
Workout app tracking heart rate zones, distance, pace, calories with haptic feedback, voice announcements, complications

## Acceptance Criteria
1. Real-time heart rate zone monitoring
2. Haptic alerts when entering new HR zone
3. GPS tracking for outdoor workouts
4. Complication shows today's activity
5. Live Activity for ongoing workouts

## Technical Notes
- HKWorkoutSession for workout tracking
- CMPedometer for step counting
- CHHapticEngine for zone alerts
- Complications: modular, circular, corner
