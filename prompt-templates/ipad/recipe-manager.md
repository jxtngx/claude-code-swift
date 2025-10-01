# Recipe Manager - iPad User Story

## User Story (INVEST)

**As a** home cook
**I want** a recipe manager that uses the camera to detect ingredients
**So that** I can organize recipes and get suggestions based on what I have on hand

### INVEST Checklist
- Independent, negotiable features, valuable for cooking, estimable with Vision/Core ML, small scope, testable

## Facts
- Platform: iPad (iPadOS 17+), Swift 6+, Xcode 26+
- Frameworks: Vision, Core ML, SwiftUI, SwiftData, AVFoundation
- ML: Ingredient recognition from photos
- Storage: Local recipes and photos
- No cloud services

## Constraints
- TDD, on-device ML, local storage, camera permission, accessibility

## Goal State
Recipe manager that detects ingredients from photos, stores recipes locally, suggests meals based on available ingredients, includes timers

## Acceptance Criteria
1. Camera detects ingredients with >80% accuracy
2. Recipes searchable by ingredient
3. Meal suggestions based on available items
4. Cooking timers with notifications
5. Export recipes to PDF

## Technical Notes
- Vision: VNCoreMLRequest for ingredient detection
- SwiftData: Recipe and ingredient storage
- Custom Core ML model for food classification
