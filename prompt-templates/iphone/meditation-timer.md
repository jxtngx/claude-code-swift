# Meditation Timer - iPhone User Story

## User Story (INVEST)

**As a** mindfulness practitioner
**I want** a meditation timer with guided breathing, ambient sounds, and haptic feedback
**So that** I can maintain a consistent practice with multi-sensory guidance without requiring an internet connection

### INVEST Checklist
- **Independent**: Self-contained meditation and mindfulness app
- **Negotiable**: Features adjustable (session lengths, sound types, breathing patterns)
- **Valuable**: Provides distraction-free, offline meditation experience
- **Estimable**: Well-defined scope using standard iOS frameworks
- **Small**: Core features achievable in 2 week sprint
- **Testable**: Clear criteria for timers, audio, and haptics

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Xcode**: 26+
- **Primary Frameworks**: AVFoundation, CoreHaptics, HealthKit, SwiftUI
- **Secondary Frameworks**: SwiftData, UserNotifications
- **Hardware**: iPhone speakers, Taptic Engine
- **Audio Files**: Bundled ambient sounds (no streaming)
- **Data Storage**: Session history in SwiftData
- **No Cloud Services**: Fully offline, on-device experience

## Constraints

- Follow TDD principles with XCTest
- All audio files bundled in app (no downloads)
- Implement proper audio session management
- Support background audio playback
- Request HealthKit mindful session permissions
- Handle audio interruptions gracefully
- Use CoreHaptics for breathing rhythm
- Support accessibility (VoiceOver, Dynamic Type)
- Implement DND-friendly local notifications
- Optimize battery usage during sessions

## Penalties

- Requiring internet connection for audio
- Poor audio session configuration
- Not handling audio interruptions
- Excessive haptic usage draining battery
- Missing error handling for audio failures
- Hardcoded session durations
- Poor timer accuracy
- Not saving session history
- Ignoring accessibility needs
- Intrusive notifications

## Rewards

- High test coverage (>85%)
- Soothing, high-quality ambient audio
- Precise haptic breathing guidance
- Accurate session timing
- Calming, minimalist interface
- Comprehensive session statistics
- Privacy-focused local-only data
- Background playback support
- Accessible meditation experience

## Goal State

Deliver a fully functional meditation timer that:
- Provides customizable session durations (5, 10, 15, 20, 30, 60 minutes)
- Plays bundled ambient sounds (rain, ocean, forest, white noise, silence)
- Guides breathing with haptic pulses and visual animations
- Tracks mindful sessions in HealthKit
- Stores session history locally in SwiftData
- Sends gentle local notifications at session end
- Supports background audio playback
- Provides session statistics and streaks
- Maintains >85% test coverage
- Creates peaceful, distraction-free experience

## Acceptance Criteria

### Session Configuration
1. **GIVEN** user opens the app
   **WHEN** they select session duration and ambient sound
   **THEN** configuration is saved for next launch

2. **GIVEN** user wants guided breathing
   **WHEN** they enable breathing guide
   **THEN** haptic pulses follow 4-7-8 breathing pattern (inhale-hold-exhale)

3. **GIVEN** user starts a session
   **WHEN** timer begins
   **THEN** ambient sound plays and breathing animation displays

### Audio Playback
4. **GIVEN** meditation session is active
   **WHEN** user receives a phone call
   **THEN** audio pauses and resumes after call ends

5. **GIVEN** user locks the phone during session
   **WHEN** session is in progress
   **THEN** audio continues playing in background

6. **GIVEN** user connects AirPods during session
   **WHEN** audio route changes
   **THEN** playback continues seamlessly on new audio route

### Haptic Guidance
7. **GIVEN** breathing guide is enabled
   **WHEN** inhale phase starts
   **THEN** gentle haptic ramp-up occurs over 4 seconds

8. **GIVEN** breathing guide is active
   **WHEN** exhale phase starts
   **THEN** gentle haptic ramp-down occurs over 8 seconds

### Session Completion
9. **GIVEN** meditation session completes
   **WHEN** timer reaches zero
   **THEN** gentle bell sound plays, session saves to HealthKit and SwiftData

10. **GIVEN** user completes a session
    **WHEN** session ends
    **THEN** summary shows duration, breathing cycles, and adds to streak

11. **GIVEN** session ends while app is in background
    **WHEN** timer completes
    **THEN** non-intrusive local notification appears

### Session History
12. **GIVEN** user has completed multiple sessions
    **WHEN** they view statistics
    **THEN** total time, session count, and streak are displayed

13. **GIVEN** user wants to review past sessions
    **WHEN** they access history
    **THEN** calendar view shows all completed sessions with durations

## Technical Notes

### Required Frameworks
- **AVFoundation**: Audio playback, session management
- **CoreHaptics**: Breathing rhythm haptic patterns
- **HealthKit**: Mindful session recording (HKCategoryType.mindfulSession)
- **SwiftUI**: Minimalist UI with breathing animations
- **SwiftData**: Session history storage
- **UserNotifications**: Session completion notifications

### Key Implementation Details
- Use `AVAudioPlayer` with bundled audio files
- Configure `AVAudioSession` with `.playback` category, `.mixWithOthers` option
- Implement `CHHapticEngine` with custom AHAP breathing patterns
- Create breathing pattern: inhale (4s), hold (7s), exhale (8s)
- Use `Timer.publish` for accurate countdown
- Save to HealthKit using `HKCategoryType.mindfulSession`
- Store sessions in SwiftData with duration and timestamp
- Implement circular breathing animation with SwiftUI
- Handle interruptions with `AVAudioSessionInterruptionNotification`
- Use `UNUserNotificationCenter` for gentle completion alerts

### Data Models
```
MeditationSession: id, duration, startTime, endTime, breathingGuideEnabled, ambientSound
UserPreferences: defaultDuration, defaultSound, breathingGuideEnabled, hapticIntensity
SessionStatistics: totalSessions, totalMinutes, currentStreak, longestStreak
```

### Bundled Audio Files
- **Rain**: 10-minute seamless loop, stereo, 256kbps AAC
- **Ocean Waves**: 10-minute loop, stereo, 256kbps AAC
- **Forest**: 10-minute loop, stereo, 256kbps AAC
- **White Noise**: 10-minute loop, mono, 128kbps AAC
- **Completion Bell**: 3-second chime, stereo, 256kbps AAC

### Haptic Pattern (4-7-8 Breathing)
```
Inhale (4 seconds): Continuous haptic, intensity ramp 0.0 → 0.7
Hold (7 seconds): Low continuous haptic, intensity 0.3
Exhale (8 seconds): Continuous haptic, intensity ramp 0.7 → 0.0
Pause (1 second): No haptic
```

### Testing Strategy
- Unit tests for timer accuracy
- Unit tests for breathing pattern timing
- Mock AVAudioPlayer for audio tests
- Mock CHHapticEngine for haptic tests
- Integration tests for HealthKit session saving
- UI tests for session start/stop flow
- Interruption tests (calls, audio route changes)
- Background playback tests
- Notification tests
- Accessibility tests with VoiceOver

### Privacy Considerations
- Request HealthKit mindful session permission
- All session data stored locally
- No analytics or usage tracking
- Audio files bundled (no network requests)
- User can delete session history
- HealthKit data respects user privacy settings
- No sharing or export features that leak data

### UX Considerations
- Minimalist, calming color palette
- Large, easy-to-tap controls
- Non-intrusive notifications
- Smooth, slow animations
- Clear session progress indicator
- Gentle, non-jarring sounds
- Optional haptics for silent meditation
- Dark mode optimized
- Reduced motion support
