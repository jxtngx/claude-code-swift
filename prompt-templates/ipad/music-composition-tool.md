# Music Composition Tool - iPad User Story

## User Story (INVEST)

**As a** musician and composer
**I want** a MIDI sequencer with virtual instruments
**So that** I can create music entirely on iPad without cloud services

### INVEST Checklist
- Independent music creation app, negotiable instruments/features, valuable for musicians, estimable scope, small MVP, testable

## Facts
- Platform: iPad (iPadOS 17+), Swift 6+
- Frameworks: AVFoundation, AudioKit, CoreMIDI, SwiftUI
- Audio: Built-in synthesizers and samplers
- Storage: Local project files, audio export
- No cloud services

## Constraints
- TDD, low-latency audio (<10ms), local file storage, Core MIDI support, export to WAV/MIDI

## Goal State
MIDI sequencer with piano roll editor, virtual instruments, mixing, audio export, metronome, recording

## Acceptance Criteria
1. Piano roll editing with quantization
2. Multiple instrument tracks
3. Real-time playback <10ms latency
4. Export to WAV and MIDI files
5. Tempo and time signature control

## Technical Notes
- AVAudioEngine for synthesis
- AVAudioSequencer for MIDI playback
- Files app integration for projects
