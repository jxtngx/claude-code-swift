# Voice Command Automator - macOS User Story

## User Story (INVEST)

**As a** productivity enthusiast who prefers hands-free control
**I want** a voice-activated automation tool that executes custom Mac commands
**So that** I can control my Mac with spoken phrases without using Siri or cloud services

### INVEST Checklist
- **Independent**: Fully self-contained macOS app with local speech processing
- **Negotiable**: Features can be adjusted (commands, trigger words, actions)
- **Valuable**: Provides hands-free Mac control with custom automation
- **Estimable**: Clear scope with well-defined Speech and AppleScript frameworks
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear acceptance criteria with measurable outcomes

## Facts

- **Platform**: macOS 14.0+
- **Swift Version**: 6+
- **Xcode**: 16+
- **Primary Frameworks**: Speech, AppKit, AppleScript
- **Secondary Frameworks**: SwiftData (command persistence), Automator, Shortcuts
- **Processing**: On-device speech recognition (no internet required)
- **Supported Actions**: App launching, AppleScript execution, Shortcuts, shell commands
- **Data Storage**: Voice commands and preferences stored locally using SwiftData
- **No Cloud Services**: Fully on-device speech recognition and command execution

## Constraints

- Follow TDD principles with XCTest
- Request microphone permission appropriately
- Support push-to-talk and continuous listening modes
- Implement customizable wake word (e.g., "Hey Computer")
- Handle speech recognition errors gracefully
- Use Swift Concurrency for async speech processing
- Execute commands safely with confirmation for destructive actions
- Support custom vocabulary for technical terms
- Respect user privacy with local-only processing
- Provide visual feedback for speech recognition state

## Penalties

- Sending audio to cloud services
- Executing destructive commands without confirmation
- Poor error handling for microphone failures
- Missing wake word customization
- Excessive CPU usage during continuous listening (>5%)
- Not implementing command confirmation for risky actions
- Poor speech recognition accuracy (<85%)
- Missing visual feedback during listening
- Not supporting custom vocabulary
- Missing unit tests for command parsing logic

## Rewards

- High test coverage (>85%)
- CPU-efficient continuous listening (<5% average)
- High speech recognition accuracy (>90%)
- Instant command execution (<500ms after speech)
- Clear visual and audio feedback
- Customizable wake words and commands
- Privacy-preserving local-first architecture
- Comprehensive command library
- Safe command execution with confirmations

## Goal State

Deliver a fully functional, privacy-focused voice automation tool that:
- Recognizes custom voice commands using on-device Speech framework
- Supports wake word activation ("Hey Computer" by default)
- Executes app launches, AppleScripts, Shortcuts, and shell commands
- Provides visual feedback via menu bar icon during listening
- Confirms destructive commands before execution
- Stores custom commands locally using SwiftData
- Supports push-to-talk mode with keyboard shortcut
- Learns custom vocabulary for technical terms
- Maintains >85% test coverage
- Uses <5% CPU during continuous listening

## Acceptance Criteria

### Core Functionality
1. **GIVEN** the user says wake word "Hey Computer"
   **WHEN** microphone is active
   **THEN** app enters listening mode with visual indicator within 200ms

2. **GIVEN** user says "Open Safari" while listening
   **WHEN** command is recognized
   **THEN** Safari launches within 500ms

3. **GIVEN** user creates custom command "Deploy website" → run shell script
   **WHEN** user says "Deploy website"
   **THEN** confirmation prompt appears before executing script

4. **GIVEN** user says "What time is it?"
   **WHEN** command matches built-in query
   **THEN** spoken response with current time via text-to-speech

5. **GIVEN** user enables continuous listening
   **WHEN** wake word is detected
   **THEN** menu bar icon changes to listening state

### Command Management
6. **GIVEN** user creates command "Screenshot" → execute keyboard shortcut ⌘+Shift+4
   **WHEN** command is saved
   **THEN** command appears in command library and activates on voice trigger

7. **GIVEN** user adds technical term "SwiftUI" to vocabulary
   **WHEN** saying "Open SwiftUI documentation"
   **THEN** recognition correctly identifies "SwiftUI" (not "swift you why")

### Performance
8. **GIVEN** continuous listening is enabled
   **WHEN** running for 1 hour
   **THEN** CPU usage remains <5% on average

9. **GIVEN** user speaks a command
   **WHEN** speech ends
   **THEN** recognition completes and processes within 500ms

### Safety
10. **GIVEN** user says command to delete files
    **WHEN** command is recognized
    **THEN** confirmation dialog appears before execution with cancel option

## Technical Notes

### Required Frameworks
- **Speech**: SFSpeechRecognizer for on-device speech recognition
- **AppKit**: NSStatusBar for menu bar, NSWindow for UI
- **Foundation**: Process for shell command execution
- **SwiftData**: Command and vocabulary persistence
- **AVFoundation**: Audio input and text-to-speech output
- **SwiftUI**: Modern UI for command editor and preferences

