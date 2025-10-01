# Video Editor - iPad User Story

## User Story (INVEST)

**As a** content creator
**I want** a video editor that processes everything on-device
**So that** I can edit videos privately without cloud rendering

### INVEST Checklist
- Independent video editing, negotiable effects/transitions, valuable for creators, estimable with AVFoundation, achievable scope, testable

## Facts
- Platform: iPad (iPadOS 17+), Swift 6+
- Frameworks: AVFoundation, VideoToolbox, SwiftUI, Photos
- Processing: On-device rendering
- Storage: Local video library
- No cloud services

## Constraints
- TDD, local rendering, Photos library access, export to 4K, thermal management

## Goal State
Video editor with timeline, trim/split/merge, transitions, filters, text overlays, audio mixing, 4K export

## Acceptance Criteria
1. Multi-track timeline editor
2. Real-time preview at 30fps
3. Apply transitions and filters
4. Export to 4K without cloud processing
5. Audio level mixing

## Technical Notes
- AVMutableComposition for editing
- AVAssetExportSession for rendering
- Core Image for filters
