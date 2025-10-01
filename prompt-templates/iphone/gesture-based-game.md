# Gesture-Based Game - iPhone User Story

## User Story (INVEST)

**As a** mobile gamer
**I want** a motion-controlled game using iPhone sensors and haptics
**So that** I can enjoy an immersive gaming experience using physical movement

### INVEST Checklist
- **Independent**: Self-contained motion-based game
- **Negotiable**: Game mechanics, difficulty levels, visual themes
- **Valuable**: Engaging physical gameplay without cloud dependencies
- **Estimable**: Standard iOS gaming frameworks
- **Small**: Achievable in 2-3 week sprint
- **Testable**: Clear performance and gameplay metrics

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Primary Frameworks**: CoreMotion, SpriteKit, CoreHaptics, SwiftUI
- **Hardware**: Accelerometer, gyroscope, Taptic Engine
- **Game Type**: Tilt-based obstacle avoidance or balance game
- **No Cloud Services**: Fully offline, local high scores

## Constraints

- Follow TDD principles
- All game logic and scores stored locally
- Use CoreMotion for tilt controls
- Implement CoreHaptics for collision feedback
- Optimize for 60fps gameplay
- Support accessibility (adjustable sensitivity)
- Handle device motion permission
- Calibrate sensors for different devices
- Battery-efficient sensor polling

## Penalties

- Cloud-based leaderboards or multiplayer
- Frame rate drops below 45fps
- Excessive battery drain
- No haptic feedback for game events
- Poor motion calibration
- Missing accessibility options

## Rewards

- Smooth 60fps gameplay
- Responsive tilt controls
- Satisfying haptic feedback
- Engaging game mechanics
- Local high score tracking
- Low battery impact
- High test coverage (>80%)

## Goal State

Deliver a motion-based game that:
- Uses device tilt for player control
- Provides haptic feedback for collisions and power-ups
- Tracks high scores locally with SwiftData
- Runs at consistent 60fps
- Offers difficulty levels
- Includes sound effects and background music
- Calibrates to device orientation
- Maintains >80% test coverage

## Acceptance Criteria

1. **GIVEN** game starts **WHEN** user tilts device **THEN** player character moves smoothly
2. **GIVEN** collision occurs **WHEN** player hits obstacle **THEN** haptic feedback triggers
3. **GIVEN** game session ends **WHEN** checking scores **THEN** high score saves locally
4. **GIVEN** user pauses **WHEN** app backgrounds **THEN** game state persists

## Technical Notes

### Frameworks
- **CoreMotion**: CMMotionManager for accelerometer/gyroscope
- **SpriteKit**: Game rendering and physics
- **CoreHaptics**: Collision and event feedback
- **SwiftData**: High score persistence

### Game Mechanics Example
- Tilt-based maze navigation
- Balance ball on platforms
- Obstacle avoidance runner
- Motion-controlled puzzle game

### Testing
- Unit tests for game logic
- Performance tests for frame rate
- Mock CoreMotion data for testing
- UI tests for game flow
