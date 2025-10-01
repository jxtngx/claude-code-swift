# Photo Gallery with ML Tagging - iPhone User Story

## User Story (INVEST)

**As a** photography enthusiast
**I want** a photo gallery that automatically tags and categorizes my photos using on-device AI
**So that** I can easily search and organize my collection without uploading images to the cloud

### INVEST Checklist
- **Independent**: Self-contained photo management and AI tagging app
- **Negotiable**: Features adjustable (tag types, search filters, albums)
- **Valuable**: Provides privacy-focused photo organization with intelligent search
- **Estimable**: Clear scope using PhotoKit, Vision, and Core ML
- **Small**: Core features achievable in 3-4 week sprint
- **Testable**: Clear acceptance criteria for tagging accuracy and search

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Xcode**: 26+
- **Primary Frameworks**: PhotoKit, Vision, Core ML, SwiftUI
- **Secondary Frameworks**: SwiftData, Photos
- **Hardware**: iPhone camera and photo library
- **ML Models**: Pre-trained Core ML models (MobileNetV3, ResNet50)
- **Data Storage**: Photo metadata and tags in SwiftData
- **No Cloud Services**: All ML inference and tagging happens on-device

## Constraints

- Follow TDD principles with XCTest
- All photo analysis performed on-device
- Request photo library permissions appropriately
- Use Neural Engine for ML inference optimization
- Handle large photo libraries efficiently
- Support accessibility (VoiceOver for image descriptions)
- Implement batch processing with progress feedback
- Use Swift Concurrency for async operations
- Optimize battery and thermal management
- Respect user privacy with local-only processing

## Penalties

- Using cloud-based image recognition APIs
- Requesting full photo library access when limited access suffices
- Poor ML inference performance causing UI lag
- Not handling photo library permission denials
- Memory leaks from image processing
- Missing error handling for ML model failures
- Hardcoded model paths or thresholds
- Poor user feedback during batch tagging
- Ignoring device thermal state during processing
- Not caching ML results

## Rewards

- High test coverage (>85%)
- Fast, accurate on-device image tagging
- Efficient photo library integration
- Smooth scrolling in photo grid
- Intelligent search with multiple filters
- Privacy-preserving local-only architecture
- Beautiful, intuitive SwiftUI interface
- Real-time tagging as photos are added
- Low battery and thermal impact

## Goal State

Deliver a fully functional photo gallery app that:
- Accesses user's photo library with PhotoKit
- Automatically tags photos using Vision and Core ML
- Detects objects, scenes, and colors in images
- Enables text-based search across all tags
- Organizes photos into smart albums by tags
- Processes images in background with progress indicator
- Stores tags locally using SwiftData
- Provides fast, responsive photo grid interface
- Maintains >85% test coverage
- Optimizes for Neural Engine performance

## Acceptance Criteria

### Photo Library Access
1. **GIVEN** the app is launched for the first time
   **WHEN** user grants photo library access
   **THEN** app displays photo grid with all accessible photos

2. **GIVEN** user has limited photo library access
   **WHEN** viewing the gallery
   **THEN** only selected photos are displayed and taggable

3. **GIVEN** photo library permission is denied
   **WHEN** user opens the app
   **THEN** helpful message appears with Settings navigation option

### ML Tagging
4. **GIVEN** a photo is selected for tagging
   **WHEN** ML model processes the image
   **THEN** tags for objects, scenes, and dominant colors are generated

5. **GIVEN** photo contains a dog
   **WHEN** Vision framework analyzes the image
   **THEN** tags include "animal", "dog", and specific breed if detectable

6. **GIVEN** photo is a sunset landscape
   **WHEN** scene classification runs
   **THEN** tags include "sunset", "landscape", "outdoor", color tags

7. **GIVEN** user has 500 untagged photos
   **WHEN** batch tagging is initiated
   **THEN** progress indicator shows completion percentage

### Search & Filtering
8. **GIVEN** user searches for "beach"
   **WHEN** query is submitted
   **THEN** all photos tagged with "beach" or related terms appear

9. **GIVEN** user selects multiple tag filters
   **WHEN** filters are applied (e.g., "dog" AND "outdoor")
   **THEN** only photos matching all tags are displayed

10. **GIVEN** user searches by color
    **WHEN** they select "blue"
    **THEN** photos with dominant blue tones are shown

### Performance
11. **GIVEN** photo library has 10,000 photos
    **WHEN** user scrolls the grid
    **THEN** scrolling remains smooth at 60fps with lazy loading

12. **GIVEN** ML tagging is processing 100 photos
    **WHEN** device reaches 80% thermal state
    **THEN** processing pauses until thermal state improves

13. **GIVEN** app is in background
    **WHEN** batch tagging is in progress
    **THEN** processing continues using BGTaskScheduler

### Smart Albums
14. **GIVEN** photos are tagged
    **WHEN** user views smart albums
    **THEN** albums auto-generate for "Animals", "Nature", "People", etc.

## Technical Notes

### Required Frameworks
- **PhotoKit**: Photo library access, PHAsset management
- **Vision**: Image analysis, object detection, scene classification
- **Core ML**: Pre-trained model inference on Neural Engine
- **SwiftUI**: Photo grid with LazyVGrid, search interface
- **SwiftData**: Tag storage and photo metadata
- **BackgroundTasks**: BGTaskScheduler for batch processing

### Key Implementation Details
- Use `PHPhotoLibrary` for photo library authorization
- Implement `VNCoreMLRequest` with MobileNetV3 or ResNet50
- Use `VNClassifyImageRequest` for scene classification
- Implement color analysis with `VNImageRequestHandler`
- Store tags in SwiftData with many-to-many relationships
- Use `PHCachingImageManager` for efficient thumbnail loading
- Implement lazy loading with `LazyVGrid` in SwiftUI
- Monitor thermal state with `ProcessInfo.thermalStateNotification`
- Use `DispatchQueue.global(qos: .utility)` for ML inference
- Cache ML results to avoid reprocessing

### Data Models
```
Photo: id, phAssetIdentifier, dateTaken, location, thumbnailData
Tag: id, name, category, confidence
PhotoTag: photoID, tagID, confidence (many-to-many)
SmartAlbum: id, name, tagFilter, autoUpdate
```

### ML Models
- **Scene Classification**: MobileNetV3 (scenes and activities)
- **Object Detection**: YOLOv5 or EfficientDet (objects and animals)
- **Color Analysis**: Custom Core ML model or Vision color histogram
- **Confidence Threshold**: >0.7 for tag inclusion

### Testing Strategy
- Unit tests for tag generation logic
- Unit tests for search query parsing
- Mock Vision requests with sample predictions
- Integration tests with sample photo library
- Performance tests for large photo libraries
- UI tests for search and filter flows
- Thermal state tests for processing throttling
- Accessibility tests for VoiceOver support
- Test ML model loading and inference

### Privacy Considerations
- Request limited photo library access by default
- All ML inference happens on-device
- Tags stored locally in SwiftData
- No photo data sent to external servers
- User can delete tags without affecting photos
- Clear explanation in permission prompts
- Respect photo library privacy settings
- No telemetry or analytics collection

### Performance Optimization
- Use Neural Engine for ML inference
- Batch process images during charging
- Cache thumbnails for grid display
- Lazy load images as user scrolls
- Throttle processing based on thermal state
- Preload ML models on app launch
- Use background tasks for large libraries
- Implement intelligent tag deduplication
