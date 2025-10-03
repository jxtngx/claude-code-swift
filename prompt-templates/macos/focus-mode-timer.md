# Focus Mode Timer - macOS User Story

## User Story (INVEST)

**As a** knowledge worker seeking better focus
**I want** a Pomodoro timer that blocks distracting apps and enables Do Not Disturb
**So that** I can maintain deep work sessions without interruptions

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with system integration
- **Negotiable**: Features can be adjusted (timer durations, blocked apps, automation)
- **Valuable**: Improves productivity through enforced focus periods
- **Estimable**: Clear scope with well-defined macOS frameworks
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, UserNotifications, EventKit
- **Secondary Frameworks**: SwiftData (session persistence), AppleScript (app control)
- **System Integration**: Do Not Disturb, app blocking, notifications
- **Data Storage**: Session history and preferences stored locally using SwiftData
- **No Cloud Services**: Fully on-device solution

## Constraints

- Follow TDD principles with XCTest
- Respect user's ability to override blocks (not force-quit apps)
- Implement gentle app blocking (warnings, not force quit)
- Support customizable Pomodoro intervals (25/5, 50/10, 90/20)
- Handle Do Not Disturb API gracefully on different macOS versions
- Use Swift Concurrency for async timer operations
- Implement proper notification scheduling
- Support menu bar presence with timer display
- Allow emergency override for blocked apps
- Integrate with system Focus modes (macOS 13+)

## Penalties

- Force-quitting apps without user consent
- Not providing override mechanism
- Excessive CPU usage from app monitoring (>1% average)
- Poor error handling for notification failures
- Missing clear timer state indication
- Not respecting system Do Not Disturb settings
- Blocking system-critical applications
- Missing unit tests for timer logic
- Poor accessibility for timer controls
- Not persisting session history

## Rewards

- High test coverage (>85%)
- CPU-efficient monitoring (<1% average)
- Clear visual timer feedback
- Seamless Do Not Disturb integration
- Flexible Pomodoro customization
- Privacy-preserving local-first architecture
- Comprehensive session statistics
- Gentle, non-intrusive blocking
- Keyboard shortcut support

## Goal State

Deliver a fully functional, respectful focus timer that:
- Implements classic Pomodoro technique (25min work, 5min break)
- Blocks specified distracting apps during focus sessions
- Enables macOS Do Not Disturb automatically during focus
- Sends notifications for session start/end
- Displays countdown in menu bar
- Tracks completed Pomodoro sessions with statistics
- Allows customizable focus/break durations
- Provides emergency override for blocked apps
- Stores session history locally using SwiftData
- Maintains >85% test coverage
- Uses <1% CPU on average

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the user starts a focus session
   **WHEN** timer begins
   **THEN** Do Not Disturb enables and menu bar shows countdown

2. **GIVEN** a focus session is active
   **WHEN** user attempts to launch a blocked app
   **THEN** gentle alert appears with option to override or refocus

3. **GIVEN** a 25-minute focus session completes
   **WHEN** timer ends
   **THEN** notification appears and 5-minute break timer starts automatically

4. **GIVEN** the user is on a break
   **WHEN** break timer ends
   **THEN** notification suggests starting next focus session

5. **GIVEN** the user wants to view statistics
   **WHEN** they open the stats window
   **THEN** completed sessions, total focus time, and daily streaks display

### Timer Management
6. **GIVEN** a focus session is running
   **WHEN** user clicks "Skip Break"
   **THEN** break is skipped and new focus session starts immediately

7. **GIVEN** the user needs emergency access to blocked app
   **WHEN** they select "Override for this session"
   **THEN** app unblocks for current session only

### Performance
8. **GIVEN** app is monitoring blocked applications
   **WHEN** running continuously
   **THEN** CPU usage remains <1% on average

9. **GIVEN** the app has 6 months of session history
   **WHEN** user views statistics
   **THEN** data loads and renders within 1 second

### Accessibility
10. **GIVEN** VoiceOver is enabled
    **WHEN** user navigates timer controls
    **THEN** all buttons and timer state have descriptive labels

## Technical Notes

