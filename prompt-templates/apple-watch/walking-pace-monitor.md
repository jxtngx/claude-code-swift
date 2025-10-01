# Walking Pace Monitor - Apple Watch User Story

## User Story (INVEST)

**As a** walker
**I want** real-time pace monitoring with audio/haptic cues
**So that** I can maintain target walking speed

## Facts
- Platform: watchOS 10+, Frameworks: CoreLocation, CoreMotion, HealthKit, AVFoundation
- Sensors: GPS, accelerometer, No cloud services

## Constraints
- TDD, GPS tracking, pace zones, haptic/audio feedback, battery optimization

## Goal State
Pace monitor with target zones, real-time feedback, route tracking, pace history

## Acceptance Criteria
1. Real-time pace calculation
2. Haptic alerts when pace deviates
3. Audio announcements every kilometer
4. Route map saved locally
5. Pace statistics and trends

## Technical Notes
- CLLocationManager for GPS
- CMPedometer for cadence
- AVSpeechSynthesizer for announcements
