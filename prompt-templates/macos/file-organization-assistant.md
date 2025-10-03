# File Organization Assistant - macOS User Story

## User Story (INVEST)

**As a** user with a cluttered Downloads folder and desktop
**I want** an automated file organization tool that sorts files intelligently
**So that** I can maintain an organized file system without manual effort

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with local file operations
- **Negotiable**: Features can be adjusted (rules, destinations, file types)
- **Valuable**: Saves time and reduces file system clutter
- **Estimable**: Clear scope with well-defined FileManager APIs
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, Foundation, UniformTypeIdentifiers, Vision
- **Secondary Frameworks**: SwiftData (rules persistence), NaturalLanguage (text analysis)
- **File Operations**: Move, copy, rename, tag, OCR for document categorization
- **Data Storage**: Organization rules and history stored locally using SwiftData
- **No Cloud Services**: Fully on-device file operations and OCR

## Constraints

- Follow TDD principles with XCTest
- Request appropriate file access permissions
- Implement safe file operations with undo capability
- Support drag-and-drop for folder selection
- Handle file conflicts gracefully (duplicates, name collisions)
- Use Swift Concurrency for async file operations
- Implement Vision OCR for PDF/image document categorization
- Support custom organization rules
- Respect Finder tags and comments
- Never permanently delete files without explicit permission

## Penalties

- Deleting files without user confirmation
- Overwriting files without conflict resolution
- Excessive CPU usage during file scanning (>10%)
- Poor error handling for permission failures
- Missing undo functionality
- Not handling symbolic links correctly
- Blocking main thread during file operations
- Missing unit tests for rule matching logic
- Poor progress feedback during batch operations
- Not preserving file metadata (tags, creation date)

## Rewards

- High test coverage (>85%)
- Safe file operations with full undo support
- Intelligent file categorization using OCR
- Fast batch processing (100+ files/minute)
- Clear progress visualization
- Customizable organization rules
- Privacy-preserving local-first architecture
- Comprehensive operation history
- Preserved file metadata and tags

## Goal State

Deliver a fully functional, safe file organization assistant that:
- Monitors specified folders (Downloads, Desktop, Documents)
- Automatically organizes files based on type, content, and date
- Uses Vision OCR to categorize documents by content
- Supports custom organization rules with pattern matching
- Provides drag-and-drop folder configuration
- Shows real-time progress during batch operations
- Maintains operation history with undo capability
- Preserves file tags, labels, and metadata
- Stores rules and history locally using SwiftData
- Maintains >85% test coverage
- Processes 100+ files/minute

## Acceptance Criteria

### Core Functionality
1. **GIVEN** a new file appears in monitored Downloads folder
   **WHEN** file is a PDF
   **THEN** file moves to ~/Documents/PDFs/ within 5 seconds

2. **GIVEN** user sets up a custom rule: "*.sketch → ~/Design/Sketch/"
   **WHEN** a .sketch file is added to monitored folder
   **THEN** file moves to specified destination matching the rule

3. **GIVEN** a scanned receipt PDF with text "invoice"
   **WHEN** Vision OCR processes the document
   **THEN** file is categorized as "Invoice" and moved to ~/Documents/Invoices/

4. **GIVEN** two files with the same name exist
   **WHEN** organization attempts to move duplicate
   **THEN** conflict dialog appears with options: skip, rename, replace

5. **GIVEN** user organized 50 files
   **WHEN** they click "Undo Last Operation"
   **THEN** all 50 files return to original locations

### Rule Management
6. **GIVEN** user creates rule "Photos from 2024 → ~/Photos/2024/"
   **WHEN** JPEG files from 2024 are detected
   **THEN** files move to specified yearly folder

7. **GIVEN** user wants to test a rule before applying
   **WHEN** they enable "Preview Mode"
   **THEN** app shows what would happen without moving files

### Performance
8. **GIVEN** 500 files need organization
   **WHEN** batch operation runs
   **THEN** all files process in <5 minutes (100+ files/minute)

9. **GIVEN** OCR is processing a 20-page PDF
   **WHEN** scanning for keywords
   **THEN** operation completes in <10 seconds

### Safety
10. **GIVEN** file operation fails mid-process
    **WHEN** error occurs
    **THEN** all completed operations can be undone and original state restored

## Technical Notes

### Required Frameworks
- **AppKit**: Main app window and drag-drop interfaces
- **Foundation**: FileManager for file operations
- **UniformTypeIdentifiers**: UTType for file type detection
- **SwiftData**: Rule and history persistence
- **Vision**: VNRecognizeTextRequest for document OCR
- **NaturalLanguage**: Keyword extraction for categorization
- **SwiftUI**: Modern UI for rules and history views

