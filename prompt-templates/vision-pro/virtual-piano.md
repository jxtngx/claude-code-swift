# Virtual Piano - Vision Pro User Story

## User Story (INVEST)

**As a** musician or learner
**I want** to play a virtual piano with hand tracking
**So that** I can practice anywhere without physical instrument

## Facts
- Platform: visionOS 1.0+, Frameworks: RealityKit, AVFoundation, ARKit
- Audio: Synthesized piano samples, No cloud services

## Constraints
- TDD, precise hand tracking, low audio latency, visual feedback, recording, accessibility

## Goal State
Virtual piano with 88 keys, hand tracking for playing, visual feedback, recording, playback, MIDI export

## Acceptance Criteria
1. Hand tracking detects key presses
2. Audio latency <20ms
3. Visual key press feedback
4. Record and playback performances
5. Export to MIDI file

## Technical Notes
- ARKit hand tracking
- AVAudioEngine for synthesis
- RealityKit for 3D keys
- Haptic feedback on key press
