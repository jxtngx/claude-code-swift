# Menu Bar System Monitor - macOS User Story

## User Story (INVEST)

**As a** power user and developer
**I want** a lightweight menu bar app that displays real-time system metrics
**So that** I can monitor CPU, memory, network, and disk usage without opening Activity Monitor

### INVEST Checklist
- **Independent**: Fully self-contained macOS menu bar app
- **Negotiable**: Metrics displayed can be customized (CPU, RAM, network, disk, battery)
- **Valuable**: Provides instant system visibility for performance monitoring
- **Estimable**: Clear scope with well-defined macOS frameworks
- **Small**: Core features achievable in 1-2 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, Foundation, System
- **Secondary Frameworks**: SwiftData (settings persistence), UserNotifications
- **System APIs**: NSProcessInfo, NSTask, sysctl, IOKit
- **Data Storage**: User preferences stored locally using SwiftData
- **No Cloud Services**: Fully on-device solution

## Constraints

- Follow TDD principles with XCTest
- Menu bar icon must be minimal and readable at 22x22 pixels
- Update frequency must balance accuracy with CPU efficiency (1-2 second intervals)
- Support both light and dark menu bar appearance
- Handle permission requests gracefully (disk access, accessibility)
- Use Swift Concurrency for async metric collection
- Implement proper memory management to avoid leaks
- Support macOS System Settings integration
- Provide keyboard shortcuts for common actions
- Support multiple display configurations

## Penalties

- Excessive CPU usage from monitoring itself (>1% CPU average)
- Memory leaks from continuous monitoring
- Blocking main thread during metric collection
- Poor handling of permission denials
- Cluttered menu bar interface
- Missing error handling for system API failures
- Hardcoded refresh intervals
- Not respecting reduced transparency accessibility setting
- Poor menu organization
- Missing unit tests for metric calculations

## Rewards

- High test coverage (>85%)
- CPU-efficient monitoring (<1% average CPU usage)
- Clean, readable menu bar interface
- Accurate real-time metrics
- Fast menu display (<100ms)
- Smooth animations and transitions
- Keyboard shortcut accessibility
- Privacy-preserving local-first architecture
- Customizable metric display

## Goal State

Deliver a fully functional, efficient system monitoring menu bar app that:
- Displays real-time CPU usage (per-core and total)
- Shows memory usage (used, available, pressure)
- Monitors network activity (upload/download speeds)
- Tracks disk I/O and available space
- Shows battery status and time remaining (for laptops)
- Provides detailed popover with graphs and history
- Allows customization of displayed metrics
- Stores preferences locally using SwiftData
- Maintains >85% test coverage
- Uses <1% CPU on average

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the app is launched for the first time
   **WHEN** user opens the app
   **THEN** a menu bar icon appears with current CPU percentage

2. **GIVEN** the system is under load
   **WHEN** CPU usage increases
   **THEN** menu bar displays updated CPU percentage within 2 seconds

3. **GIVEN** the user clicks the menu bar icon
   **WHEN** menu opens
   **THEN** detailed metrics are displayed for CPU, RAM, network, and disk

4. **GIVEN** the user wants to customize metrics
   **WHEN** they access preferences
   **THEN** they can toggle which metrics appear in menu bar and menu

5. **GIVEN** the user hovers over a metric
   **WHEN** they view the detailed popover
   **THEN** a mini graph shows last 60 seconds of history

### Data Persistence
6. **GIVEN** the user customizes preferences
   **WHEN** app is restarted
   **THEN** all preferences are restored from local storage

7. **GIVEN** the system denies disk access
   **WHEN** user attempts to view disk metrics
   **THEN** app shows helpful message with instructions to enable in System Settings

### Performance
8. **GIVEN** the app is running continuously
   **WHEN** monitoring all metrics
   **THEN** app CPU usage is <1% on average

9. **GIVEN** the app has been running for 24 hours
   **WHEN** checking memory usage
   **THEN** memory footprint remains <50MB with no leaks

### Accessibility
10. **GIVEN** user has enabled reduced transparency
    **WHEN** they view the menu
    **THEN** all UI elements respect accessibility settings

## Technical Notes

### Required Frameworks
- **AppKit**: NSStatusBar, NSMenu, NSPopover for menu bar UI
- **Foundation**: ProcessInfo, FileManager for system metrics
- **System**: Process metrics via Darwin APIs
- **SwiftData**: Local preference persistence
- **IOKit**: Battery and hardware information
- **Network**: Network activity monitoring

### Key Implementation Details
- Use `NSStatusBar.system.statusItem` for menu bar presence
- Implement `HOST_VM_INFO64` sysctl for memory statistics
- Use `ProcessInfo.processInfo.thermalState` for thermal monitoring
- Query network interfaces via `getifaddrs()` for traffic stats
- Implement `IOPSCopyPowerSourcesInfo` for battery metrics
- Use `Timer.publish` with Combine for metric refresh cycles
- Design reusable SwiftUI views for graphs in popover
- Implement proper error handling with user-friendly messages
- Use `@MainActor` to ensure UI updates on main thread
- Cache metric calculations to reduce system calls

### Data Models
```
SystemMetrics: timestamp, cpuUsage, memoryUsed, memoryAvailable, networkUp, networkDown, diskRead, diskWrite
UserPreferences: showCPU, showMemory, showNetwork, showDisk, showBattery, refreshInterval, menuBarDisplayMode
MetricHistory: metricType, values (last 60 entries), maxCapacity
```

### Menu Bar Display Options
- Compact: Single value (e.g., "45%")
- Icon + Value: Small icon with percentage
- Graph: Mini sparkline chart
- Multi-metric: Multiple values separated (e.g., "CPU 45% | RAM 8GB")

### Testing Strategy
- Unit tests for metric calculation logic
- Unit tests for sysctl wrapper functions
- Integration tests for SwiftData persistence
- Performance tests to verify <1% CPU usage
- Memory leak tests with Instruments
- UI tests for menu interaction
- Mock system data for consistent testing
- Accessibility tests with VoiceOver

### Privacy Considerations
- No data collection or telemetry
- All metrics read from public system APIs
- No network requests (purely local monitoring)
- User preferences encrypted at rest using iOS data protection
- No data sharing with third parties
- Clearly explain any permission requests
- User can completely quit and remove app

### Advanced Features (Optional)
- Export metrics to CSV
- Set alerts for threshold violations (e.g., CPU > 80%)
- Custom keyboard shortcuts
- Launch at login preference
- Historical data retention (7/30 days)
- Process-level CPU usage breakdown
- GPU usage monitoring (Apple Silicon)
- Temperature sensors (via SMC)