### Key Implementation Details
- Use `FileManager.default` for all file operations
- Monitor folders via `FSEvents` API or `DispatchSource.makeFileSystemObjectSource`
- Implement `VNRecognizeTextRequest` for PDF and image OCR
- Extract keywords using `NLTagger` with `nameTypeOrLexicalClass` scheme
- Store file operations in undo stack with original paths
- Design rule engine with regex pattern matching
- Use `OperationQueue` for concurrent file processing
- Implement safe file move with temporary staging
- Preserve extended attributes using `getxattr`/`setxattr`
- Cache OCR results to avoid re-processing

### Data Models
```
OrganizationRule: id, name, pattern, destinationPath, fileTypeFilter, priority, isEnabled, lastUsed
FileOperation: id, timestamp, sourceURL, destinationURL, operationType (move/copy/rename), canUndo, metadata
FileMetadata: url, type, size, creationDate, modificationDate, tags, keywords
UserPreferences: monitoredFolders, enableAutoOrganize, ocrEnabled, conflictResolution, undoHistoryLimit
```

### Default Organization Rules
- **Images**: `*.{jpg,png,heic}` → `~/Pictures/`
- **Videos**: `*.{mov,mp4}` → `~/Movies/`
- **Documents**: `*.{pdf,doc,docx}` → `~/Documents/`
- **Archives**: `*.{zip,tar,gz}` → `~/Downloads/Archives/`
- **Code**: `*.{swift,js,py}` → `~/Developer/`
- **Music**: `*.{mp3,m4a,flac}` → `~/Music/`
- **Screenshots**: `Screenshot*.png` → `~/Pictures/Screenshots/`

### OCR Categorization Logic
1. Extract text from PDF/image using Vision framework
2. Tokenize text with `NLTokenizer`
3. Extract key entities using `NLTagger` (person, organization, location)
4. Identify document type keywords:
   - Invoice: "invoice", "amount due", "payment"
   - Receipt: "receipt", "total", "purchased"
   - Contract: "agreement", "terms", "signed"
   - Resume: "experience", "education", "skills"
5. Match keywords to predefined categories
6. Move to category-specific folder

### File Conflict Resolution Strategies
- **Skip**: Don't move conflicting file
- **Rename**: Append number suffix (file-1.pdf, file-2.pdf)
- **Replace**: Overwrite existing file (with confirmation)
- **Keep Both**: Rename new file with timestamp
- **Smart Merge**: For duplicates, keep newest or largest

### Testing Strategy
- Unit tests for rule pattern matching
- Unit tests for OCR keyword extraction
- Integration tests for file move operations
- Integration tests for SwiftData persistence
- Performance tests for batch processing speed
- UI tests for drag-drop folder configuration
- Safety tests for undo/redo functionality
- Mock FileManager for consistent testing

### Privacy Considerations
- No data collection or telemetry
- OCR processing happens entirely on-device
- File paths never leave the system
- No network requests
- Clear explanation of folder access permissions
- User controls which folders are monitored
- Operation history can be cleared anytime

### Safety Mechanisms
- **Undo Stack**: Store last 100 operations with full reversal capability
- **Dry Run Mode**: Preview changes without executing
- **Trash Integration**: Option to move conflicts to Trash instead of replacing
- **Operation Logging**: Detailed log of all file operations
- **Rollback on Error**: Atomic operations with automatic rollback
- **Backup Prompt**: Suggest Time Machine backup before large operations

### UI Components
- **Main Window**: Monitored folders list, recent operations
- **Rules Panel**: Create, edit, reorder organization rules
- **History View**: Searchable log of all file operations
- **Progress Window**: Real-time progress with file counts and speed
- **Conflict Dialog**: User-friendly conflict resolution options
- **Preferences**: Enable/disable auto-organize, OCR, monitors

### Keyboard Shortcuts
- `⌘ + N`: New organization rule
- `⌘ + R`: Run organization now
- `⌘ + Z`: Undo last operation
- `⌘ + Shift + Z`: Redo operation
- `⌘ + ,`: Open preferences
- `⌘ + L`: View operation history

### Advanced Features (Optional)
- Smart date-based folders (organize by year/month)
- Tag-based organization (use existing Finder tags)
- Duplicate file detection and cleanup
- Large file archiving (compress old files)
- Cloud folder organization (iCloud Drive, Dropbox)
- Scheduled organization (daily cleanup at specific time)
- Email attachment auto-save and organize
- Browser download integration
- Scriptable rules with JavaScript
- Team shared rules (export/import rule sets)
- Machine learning for custom categorization
- Integration with Hazel-like automation
