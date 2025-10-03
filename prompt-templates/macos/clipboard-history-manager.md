# Clipboard History Manager - macOS User Story

## User Story (INVEST)

**As a** developer and content creator
**I want** a clipboard history manager that stores my recent copy/paste actions
**So that** I can access previously copied text, images, and files without losing them

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with local storage
- **Negotiable**: Features can be adjusted (history length, file types, search)
- **Valuable**: Prevents data loss and improves copy/paste workflow
- **Estimable**: Clear scope with well-defined AppKit frameworks
- **Small**: Core features achievable in 1-2 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, UniformTypeIdentifiers, Combine
- **Secondary Frameworks**: SwiftData (clipboard history), CryptoKit (encryption)
- **Supported Types**: Text, images (PNG, JPEG), URLs, files, colors, rich text
- **Data Storage**: All clipboard items stored locally using SwiftData with encryption
- **No Cloud Services**: Fully on-device solution with no sync

## Constraints

- Follow TDD principles with XCTest
- Poll clipboard efficiently without excessive CPU usage
- Support keyboard shortcut for quick access (⌘ + Shift + V)
- Implement search across clipboard history
- Handle sensitive data with optional encryption
- Use Swift Concurrency for async clipboard monitoring
- Implement proper memory management for large images
- Support pinning frequently used items
- Exclude sensitive apps (password managers, banking)
- Respect macOS pasteboard type identifiers

## Penalties

- Excessive CPU usage from clipboard polling (>1% average)
- Storing passwords or sensitive data without user consent
- Poor error handling for clipboard access failures
- Memory leaks from cached images
- Slow search performance (>500ms)
- Not implementing clipboard exclusion rules
- Missing keyboard shortcut support
- Poor UI responsiveness with large history
- Not encrypting sensitive data at rest
- Missing unit tests for clipboard parsing

## Rewards

- High test coverage (>85%)
- CPU-efficient clipboard monitoring (<1% average)
- Instant search results (<100ms)
- Secure encryption for sensitive items
- Fast popup window display (<50ms)
- Customizable history retention
- Privacy-preserving local-first architecture
- Smart clipboard type detection
- Seamless integration with macOS

## Goal State

Deliver a fully functional, privacy-focused clipboard history manager that:
- Monitors clipboard changes in real-time
- Stores last 1000 clipboard items by default (configurable)
- Supports text, images, URLs, files, and rich text
- Provides keyboard shortcut (⌘ + Shift + V) for quick access
- Implements full-text search across history
- Allows pinning favorite items
- Encrypts sensitive data using CryptoKit
- Excludes clipboard from specified applications
- Auto-clears history after configurable period
- Maintains >85% test coverage
- Uses <1% CPU on average

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the user copies text
   **WHEN** clipboard changes
   **THEN** new entry appears in history within 500ms

2. **GIVEN** the user presses ⌘ + Shift + V
   **WHEN** popup window appears
   **THEN** most recent clipboard items display in reverse chronological order

3. **GIVEN** the user searches for "function"
   **WHEN** typing in search field
   **THEN** results filter to show only matching entries within 100ms

4. **GIVEN** the user clicks a history item
   **WHEN** selecting it
   **THEN** item is copied to clipboard and popup closes

5. **GIVEN** the user copies an image
   **WHEN** viewing history
   **THEN** thumbnail preview displays alongside timestamp

### Data Management
6. **GIVEN** the user has 1000 clipboard items
   **WHEN** copying a new item
   **THEN** oldest unpinned item is removed automatically

7. **GIVEN** the user enables "Exclude Password Managers"
   **WHEN** copying from 1Password
   **THEN** clipboard content is not saved to history

### Performance
8. **GIVEN** the app is monitoring clipboard
   **WHEN** running continuously
   **THEN** CPU usage remains <1% on average

9. **GIVEN** history contains 500 items with images
   **WHEN** user opens popup
   **THEN** window displays within 50ms

### Security
10. **GIVEN** user enables encryption
    **WHEN** clipboard items are saved
    **THEN** all data is encrypted at rest using CryptoKit

