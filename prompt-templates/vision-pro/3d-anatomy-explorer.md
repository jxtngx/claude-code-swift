# 3D Anatomy Explorer - Vision Pro User Story

## User Story (INVEST)

**As a** medical student or educator
**I want** to explore 3D anatomical models in AR
**So that** I can learn anatomy interactively

## Facts
- Platform: visionOS 1.0+, Frameworks: RealityKit, SwiftUI, ARKit
- Models: Bundled anatomical USDZ models, No cloud services

## Constraints
- TDD, accurate medical models, layer visibility, annotations, quiz mode, accessibility

## Goal State
Anatomy explorer with layered systems (skeletal, muscular, organs), labels, cross-sections, rotation, zoom

## Acceptance Criteria
1. Toggle visibility of anatomical layers
2. Tap body parts for info labels
3. Cross-section view with slice plane
4. Quiz mode hides labels
5. Rotate and scale with gestures

## Technical Notes
- RealityKit for 3D models
- Entity layer management
- Hand tracking for slicing
- Local educational content database
