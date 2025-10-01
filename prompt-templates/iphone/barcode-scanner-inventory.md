# Barcode Scanner Inventory - iPhone User Story

## User Story (INVEST)

**As a** small business owner or home organizer
**I want** a barcode scanner app that tracks inventory locally
**So that** I can manage stock without relying on cloud services or subscriptions

### INVEST Checklist
- **Independent**: Self-contained inventory management with barcode scanning
- **Negotiable**: Features adjustable (categories, alerts, custom fields)
- **Valuable**: Provides offline inventory tracking and management
- **Estimable**: Clear scope using Vision and Core Data
- **Small**: Core features achievable in 2-3 week sprint
- **Testable**: Clear criteria for scanning accuracy and data management

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Xcode**: 26+
- **Primary Frameworks**: Vision, AVFoundation, SwiftUI, Core Data
- **Hardware**: iPhone camera
- **Barcode Types**: EAN, UPC, Code128, QR codes
- **No Cloud Services**: All data stored locally on device

## Constraints

- Follow TDD principles with XCTest
- All inventory data stored in Core Data
- Request camera permission appropriately
- Real-time barcode detection using Vision
- Support multiple barcode formats
- Handle low-light scanning conditions
- Implement CSV export for backups
- Support accessibility (VoiceOver labels)
- Optimize camera battery usage

## Penalties

- Using cloud-based product databases
- Poor camera session management
- Not handling barcode detection failures
- Missing duplicate detection
- Hardcoded category lists
- No data export capability
- Poor low-light performance
- Ignoring battery optimization

## Rewards

- Fast, accurate barcode scanning
- Comprehensive inventory tracking
- Efficient data entry workflow
- Low stock alerts
- CSV export for backups
- Privacy-focused local storage
- Intuitive SwiftUI interface
- High test coverage (>85%)

## Goal State

Deliver a barcode scanner inventory app that:
- Scans barcodes in real-time using Vision
- Creates inventory items with name, quantity, price, category
- Tracks stock levels and sends low stock alerts
- Supports manual barcode entry for damaged codes
- Provides search and filtering by category
- Exports inventory to CSV
- Stores all data locally in Core Data
- Maintains >85% test coverage

## Acceptance Criteria

1. **GIVEN** camera permission granted **WHEN** scanning a barcode **THEN** item detected within 2 seconds
2. **GIVEN** barcode already exists **WHEN** scanned again **THEN** quantity increment option shown
3. **GIVEN** item quantity below threshold **WHEN** viewing inventory **THEN** low stock indicator displayed
4. **GIVEN** user wants backup **WHEN** selecting export **THEN** CSV file generated and shareable
5. **GIVEN** barcode unreadable **WHEN** scan fails **THEN** manual entry option available

## Technical Notes

### Frameworks
- **Vision**: VNDetectBarcodesRequest for real-time detection
- **AVFoundation**: AVCaptureSession for camera feed
- **SwiftUI**: Scanner interface and inventory list
- **Core Data**: Local inventory storage

### Data Model
```
InventoryItem: id, name, barcode, quantity, price, category, minQuantity, dateAdded
Category: id, name, color
```

### Testing
- Unit tests for barcode validation
- Mock Vision detection for tests
- UI tests for scan-to-add flow
- Core Data integration tests
- CSV export validation tests
