# Screen Time Analyzer - macOS User Story

## User Story (INVEST)

**As a** productivity-focused professional
**I want** an app that tracks how I spend time across applications
**So that** I can understand my work patterns and improve focus without sending data to third parties

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with local tracking
- **Negotiable**: Metrics tracked can be customized (apps, websites, categories)
- **Valuable**: Provides actionable insights into productivity patterns
- **Estimable**: Clear scope with well-defined accessibility APIs
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, ApplicationServices, Accessibility
- **Secondary Frameworks**: SwiftData (activity persistence), Charts (visualizations)
- **System APIs**: NSWorkspace, AXUIElement, CGWindowListCopyWindowInfo
- **Data Storage**: All tracking data stored locally using SwiftData
- **No Cloud Services**: Fully on-device solution with no telemetry

## Constraints

- Follow TDD principles with XCTest
- Request accessibility permissions appropriately
- Update tracking efficiently without impacting system performance
- Support both active window and total app time tracking
- Handle permission denials gracefully
- Use Swift Concurrency for async tracking operations
- Implement proper data aggregation for long-term storage
- Respect user privacy with clear data handling policies
- Support data export for user portability
- Categorize apps intelligently (work, social, entertainment, utilities)

## Penalties

- Excessive CPU usage from tracking (>2% average)
- Privacy violations by tracking sensitive data
- Poor error handling for accessibility API failures
- Missing clear permission request explanations
- Not handling app renames or updates
- Memory leaks from continuous monitoring
- Blocking main thread during data aggregation
- Missing unit tests for time calculation logic
- Poor data visualization
- Not allowing users to exclude apps from tracking

## Rewards

- High test coverage (>85%)
- CPU-efficient tracking (<2% average CPU usage)
- Clear, actionable productivity insights
- Accurate time tracking with minute precision
- Fast data queries and visualizations
- Customizable app categories
- Privacy-preserving local-first architecture
- Comprehensive data export options
- Intuitive charts and reports

## Goal State

Deliver a fully functional, privacy-focused screen time analyzer that:
- Tracks active application usage by window focus
- Records total time spent per application daily/weekly/monthly
- Categorizes applications (Work, Social, Entertainment, Development, Utilities)
- Provides visualizations with bar charts, pie charts, and timelines
- Shows productivity trends over time
- Allows manual app category assignment
- Excludes specified apps from tracking
- Stores all data locally using SwiftData
- Maintains >85% test coverage
- Uses <2% CPU on average during tracking

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the app has accessibility permissions
   **WHEN** user switches between applications
   **THEN** time spent is recorded per application with minute accuracy

2. **GIVEN** the user has used multiple apps today
   **WHEN** they view the dashboard
   **THEN** a bar chart shows time spent per application

3. **GIVEN** the user wants to categorize apps
   **WHEN** they assign "Xcode" to "Development" category
   **THEN** Xcode appears under Development in all reports

4. **GIVEN** the user reviews weekly data
   **WHEN** they navigate to weekly view
   **THEN** aggregated time per app and category displays for past 7 days

5. **GIVEN** the user wants to exclude an app
   **WHEN** they add "System Settings" to exclusion list
   **THEN** time in System Settings is not tracked

### Data Persistence
6. **GIVEN** the app has tracked data for 30 days
   **WHEN** user quits and relaunches
   **THEN** all historical data loads correctly

7. **GIVEN** the user denies accessibility permissions
   **WHEN** they attempt to use the app
   **THEN** clear instructions show how to enable in System Settings

### Performance
8. **GIVEN** the app is running continuously
   **WHEN** tracking 10+ application switches per hour
   **THEN** app CPU usage remains <2% on average

9. **GIVEN** the app has 6 months of tracking data
   **WHEN** user views monthly report
   **THEN** query and visualization render in <1 second

### Accessibility
10. **GIVEN** VoiceOver is enabled
    **WHEN** user navigates charts
    **THEN** all data points have descriptive labels

## Technical Notes

### Required Frameworks
- **AppKit**: Main app window and menu bar presence
- **ApplicationServices**: Window and app focus tracking
- **Accessibility**: AXUIElement for focused window detection
- **SwiftData**: Local activity and preference persistence
- **Charts**: Native Swift Charts for data visualization
- **SwiftUI**: Modern UI for dashboard and reports

### Key Implementation Details
- Use `NSWorkspace.shared.notificationCenter` to observe app activation
- Implement `AXUIElementCopyAttributeValue` for focused window title
- Use `CGWindowListCopyWindowInfo` as fallback for window tracking
- Store activity snapshots every minute using SwiftData
- Aggregate snapshots into hourly/daily summaries for efficient queries
- Implement background timer with `Timer.scheduledTimer` for sampling
- Design reusable SwiftUI views for charts and statistics
- Use `@Observable` pattern for reactive data updates
- Cache aggregated data to reduce database queries
- Implement proper error handling with user-friendly alerts

### Data Models
```
ActivitySnapshot: timestamp, bundleIdentifier, appName, windowTitle, duration (60 seconds)
DailySummary: date, appBundleID, appName, totalSeconds, category
AppCategory: bundleIdentifier, name, categoryType, isExcluded, customColor
UserPreferences: trackingEnabled, excludedApps, updateInterval, chartType, weekStartDay
```

### App Categories
- **Work**: Productivity apps (Office, email, project management)
- **Development**: IDEs, terminals, version control
- **Social**: Messaging, social media, video calls
- **Entertainment**: Streaming, gaming, music
- **Utilities**: System tools, file management, settings
- **Creative**: Design, photo/video editing, music production
- **Research**: Browsers, documentation, reading
- **Uncategorized**: Default for unassigned apps

### Tracking Mechanism
1. Register for `NSWorkspace.didActivateApplicationNotification`
2. On app activation, record timestamp and app bundle ID
3. Every 60 seconds, check focused window using Accessibility APIs
4. Create `ActivitySnapshot` with app info and duration
5. Aggregate snapshots into `DailySummary` at end of day
6. Prune old snapshots after aggregation to conserve space

### Testing Strategy
- Unit tests for time calculation and aggregation logic
- Unit tests for category assignment rules
- Integration tests for SwiftData persistence
- Performance tests to verify <2% CPU usage
- UI tests for chart interaction
- Accessibility tests with VoiceOver
- Mock NSWorkspace notifications for consistent testing
- Data migration tests for schema updates

### Privacy Considerations
- No data collection or telemetry
- Window titles not stored (only app names)
- All data stays local on device
- Clear explanation of accessibility permission usage
- User can export and delete all data
- No network requests
- Data encrypted at rest using FileVault
- Option to pause tracking anytime

### Visualizations
- **Daily Chart**: Bar chart showing time per app for today
- **Weekly Chart**: Stacked bar chart showing daily breakdown by category
- **Monthly Chart**: Line chart showing trends over 30 days
- **Category Pie Chart**: Distribution of time across categories
- **Timeline View**: Hour-by-hour activity visualization
- **Top Apps**: Ranked list of most-used applications

### Advanced Features (Optional)
- Productivity score based on work vs. distraction ratio
- Focus sessions with automatic tracking pause
- Daily/weekly reports via local notifications
- Goal setting (e.g., "Limit social media to 1 hour/day")
- Browser extension for website-level tracking
- Compare current week to previous weeks
- Export to CSV for external analysis
- Custom date range queries
- Keyboard shortcuts for quick stats access
- Menu bar widget showing today's top app
