# Heart Rate Zones Tracker - Apple Watch User Story

## User Story (INVEST)

**As an** athlete
**I want** heart rate zone tracking during workouts
**So that** I can train in target zones with haptic feedback

## Facts
- Platform: watchOS 10+, Frameworks: HealthKit, WorkoutKit, CoreHaptics
- Sensors: Heart rate monitor, No cloud services

## Constraints
- TDD, real-time HR monitoring, zone calculations, haptic alerts, HealthKit logging

## Goal State
HR zone tracker with 5 zones (rest, fat burn, cardio, peak, max), haptic transitions, time-in-zone stats

## Acceptance Criteria
1. Real-time HR zone display
2. Haptic pulse on zone change
3. Time spent in each zone
4. Post-workout zone distribution
5. Max HR calibration

## Technical Notes
- HKWorkoutSession for HR access
- Zone calculations based on max HR
- CHHapticEngine for zone alerts
