# Claude Code Swift Templates

AI-assisted Swift and SwiftUI development toolkit for building privacy-focused, on-device applications across Apple platforms.

## Overview

This repository provides a multi-agent AI system and comprehensive user story templates for building high-quality Swift applications using Claude Code. The project emphasizes:

- **Privacy-First Architecture**: On-device processing with no cloud dependencies
- **Test-Driven Development**: >85% test coverage goal
- **Multi-Agent Collaboration**: 14 specialized AI agents for different Apple frameworks
- **Ready-to-Use Templates**: 35 platform-specific user stories following INVEST + FCPRG principles
- **Apple Platform Expertise**: iPhone, iPad, Apple Watch, Vision Pro, and macOS

## Prerequisites

### Required

- **Apple Developer Account** - Required for deploying apps to physical devices
  - Sign up at [developer.apple.com/programs](https://developer.apple.com/programs)
  - **Free Account**: Test on simulator only
  - **Paid Account** ($99/year): Deploy to physical devices, access to all frameworks, App Store distribution
- **Mac Computer** - Running macOS 14.0 or later
- **Xcode 16+** - Download from Mac App Store
- **Claude Code** - Terminal or VS Code with Claude Code extension
  - **Important**: Xcode does not have Claude Code integration or built-in terminal support
  - Use Terminal with Claude Code CLI OR VS Code with Claude Code extension
  - Keep Claude Code session open alongside Xcode for development

### Recommended

- Swift 6+ knowledge
- SwiftUI experience
- Familiarity with TDD principles
- Understanding of Apple Human Interface Guidelines

## Quick Start

### 1. Clone or Fork Repository

```bash
git clone https://github.com/jxtngx/claude-code-swift.git
cd claude-code-swift
```

### 2. Open in Claude Code

**Important**: Since Xcode does not have Claude Code integration, you'll work with two applications:

**Option A: Terminal + Xcode**
```bash
# In Terminal, navigate to your project
cd /path/to/your/xcode-project

# Start Claude Code CLI session
claude-code
```

**Option B: VS Code + Xcode**
1. Open the Xcode project folder in VS Code (with Claude Code extension)
2. Use Claude Code in VS Code for AI assistance
3. Open the `.xcodeproj` file in Xcode for building and running

**Workflow**: Use Claude Code (Terminal or VS Code) to generate Swift code, then copy to Xcode project files or use file navigation to edit directly.

### 3. Create Xcode Project

Create a new Xcode project for your target platform:
- **iOS App**: File → New → Project → iOS → App
- **watchOS App**: File → New → Project → watchOS → App
- **visionOS App**: File → New → Project → visionOS → App
- **macOS App**: File → New → Project → macOS → App

Choose SwiftUI as the interface and Swift as the language.

### 4. Choose a User Story Template

Browse `prompt-templates/` and select a template that matches your platform and app idea:

```
prompt-templates/
├── iphone/          # 7 iPhone app templates
├── ipad/            # 7 iPad app templates
├── apple-watch/     # 7 Apple Watch app templates
├── vision-pro/      # 7 Vision Pro app templates
└── macos/           # 7 macOS app templates
```

### 5. Submit Template to Claude Code

Copy the entire contents of your chosen template and paste it into Claude Code. The AI agents will:
1. Analyze the user story
2. Plan the implementation
3. Write code following TDD principles
4. Create comprehensive tests
5. Ensure accessibility and performance optimization

### 6. Build and Test

Follow the AI-generated implementation:
- Run tests with `⌘ + U`
- Build the project with `⌘ + B`
- Run on simulator or device with `⌘ + R`

## Project Structure

```
claude-code-swift/
├── .claude/
│   └── agents/              # 14 specialized AI agents
│       ├── supervisor.md    # Task router and coordinator
│       ├── frontend.md      # SwiftUI interface specialist
│       ├── backend.md       # Networking and persistence
│       ├── cloud.md         # AWS integration (when needed)
│       ├── sensors.md       # Motion, location, biometric sensors
│       ├── healthkit.md     # Health and fitness data
│       ├── homekit.md       # Smart home device control
│       ├── weatherkit.md    # Weather data and forecasting
│       ├── coreml.md        # Machine learning models
│       ├── vision.md        # Computer vision
│       ├── speech.md        # Speech recognition and synthesis
│       ├── avfoundation.md  # Audio and video
│       ├── haptics.md       # Haptic feedback
│       └── foundationmodels.md # Apple Intelligence and NLP
├── prompt-templates/        # Platform-specific user stories
│   ├── iphone/
│   ├── ipad/
│   ├── apple-watch/
│   ├── vision-pro/
│   └── macos/
├── CLAUDE.md                # Root agent instructions
└── README.md                # This file
```

## Available AI Agents

The project includes 14 specialized agents, each expert in specific Apple frameworks:

| Agent | Expertise |
|-------|-----------|
| **Supervisor** | Task routing and multi-agent coordination |
| **Frontend** | SwiftUI views, navigation, accessibility |
| **Backend** | URLSession, Core Data, SwiftData, background tasks |
| **Cloud** | AWS services (Lambda, S3, DynamoDB) when explicitly required |
| **Sensors** | CoreMotion, CoreLocation, SensorKit, biometric sensors |
| **HealthKit** | Health data management, workouts, clinical records |
| **HomeKit** | Smart home accessories, automation, HomeKit Secure Video, Matter |
| **WeatherKit** | Weather forecasts, alerts, current conditions, precipitation data |
| **CoreML** | ML model integration, Create ML, Neural Engine optimization |
| **Vision** | Image analysis, object detection, OCR |
| **Speech** | Speech recognition and synthesis |
| **AVFoundation** | Audio/video capture, playback, editing |
| **Haptics** | CoreHaptics, UIFeedbackGenerator, Taptic Engine |
| **FoundationModels** | Apple Intelligence, Natural Language, SiriKit |

See [`.claude/agents/supervisor.md`](.claude/agents/supervisor.md) for detailed routing guidelines.

## User Story Templates

### iPhone (7 Templates)
- **Fitness Activity Tracker** - HealthKit, CoreMotion, step tracking
- **Voice Note Recorder** - AVFoundation, Speech, on-device transcription
- **Photo Gallery with ML Tagging** - Vision, Core ML, PhotoKit
- **Meditation Timer** - AVFoundation, CoreHaptics, HealthKit
- **Barcode Scanner Inventory** - Vision, Core Data
- **Gesture-Based Game** - CoreMotion, SpriteKit, haptics
- **Personal Journal** - Natural Language, CryptoKit, encryption

### iPad (7 Templates)
- **Digital Sketchpad** - PencilKit, Vision, Apple Pencil optimization
- **Document Scanner & OCR** - VisionKit, Vision, PDFKit
- **Recipe Manager** - Vision, Core ML, ingredient detection
- **Music Composition Tool** - AVFoundation, MIDI, synthesis
- **Video Editor** - AVFoundation, on-device rendering
- **Math Problem Solver** - PencilKit, Vision, symbolic math
- **Interactive Storybook** - SpriteKit, TTS, animations

### Apple Watch (7 Templates)
- **Workout Companion** - HealthKit, WorkoutKit, HR zones
- **Breathing Exercise** - CoreHaptics, Digital Crown
- **Water Intake Tracker** - HealthKit, complications
- **Walking Pace Monitor** - CoreLocation, CoreMotion
- **Heart Rate Zones** - HealthKit, zone tracking
- **Stand Reminder** - CoreMotion, smart notifications
- **Sleep Quality Tracker** - HealthKit, HRV, sleep analysis

### Vision Pro (7 Templates)
- **3D Model Viewer** - RealityKit, hand tracking, USDZ
- **Spatial Meditation Environment** - ImmersiveSpace, spatial audio
- **Virtual Workspace Organizer** - Spatial windows, layouts
- **3D Anatomy Explorer** - RealityKit, educational models
- **Spatial Photo Album** - PhotoKit, 3D layouts
- **Virtual Piano** - Hand tracking, audio synthesis
- **Mindfulness Space** - Customizable environments, HealthKit

### macOS (7 Templates)
- **Menu Bar System Monitor** - AppKit, IOKit, CPU/memory/network tracking
- **Markdown Note Editor** - AppKit, WebKit, live preview, local files
- **Screen Time Analyzer** - Accessibility APIs, app usage tracking
- **Clipboard History Manager** - NSPasteboard, search, encryption
- **Focus Mode Timer** - Pomodoro, Do Not Disturb, app blocking
- **File Organization Assistant** - FileManager, Vision OCR, smart sorting
- **Voice Command Automator** - Speech, AppleScript, hands-free control

All templates follow **INVEST** (Independent, Negotiable, Valuable, Estimable, Small, Testable) and **FCPRG** (Facts, Constraints, Penalties, Rewards, Goal State) principles.

## Development Philosophy

### Core Principles

1. **On-Device First**
   - All processing happens locally when possible
   - No cloud dependencies unless explicitly required
   - User data never leaves the device

2. **Test-Driven Development**
   - Write tests before implementation
   - Target >85% code coverage
   - Unit, integration, and UI tests

3. **Accessibility by Default**
   - VoiceOver labels and hints
   - Dynamic Type support
   - Reduced motion alternatives
   - Color contrast compliance

4. **Performance Optimization**
   - Battery-efficient implementations
   - Thermal state monitoring
   - Memory management
   - 60fps target for animations

5. **Privacy & Security**
   - Minimal permission requests
   - Encryption for sensitive data
   - Transparent data usage
   - HIPAA compliance where applicable

## Apple Developer Account

### Why You Need It

An Apple Developer Account is **required** to:
- Deploy apps to physical iPhone, iPad, Apple Watch, or Vision Pro devices
- Access certain frameworks (HealthKit, SiriKit on real devices)
- Test features that require real hardware (camera, sensors, Face ID)
- Distribute apps via TestFlight or App Store

### Account Types

**Free Account**
- Test on iOS Simulator
- Limited to 7 days per device installation
- Cannot use all entitlements
- Good for learning and development

**Paid Account** ($99/year USD)
- Deploy to unlimited devices for one year
- Access to all frameworks and entitlements
- TestFlight beta distribution
- App Store distribution
- CloudKit, Sign in with Apple, etc.

### Sign Up

1. Visit [developer.apple.com/programs](https://developer.apple.com/programs)
2. Sign in with your Apple ID
3. Choose Individual or Organization membership
4. Complete enrollment and payment (for paid account)

**Note**: You can develop and test on the Simulator without any account. The account is only needed when deploying to physical devices.

## Example Usage

Here's a typical workflow using the **Fitness Activity Tracker** template:

1. **Create Xcode Project**
   ```
   File → New → Project → iOS → App
   Product Name: FitnessTracker
   Interface: SwiftUI
   Language: Swift
   ```

2. **Open in Claude Code**
   - In Terminal: `cd ~/YourXcodeProject && claude-code`
   - OR in VS Code: Open project folder with Claude Code extension

3. **Open Template**
   - Navigate to `prompt-templates/iphone/fitness-activity-tracker.md`
   - Copy entire contents

4. **Submit to Claude Code**
   - Paste template into Claude Code chat (Terminal or VS Code)
   - AI will analyze requirements and create implementation plan

5. **Agent Collaboration**
   - **Supervisor** routes tasks to appropriate agents
   - **HealthKit Agent** implements step tracking and workout logging
   - **Frontend Agent** creates SwiftUI views
   - **Backend Agent** sets up SwiftData persistence
   - **Haptics Agent** adds haptic feedback for goals

6. **Switch to Xcode**
   - Claude Code generates Swift files in your project
   - Open Xcode to build and run
   - Files auto-sync if both apps are watching the same directory

7. **Review & Test**
   - Review generated code in Xcode
   - Run tests with `⌘ + U` in Xcode
   - Test on simulator or device
   - Return to Claude Code for refinements

8. **Deploy**
   - Configure signing with your Apple Developer Account in Xcode
   - Build for device
   - Install and test on physical hardware

## Additional Resources

### Apple Documentation
- [Swift Documentation](https://swift.org/documentation/)
- [SwiftUI Documentation](https://developer.apple.com/documentation/swiftui/)
- [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/)
- [App Distribution Guide](https://developer.apple.com/distribute/)

### Learning Resources
- [100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)
- [Swift by Sundell](https://www.swiftbysundell.com/)
- [Apple Developer Videos](https://developer.apple.com/videos/)
- [Swift Forums](https://forums.swift.org/)

### Testing & Quality
- [XCTest Documentation](https://developer.apple.com/documentation/xctest)
- [UI Testing Guide](https://developer.apple.com/documentation/xctest/user_interface_tests)
- [Performance Testing](https://developer.apple.com/documentation/xctest/performance_tests)

---

**Built with**: Claude Code
