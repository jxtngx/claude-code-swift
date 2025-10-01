# Spatial Meditation Environment - Vision Pro User Story

## User Story (INVEST)

**As a** meditation practitioner
**I want** immersive spatial environments for meditation
**So that** I can practice in calming 3D spaces without distractions

## Facts
- Platform: visionOS 1.0+, Frameworks: RealityKit, Spatial Audio, SwiftUI
- Environments: Forest, beach, mountain, space (bundled 3D scenes)
- Audio: Spatial ambient sounds, No cloud services

## Constraints
- TDD, immersive spaces, spatial audio, hand gestures, timer integration, low battery mode

## Goal State
Meditation app with immersive 3D environments, spatial audio, breathing animations, session timer, progress tracking

## Acceptance Criteria
1. Immersive environment surrounds user
2. Spatial audio from directional sources
3. Floating breathing guide sphere
4. Hand gesture to start/pause timer
5. Session saves locally

## Technical Notes
- ImmersiveSpace for full environment
- Spatial audio with AudioSource placement
- RealityKit particle effects
- SwiftData for session history
