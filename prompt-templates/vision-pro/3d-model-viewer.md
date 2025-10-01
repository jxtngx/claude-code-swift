# 3D Model Viewer - Vision Pro User Story

## User Story (INVEST)

**As a** 3D artist or educator
**I want** to view and manipulate 3D models in spatial environment
**So that** I can inspect models at scale with natural hand gestures

### INVEST Checklist
- Independent spatial viewer, negotiable model formats, valuable for visualization, estimable with RealityKit, achievable scope, testable

## Facts
- Platform: visionOS 1.0+, Swift 6+
- Frameworks: RealityKit, SwiftUI, ARKit
- Input: Hand tracking, eye tracking, gestures
- Models: USDZ, OBJ, FBX (bundled/imported)
- Storage: Local file system
- No cloud services

## Constraints
- TDD, spatial gestures, hand tracking, model LOD optimization, volumetric bounds, accessibility

## Goal State
3D viewer with gesture controls (pinch-scale, rotate, move), model library, annotation tools, measurement, export to USDZ

## Acceptance Criteria
1. Load USDZ models from Files app
2. Pinch to scale models naturally
3. Rotate models with two-hand gestures
4. Place models in real environment
5. Measure dimensions with hand tracking
6. Annotate models with spatial notes

## Technical Notes
- RealityKit for 3D rendering
- ARKit hand tracking
- Spatial gesture recognizers
- USDZ model loading and optimization
- LOD for performance
