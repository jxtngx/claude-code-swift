# Math Problem Solver - iPad User Story

## User Story (INVEST)

**As a** student
**I want** to write math equations with Apple Pencil and get step-by-step solutions
**So that** I can learn math concepts with instant on-device feedback

### INVEST Checklist
- Independent tutoring app, negotiable math topics, valuable for learning, estimable with PencilKit/Vision, small scope, testable

## Facts
- Platform: iPad (iPadOS 17+), Swift 6+
- Frameworks: PencilKit, Vision, Core ML, SwiftUI
- Recognition: Handwritten equation detection
- Solving: On-device symbolic math engine
- No cloud services

## Constraints
- TDD, on-device OCR, local computation, Apple Pencil optimization, step-by-step explanations

## Goal State
Math solver recognizing handwritten equations, showing step-by-step solutions, graphing functions, practice problems

## Acceptance Criteria
1. Recognizes handwritten equations >90% accuracy
2. Provides step-by-step solutions
3. Graphs functions in real-time
4. Supports algebra, calculus, geometry
5. Practice mode with generated problems

## Technical Notes
- Vision: VNRecognizeTextRequest for equation detection
- Custom symbolic math engine
- PencilKit for natural writing
