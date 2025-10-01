# Fitness Activity Tracker - iPhone User Story

## User Story (INVEST)

**As a** fitness enthusiast
**I want** a simple activity tracker that monitors my daily movement, steps, and workouts
**So that** I can track my fitness progress and stay motivated without sharing my data with external services

### INVEST Checklist
- **Independent**: Fully self-contained on-device app
- **Negotiable**: Features can be adjusted (calorie tracking, workout types, goals)
- **Valuable**: Provides immediate value through privacy-focused fitness tracking
- **Estimable**: Clear scope with well-defined frameworks
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Xcode**: 26+
- **Primary Frameworks**: HealthKit, CoreMotion, SwiftUI
- **Secondary Frameworks**: SwiftData (local persistence), UserNotifications
- **Hardware**: iPhone accelerometer, gyroscope, pedometer
- **Data Storage**: All data stored locally on device using SwiftData
- **No Cloud Services**: Fully on-device solution

## Constraints

- Follow TDD principles with XCTest
- All data must remain on-device (no cloud sync)
- Request minimal HealthKit permissions (steps, distance, workouts, active energy)
- Implement proper battery optimization for continuous tracking
- Support accessibility (VoiceOver, Dynamic Type, reduced motion)
- Handle HealthKit authorization gracefully when denied
- Use Swift Concurrency for async operations
- Implement proper data encryption for health data
- Support background updates efficiently

## Penalties

- Using cloud services or external APIs
- Requesting unnecessary HealthKit permissions
- Excessive battery drain from background processing
- Poor error handling for HealthKit failures
- Ignoring accessibility requirements
- Not implementing proper data validation
- Hardcoded values instead of configuration
- Missing unit tests for core functionality
- Poor user feedback during data sync
- Not handling device capability variations

## Rewards

- High test coverage (>85%)
- Battery-efficient background tracking
- Intuitive, accessible user interface
- Seamless HealthKit integration
- Fast app launch and data loading
- Smooth animations and transitions
- Clear data visualizations
- Privacy-preserving local-first architecture
- Robust offline functionality

## Goal State

Deliver a fully functional, privacy-focused fitness tracking app that:
- Tracks daily steps, distance, and active minutes using HealthKit
- Records manual workouts with duration, type, and intensity
- Displays daily, weekly, and monthly activity summaries
- Shows progress toward customizable daily goals
- Sends local notifications for goal achievements
- Stores all data locally using SwiftData
- Provides accessible, intuitive SwiftUI interface
- Maintains >85% test coverage
- Optimizes battery usage for background tracking

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the app is launched for the first time
   **WHEN** user grants HealthKit permissions
   **THEN** app displays current day's step count and distance

2. **GIVEN** the user is walking with their iPhone
   **WHEN** the pedometer detects steps
   **THEN** the step count updates in real-time on the main screen

3. **GIVEN** the user wants to log a workout
   **WHEN** they select workout type, duration, and intensity
   **THEN** the workout is saved to HealthKit and displayed in workout history

4. **GIVEN** the user has completed their daily step goal
   **WHEN** the goal is achieved
   **THEN** a local notification is sent and haptic feedback is triggered

5. **GIVEN** the user views weekly statistics
   **WHEN** they navigate to the stats screen
   **THEN** a chart displays daily step counts for the past 7 days

### Data Persistence
6. **GIVEN** the app is closed and reopened
   **WHEN** user views their data
   **THEN** all workout history and goals are persisted locally

7. **GIVEN** the user has denied HealthKit permissions
   **WHEN** they attempt to track activity
   **THEN** app shows helpful message with instructions to enable in Settings

### Performance
8. **GIVEN** the app is running in background
   **WHEN** monitoring step count
   **THEN** battery drain is <5% over 8 hours of tracking

9. **GIVEN** the app has 6 months of workout data
   **WHEN** user opens the app
   **THEN** launch time is <2 seconds

### Accessibility
10. **GIVEN** VoiceOver is enabled
    **WHEN** user navigates the app
    **THEN** all UI elements have descriptive labels and hints

## Technical Notes

### Required Frameworks
- **HealthKit**: Step count, distance, workouts, active energy queries
- **CoreMotion**: Pedometer data, motion activity
- **SwiftUI**: Declarative UI with @Observable pattern
- **SwiftData**: Local workout and goal persistence
- **UserNotifications**: Local notifications for goal achievements
- **CoreHaptics**: Haptic feedback for milestones

### Key Implementation Details
- Use `HKHealthStore` for HealthKit authorization and queries
- Implement `HKObserverQuery` for real-time step count updates
- Use `CMPedometer` for step counting when HealthKit unavailable
- Store custom workout data and goals in SwiftData models
- Implement `HKStatisticsCollectionQuery` for weekly/monthly summaries
- Use `BGTaskScheduler` for efficient background updates
- Design reusable SwiftUI components for charts and statistics
- Implement proper error handling with user-friendly messages

### Data Models
```
Workout: id, type, duration, startDate, endDate, caloriesBurned, notes
DailyGoal: id, stepTarget, distanceTarget, activeMinutesTarget, date
ActivitySummary: id, date, steps, distance, activeMinutes, caloriesBurned
```

### Testing Strategy
- Unit tests for HealthKit query logic
- Unit tests for goal calculation and validation
- Integration tests for SwiftData persistence
- UI tests for workout creation flow
- Performance tests for data loading with large datasets
- Accessibility tests with VoiceOver
- Mock HealthKit data for consistent testing

### Privacy Considerations
- Request only necessary HealthKit permissions
- Explain data usage clearly in permission prompts
- All data encrypted at rest using iOS data protection
- No telemetry or analytics collection
- No data sharing with third parties
- User can delete all data locally