### Required Frameworks
- **AppKit**: NSStatusBar for menu bar, NSWindow for main interface
- **UserNotifications**: Local notifications for session transitions
- **SwiftData**: Session history and preferences persistence
- **SwiftUI**: Modern UI for preferences and statistics
- **EventKit**: Optional calendar integration for blocking time
- **AppleScript**: App launching detection and gentle blocking

### Key Implementation Details
- Use `NSStatusItem` for menu bar timer display
- Implement `Timer.publish` with Combine for countdown
- Monitor app launches via `NSWorkspace.didLaunchApplicationNotification`
- Use `NSAppleScript` to bring focus back from blocked apps
- Display `NSAlert` when user attempts to launch blocked app
- Enable Do Not Disturb via private API or shortcut automation
- Store completed sessions with timestamps in SwiftData
- Design reusable SwiftUI views for statistics charts
- Implement state machine for session phases (focus/break/idle)
- Cache current session state to survive app restarts

### Data Models
```
FocusSession: id, startTime, endTime, duration, wasCompleted, sessionType (focus/break), interruptions
UserPreferences: focusDuration, breakDuration, longBreakDuration, longBreakInterval, blockedApps, enableDND, soundEnabled
BlockedApp: bundleIdentifier, name, isTemporarilyUnblocked, customMessage
SessionStatistics: date, completedSessions, totalFocusMinutes, longestStreak, currentStreak
```

### Pomodoro Variants Support
- **Classic**: 25 min focus, 5 min break, long break (15 min) every 4 sessions
- **Short**: 15 min focus, 3 min break
- **Extended**: 50 min focus, 10 min break
- **Deep Work**: 90 min focus, 20 min break
- **Custom**: User-defined intervals

### Session State Machine
```
Idle → Focus → Break → Focus → Break → Focus → Break → Focus → Long Break → Idle
```

### App Blocking Mechanism
1. Register for `NSWorkspace.didLaunchApplicationNotification`
2. Check if launched app is in blocked list
3. If blocked and session active, show `NSAlert` with options:
   - "Stay Focused" (default): Close alert, don't launch app
   - "Override for Session": Allow app this session only
   - "Remove from Blocked": Permanently unblock
4. If "Stay Focused", use `NSAppleScript` to activate previous app
5. Log interruption in `FocusSession` for statistics

### Testing Strategy
- Unit tests for timer countdown logic
- Unit tests for session state transitions
- Integration tests for SwiftData persistence
- Performance tests for monitoring overhead
- UI tests for timer controls and interruption alerts
- Notification tests with UNUserNotificationCenter
- Mock NSWorkspace for app launch testing
- Accessibility tests with VoiceOver

### Privacy Considerations
- No data collection or telemetry
- All session data stored locally
- Blocked app list never shared
- Clear explanation of app monitoring
- No screenshots or window content access
- User can delete session history anytime
- No network requests
- Transparent about system integration

### Menu Bar Display
- **Timer View**: "25:00" countdown
- **Session Indicator**: Color coding (red = focus, green = break)
- **Quick Actions**: Start/Pause/Skip in dropdown
- **Statistics**: Quick view of today's completed sessions
- **Preferences**: Access to settings

### Notifications
- **Session Start**: "Focus session started. Stay focused!"
- **Session End**: "Great work! Time for a break."
- **Break End**: "Break over. Ready for another session?"
- **Milestone**: "You've completed 4 sessions today!"
- **Interruption**: "App blocked during focus. Stay on track!"

### Keyboard Shortcuts
- `⌘ + Shift + F`: Start/pause focus session
- `⌘ + Shift + B`: Start break
- `⌘ + Shift + S`: Skip current timer
- `⌘ + Shift + R`: Reset timer
- `⌘ + ,`: Open preferences

### Advanced Features (Optional)
- Website blocking via hosts file or DNS filtering
- Spotify/Apple Music integration (play focus playlists)
- Integrate with macOS Focus modes
- Calendar blocking (create "Focus Time" events)
- Daily focus goals with progress tracking
- Achievement system (badges for streaks)
- Ambient sounds during focus (white noise, nature)
- Export statistics to CSV
- Team mode (sync focus sessions with colleagues)
- Smart scheduling (suggest best focus times based on history)
- Break activity suggestions (stretches, hydration reminders)
