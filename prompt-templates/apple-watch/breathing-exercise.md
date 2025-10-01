# Breathing Exercise - Apple Watch User Story

## User Story (INVEST)

**As a** mindfulness practitioner
**I want** guided breathing exercises on my Apple Watch
**So that** I can reduce stress with haptic and Digital Crown feedback

### INVEST Checklist
- Independent breathing app, negotiable patterns, valuable for wellness, estimable scope, small size, testable

## Facts
- Platform: watchOS 10+, Swift 6+
- Frameworks: HealthKit, CoreHaptics, WatchKit, SwiftUI
- Sensors: Heart rate, Digital Crown
- Haptics: Taptic Engine guidance
- No cloud services

## Constraints
- TDD, HealthKit mindful sessions, Digital Crown input, haptic breathing rhythm, Always-On compatible

## Goal State
Breathing app with 4-7-8 pattern, Digital Crown pace control, haptic guidance, HRV tracking, complication

## Acceptance Criteria
1. Haptic pulses guide inhale/exhale
2. Digital Crown adjusts breathing pace
3. Saves mindful session to HealthKit
4. Tracks heart rate variability
5. Complication shows session streak

## Technical Notes
- CHHapticPattern for breathing rhythm
- WKCrownSequencer for Digital Crown
- HKCategoryType.mindfulSession
- Heart rate monitoring during session