### Key Implementation Details
- Use `SFSpeechRecognizer` with on-device recognition (requiresOnDeviceRecognition = true)
- Implement `SFSpeechAudioBufferRecognitionRequest` for streaming audio
- Use `AVAudioEngine` to capture microphone input
- Detect wake word using custom keyword spotting or continuous recognition
- Parse recognized text with regex or natural language processing
- Execute AppleScripts using `NSAppleScript`
- Launch apps via `NSWorkspace.shared.launchApplication`
- Run shell commands using `Process` with safety checks
- Integrate Shortcuts via `NSUserActivity` or URL schemes
- Provide TTS feedback using `AVSpeechSynthesizer`

### Data Models
```
VoiceCommand: id, trigger, action, actionType (app/script/shortcut/shell), requiresConfirmation, isEnabled, lastUsed
CustomVocabulary: word, pronunciation, category
UserPreferences: wakeWord, continuousListening, pushToTalkShortcut, confirmDestructive, microphoneDevice, voiceFeedback
CommandHistory: timestamp, trigger, action, wasSuccessful, executionTime
```

### Supported Action Types
- **App Launch**: Open application by name or bundle ID
- **AppleScript**: Execute custom AppleScript code
- **Shortcuts**: Trigger macOS Shortcuts via name
- **Shell Command**: Execute bash/zsh commands with safety restrictions
- **Keyboard Shortcut**: Simulate key presses
- **URL**: Open URL in default browser
- **Text**: Type text at cursor position
- **System**: Volume, brightness, sleep, lock, logout

### Wake Word Detection
1. Start continuous speech recognition in background
2. Buffer audio in small chunks (1-2 seconds)
3. Process each chunk for wake word match
4. When wake word detected, enter active listening mode
5. Capture next 5 seconds for command recognition
6. Parse command and execute action
7. Return to wake word detection mode

### Command Parsing Examples
```
"Open Safari" → Launch app: Safari
"Run deployment script" → Execute AppleScript: deployment.scpt
"What's the weather?" → Execute Shortcut: Get Weather
"Make text bigger" → Keyboard shortcut: ⌘+
"Show me my files" → Open Finder at ~/Documents
"Set volume to fifty percent" → System action: Volume 50%
```

### Safety Restrictions for Shell Commands
- Whitelist safe commands (ls, cd, echo, date, etc.)
- Blacklist destructive commands (rm -rf, dd, sudo, etc.)
- Require explicit confirmation for file modifications
- Run commands in sandboxed environment when possible
- Log all command executions
- Provide undo capability where applicable

### Testing Strategy
- Unit tests for command parsing logic
- Unit tests for wake word detection
- Integration tests for SwiftData persistence
- Performance tests for recognition latency
- UI tests for command editor interface
- Mock SFSpeechRecognizer for consistent testing
- Accessibility tests with VoiceOver
- Audio simulation tests for recognition accuracy

### Privacy Considerations
- All speech recognition happens on-device
- No audio sent to cloud services
- Microphone access clearly explained
- Visual indicator when microphone is active
- User can disable listening anytime
- Command history can be cleared
- No telemetry or analytics
- Audio buffers cleared after processing

### Menu Bar States
- **Idle**: Gray microphone icon
- **Listening for Wake Word**: Gray with subtle animation
- **Active Listening**: Blue microphone with pulsing
- **Processing Command**: Orange with spinner
- **Executing**: Green checkmark
- **Error**: Red X with error tooltip

### Visual Feedback
- **Menu Bar Icon**: State indication with color and animation
- **Popup Window**: Transcribed text appears as user speaks
- **Confirmation Dialog**: For commands requiring approval
- **Success Notification**: "Command executed successfully"
- **Error Alert**: Clear error message with troubleshooting

### Default Commands Library
- "Open [App Name]" → Launch application
- "Quit [App Name]" → Close application
- "What time is it?" → Speak current time
- "Mute/Unmute" → Toggle audio output
- "Take a screenshot" → ⌘+Shift+3
- "Lock screen" → Lock Mac
- "Empty trash" → Empty Trash with confirmation
- "Show desktop" → F11 or Mission Control
- "Sleep" → Put Mac to sleep

### Keyboard Shortcuts
- `⌘ + Shift + L`: Toggle listening mode
- `⌘ + Shift + Space`: Push-to-talk (hold to listen)
- `⌘ + Shift + M`: Mute/unmute microphone
- `⌘ + ,`: Open preferences
- `⌘ + N`: Create new command

### Advanced Features (Optional)
- Multi-language support (English, Spanish, French, etc.)
- Context-aware commands (different commands per app)
- Variable support ("Open file [filename]")
- Conditional logic ("If Mail is open, then...")
- Chained commands ("Open Safari and navigate to apple.com")
- Voice-activated Automator workflows
- Integration with HomeKit (control smart home)
- Calendar and reminder creation via voice
- Email dictation and sending
- Mathematical calculations via spoken queries
- Web search integration
- Spotify/Apple Music voice control
- Custom wake word training
- Voice profiles for multiple users