## Technical Notes

### Required Frameworks
- **AppKit**: NSPasteboard for clipboard access, NSWindow for popup
- **UniformTypeIdentifiers**: UTType for clipboard content type detection
- **SwiftData**: Clipboard history persistence
- **CryptoKit**: AES encryption for sensitive data
- **Combine**: Reactive clipboard monitoring
- **SwiftUI**: Modern UI for popup and preferences

### Key Implementation Details
- Use `NSPasteboard.general.changeCount` to detect clipboard changes
- Poll clipboard every 500ms using `Timer.publish` with Combine
- Extract clipboard types using `readObjects(forClasses:options:)`
- Store text as String, images as compressed Data, files as URLs
- Implement `Searchable` protocol for full-text search
- Use `NSEvent.addGlobalMonitorForEvents` for keyboard shortcut
- Design compact SwiftUI popup with list and search
- Implement thumbnail generation for images using `NSImage`
- Encrypt sensitive items using `AES.GCM` from CryptoKit
- Cache recent items in memory to reduce database hits

### Data Models
```
ClipboardItem: id, content (Data), contentType (UTType), timestamp, isPinned, isEncrypted, appBundleID, preview (String?)
UserPreferences: maxHistorySize, excludedApps, encryptionEnabled, searchShortcut, retentionDays, compressImages
ExcludedApp: bundleIdentifier, name, reason (password manager, banking, etc.)
```

### Supported Content Types
- **Text**: Plain text, rich text (RTF), attributed strings
- **Images**: PNG, JPEG, GIF, TIFF (with compression)
- **URLs**: Web URLs, file URLs
- **Files**: File paths and file promises
- **Colors**: NSColor representations
- **Code**: Source code with syntax detection

### Clipboard Monitoring Flow
1. Start timer polling `NSPasteboard.general` every 500ms
2. Check `changeCount` to detect changes
3. If changed, read available types from pasteboard
4. Extract content based on type priority (image > text > file > url)
5. Create `ClipboardItem` with metadata
6. Save to SwiftData with optional encryption
7. Update in-memory cache
8. Emit notification for UI update

### Testing Strategy
- Unit tests for clipboard type detection
- Unit tests for encryption/decryption logic
- Integration tests for SwiftData persistence
- Performance tests for search speed
- UI tests for keyboard shortcut functionality
- Memory leak tests with large images
- Mock NSPasteboard for consistent testing
- Accessibility tests with VoiceOver

### Privacy Considerations
- No data collection or telemetry
- All clipboard items stored locally
- Optional encryption for sensitive data
- Clear privacy policy in preferences
- Exclude password managers by default
- User can clear history anytime
- No network requests
- Data encrypted at rest with FileVault
- Transparent about clipboard monitoring

### Popup UI Features
- **Quick Search**: Type to filter results instantly
- **Keyboard Navigation**: Arrow keys to navigate, Enter to select
- **Preview Pane**: Show full content for selected item
- **Type Icons**: Visual indicators for text, image, file, URL
- **Pin Icon**: Star indicator for pinned items
- **Delete Button**: Remove individual items
- **Clear All**: Button to clear entire history
- **Preferences Access**: Gear icon to open settings

### Keyboard Shortcuts
- `⌘ + Shift + V`: Open clipboard history popup
- `⌘ + F`: Focus search field
- `↑/↓`: Navigate items
- `Enter`: Paste selected item
- `⌘ + Delete`: Delete selected item
- `⌘ + P`: Pin/unpin selected item
- `Esc`: Close popup

### Advanced Features (Optional)
- Sync favorites across Macs via iCloud (opt-in)
- Smart categorization (Code, URLs, Text, Images)
- Clipboard statistics (most copied items)
- Snippets feature (create reusable text templates)
- OCR for copied images (extract text)
- Format conversion (Markdown to HTML, JSON formatter)
- Duplicate detection (merge identical copies)
- Export history to JSON/CSV
- Custom keyboard shortcuts
- Menu bar icon showing clipboard count
- Quick paste menu in context menu
