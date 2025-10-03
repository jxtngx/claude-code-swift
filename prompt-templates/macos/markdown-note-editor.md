# Markdown Note Editor - macOS User Story

## User Story (INVEST)

**As a** writer and knowledge worker
**I want** a privacy-focused markdown editor with live preview and local file management
**So that** I can write and organize notes without cloud sync or subscriptions

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with no dependencies
- **Negotiable**: Features can be adjusted (syntax themes, export formats, shortcuts)
- **Valuable**: Provides distraction-free writing with markdown support
- **Estimable**: Clear scope with well-defined AppKit and NaturalLanguage frameworks
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: AppKit, SwiftUI, UniformTypeIdentifiers
- **Secondary Frameworks**: SwiftData (metadata), NaturalLanguage (word count, readability)
- **File Formats**: Markdown (.md), Plain Text (.txt), HTML export, PDF export
- **Data Storage**: User-selected directory, no app sandbox restrictions for user files
- **No Cloud Services**: Fully local file system operations

## Constraints

- Follow TDD principles with XCTest
- Support standard markdown syntax (CommonMark spec)
- Implement syntax highlighting for code blocks
- Real-time preview with <100ms update latency
- Support macOS native file operations (Open, Save, Save As)
- Handle large documents efficiently (>10,000 lines)
- Use Swift Concurrency for async file operations
- Implement proper autosave and version management
- Support keyboard shortcuts for formatting
- Respect macOS typography and spacing conventions

## Penalties

- Laggy preview updates (>200ms)
- Missing standard markdown syntax support
- Poor error handling for file I/O failures
- Not supporting macOS standard keyboard shortcuts
- Memory leaks with large documents
- Missing autosave functionality
- Poor syntax highlighting readability
- Not handling file permission errors gracefully
- Blocking main thread during file operations
- Missing unit tests for markdown parsing

## Rewards

- High test coverage (>85%)
- Instant preview updates (<100ms)
- Distraction-free, clean interface
- Accurate markdown rendering
- Fast file operations
- Smooth scrolling with large documents
- Customizable syntax themes
- Privacy-preserving local-first architecture
- Standard keyboard shortcut support

## Goal State

Deliver a fully functional, privacy-focused markdown editor that:
- Provides split-pane markdown editing with live preview
- Supports CommonMark syntax with GFM extensions
- Includes syntax highlighting for code blocks (50+ languages)
- Offers customizable editor themes (light, dark, sepia)
- Implements word count, character count, reading time
- Exports to HTML, PDF, and plain text
- Supports macOS native file operations
- Stores recent documents and preferences locally
- Maintains >85% test coverage
- Handles documents up to 50,000 lines smoothly

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the user creates a new document
   **WHEN** they type markdown syntax
   **THEN** preview pane updates within 100ms with rendered HTML

2. **GIVEN** the user types a heading with `#` syntax
   **WHEN** preview renders
   **THEN** heading appears with correct size and styling per CommonMark spec

3. **GIVEN** the user inserts a code block with language identifier
   **WHEN** preview renders
   **THEN** syntax highlighting applies for the specified language

4. **GIVEN** the user wants to export
   **WHEN** they choose File → Export as PDF
   **THEN** markdown is rendered to PDF with proper formatting

5. **GIVEN** the user works on a document
   **WHEN** they haven't saved for 30 seconds
   **THEN** document autosaves to temporary location

### File Operations
6. **GIVEN** the user opens an existing markdown file
   **WHEN** file contains valid markdown
   **THEN** editor loads content and preview renders correctly

7. **GIVEN** the user attempts to save to a restricted directory
   **WHEN** permission is denied
   **THEN** app shows helpful error message and alternative save location

### Performance
8. **GIVEN** the user loads a 10,000-line markdown document
   **WHEN** editing and scrolling
   **THEN** UI remains responsive with no stuttering

9. **GIVEN** the user types continuously
   **WHEN** preview is visible
   **THEN** CPU usage stays <15% during active editing

### Accessibility
10. **GIVEN** user has enabled Increase Contrast
    **WHEN** they view the editor
    **THEN** syntax highlighting meets WCAG AA contrast ratios

## Technical Notes

### Required Frameworks
- **AppKit**: NSTextView for editing, NSSplitView for layout
- **SwiftUI**: Modern UI components and preferences window
- **UniformTypeIdentifiers**: File type handling for markdown
- **SwiftData**: Recent documents and user preferences
- **NaturalLanguage**: Word count, reading time estimation
- **WebKit**: Markdown preview rendering (WKWebView)

### Key Implementation Details
- Use `NSTextView` with custom `NSLayoutManager` for syntax highlighting
- Implement markdown-to-HTML parser using regex or `Down` library
- Use `WKWebView` for live preview with custom CSS
- Debounce preview updates to 100ms using Combine
- Implement `NSDocument` architecture for file management
- Use `FileManager` for autosave and temporary file handling
- Design reusable SwiftUI views for preferences and export options
- Implement proper error handling with `NSAlert` dialogs
- Use `NSFontPanel` for font customization
- Cache rendered markdown to reduce re-parsing

### Data Models
```
Document: fileURL, content, wordCount, characterCount, lastModified, isAutosaved
UserPreferences: editorTheme, previewCSS, fontSize, fontFamily, autosaveInterval, showWordCount
RecentDocument: fileURL, lastOpened, isFavorite
ExportOptions: format (html, pdf, txt), includeCSS, includeTOC, customTemplate
```

### Markdown Features Support
- **CommonMark**: Headings, emphasis, lists, links, images, code blocks
- **GFM Extensions**: Tables, task lists, strikethrough, autolinks
- **Syntax Highlighting**: 50+ languages via highlight.js or custom parser
- **Math**: Optional LaTeX support with MathJax or KaTeX
- **Diagrams**: Optional Mermaid diagram support

### Keyboard Shortcuts
- `⌘ + B`: Bold
- `⌘ + I`: Italic
- `⌘ + K`: Insert link
- `⌘ + Shift + K`: Insert code block
- `⌘ + 1-6`: Insert heading level
- `⌘ + L`: Insert list
- `⌘ + /`: Toggle preview
- `⌘ + E`: Export

### Testing Strategy
- Unit tests for markdown parser logic
- Unit tests for syntax highlighting rules
- Integration tests for file save/load operations
- Performance tests for large document handling
- UI tests for keyboard shortcut functionality
- Accessibility tests with VoiceOver
- Mock file system for consistent testing
- Snapshot tests for rendered output

### Privacy Considerations
- No data collection or telemetry
- All files stored locally at user-specified locations
- No network requests (fully offline)
- User preferences encrypted at rest
- No analytics or crash reporting
- Clear file permission request explanations
- User can export data anytime

### Advanced Features (Optional)
- Version history with Time Machine integration
- Custom CSS for preview styling
- Export templates for different formats
- Tag system with SwiftData
- Full-text search across all documents
- iCloud Drive integration (opt-in)
- Distraction-free mode (hide UI chrome)
- Focus mode (highlight current line/paragraph)
- Vim keybindings (opt-in)
- Custom markdown snippets
