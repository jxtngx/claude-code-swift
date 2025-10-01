# Voice Note Recorder - iPhone User Story

## User Story (INVEST)

**As a** busy professional
**I want** a voice note recorder that transcribes my recordings locally
**So that** I can quickly capture ideas and search through them without my voice data leaving my device

### INVEST Checklist
- **Independent**: Self-contained audio recording and transcription app
- **Negotiable**: Features adjustable (categories, tags, export formats)
- **Valuable**: Provides privacy-focused voice recording with searchable transcriptions
- **Estimable**: Well-defined scope using standard iOS frameworks
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear criteria for recording, transcription, and playback

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Xcode**: 26+
- **Primary Frameworks**: AVFoundation, Speech, SwiftUI
- **Secondary Frameworks**: Core Data, FileManager
- **Hardware**: iPhone microphone
- **Data Storage**: Audio files in app sandbox, metadata in Core Data
- **Transcription**: On-device using Speech framework
- **No Cloud Services**: All recording and transcription happens locally

## Constraints

- Follow TDD principles with XCTest
- All audio files and transcriptions stored locally
- Use on-device speech recognition (no server-based)
- Request microphone and speech recognition permissions properly
- Handle audio session interruptions (calls, other apps)
- Implement proper file size management
- Support accessibility (VoiceOver for playback controls)
- Use Swift Concurrency for async operations
- Optimize battery usage during recording
- Handle background/foreground transitions

## Penalties

- Using cloud-based transcription services
- Requesting unnecessary permissions
- Poor audio quality from improper session configuration
- Not handling audio interruptions gracefully
- Memory leaks from audio buffers
- Missing error handling for transcription failures
- Hardcoded file paths
- Poor user feedback during recording/transcription
- Not implementing file size limits
- Ignoring storage space constraints

## Rewards

- High test coverage (>85%)
- Crystal-clear audio quality
- Fast, accurate on-device transcription
- Intuitive recording interface
- Efficient file storage and management
- Smooth playback with scrubbing
- Searchable transcription text
- Privacy-preserving local-only architecture
- Accessible playback controls

## Goal State

Deliver a fully functional voice note recorder that:
- Records high-quality audio with one-tap interface
- Transcribes recordings on-device using Speech framework
- Stores audio files locally in app sandbox
- Provides playback with seek controls and playback speed
- Enables text search across all transcriptions
- Organizes notes with titles, tags, and timestamps
- Manages storage with file size warnings
- Maintains >85% test coverage
- Handles audio interruptions gracefully

## Acceptance Criteria

### Recording
1. **GIVEN** the app is launched
   **WHEN** user taps the record button
   **THEN** audio recording starts with visual feedback (waveform/timer)

2. **GIVEN** recording is in progress
   **WHEN** user receives a phone call
   **THEN** recording pauses automatically and resumes after call ends

3. **GIVEN** user is recording
   **WHEN** they tap stop
   **THEN** audio is saved with timestamp and transcription begins

4. **GIVEN** microphone permission is denied
   **WHEN** user tries to record
   **THEN** helpful alert shows with instructions to enable in Settings

### Transcription
5. **GIVEN** a recording has finished
   **WHEN** transcription completes
   **THEN** text appears below the recording with word-level timestamps

6. **GIVEN** speech recognition permission is denied
   **WHEN** recording finishes
   **THEN** audio is saved but transcription shows "unavailable" message

7. **GIVEN** transcription fails due to unclear audio
   **WHEN** error occurs
   **THEN** user sees retry option and can edit transcription manually

### Playback
8. **GIVEN** user selects a saved recording
   **WHEN** they tap play
   **THEN** audio plays with progress indicator and current time display

9. **GIVEN** audio is playing
   **WHEN** user drags the progress slider
   **THEN** playback seeks to selected timestamp

10. **GIVEN** user adjusts playback speed
    **WHEN** they select 0.5x, 1x, 1.5x, or 2x
    **THEN** audio plays at selected rate without pitch distortion

### Search & Organization
11. **GIVEN** user has multiple recordings
    **WHEN** they search for a keyword
    **THEN** all recordings with matching transcription text are displayed

12. **GIVEN** user wants to organize recordings
    **WHEN** they add title and tags
    **THEN** metadata is saved and searchable

### Storage Management
13. **GIVEN** available storage is low (<100MB)
    **WHEN** user tries to record
    **THEN** warning appears with option to delete old recordings

14. **GIVEN** user wants to export a recording
    **WHEN** they select share option
    **THEN** audio file exports with transcription text

## Technical Notes

### Required Frameworks
- **AVFoundation**: Audio recording, playback, session management
- **Speech**: On-device speech recognition (SFSpeechRecognizer)
- **SwiftUI**: User interface with waveform visualization
- **Core Data**: Metadata storage (title, tags, transcription, timestamps)
- **FileManager**: Audio file management in app sandbox
- **CoreHaptics**: Haptic feedback for recording start/stop

### Key Implementation Details
- Use `AVAudioRecorder` with `.m4a` format for compression
- Configure `AVAudioSession` with `.record` category
- Implement `SFSpeechRecognizer` with on-device recognition
- Store audio files in `FileManager.default.urls(for: .documentDirectory)`
- Use `AVAudioPlayer` with time observation for playback
- Implement waveform visualization with audio metering
- Handle interruptions with `AVAudioSessionInterruptionNotification`
- Use `AVAudioTimePitchAlgorithm.timeDomain` for speed without pitch change
- Implement background task for transcription completion

### Data Models
```
Recording: id, title, audioFileURL, duration, createdDate, transcription, tags
Tag: id, name, color
RecordingTag: recordingID, tagID (many-to-many relationship)
```

### Audio Configuration
- **Format**: AAC (M4A) for compression
- **Sample Rate**: 44.1 kHz
- **Channels**: Mono
- **Bit Rate**: 128 kbps
- **Audio Quality**: High

### Testing Strategy
- Unit tests for file management logic
- Unit tests for transcription text processing
- Integration tests for recording and playback
- Mock AVAudioRecorder for deterministic tests
- Mock SFSpeechRecognizer with sample transcriptions
- UI tests for record/stop/playback flow
- Performance tests for large audio files
- Storage tests with file size limits
- Accessibility tests for playback controls

### Privacy Considerations
- Request microphone permission with clear explanation
- Request speech recognition permission separately
- All audio stored locally in app sandbox
- Transcriptions never sent to server
- User can delete recordings and transcriptions
- No telemetry or analytics
- Audio files encrypted at rest by iOS
- Clear data deletion removes all files and metadata
