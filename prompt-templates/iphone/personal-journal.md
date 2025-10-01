# Personal Journal - iPhone User Story

## User Story (INVEST)

**As a** journaler seeking self-reflection
**I want** a private diary app with AI-powered sentiment analysis
**So that** I can track my emotional patterns over time without my entries leaving my device

### INVEST Checklist
- **Independent**: Self-contained journaling with on-device NLP
- **Negotiable**: Features like tags, photos, sentiment tracking
- **Valuable**: Privacy-focused journaling with emotional insights
- **Estimable**: Well-defined NLP and data storage requirements
- **Small**: Core features in 2-3 week sprint
- **Testable**: Clear criteria for entry creation and sentiment analysis

## Facts

- **Platform**: iPhone (iOS 17+)
- **Swift Version**: 6+
- **Primary Frameworks**: Natural Language, SwiftUI, SwiftData, CryptoKit
- **NLP**: On-device sentiment analysis using NLTagger
- **Security**: Encrypted entries with biometric authentication
- **No Cloud Services**: All entries stored locally with encryption

## Constraints

- Follow TDD principles
- Encrypt all entries using CryptoKit
- On-device sentiment analysis only
- Require biometric authentication
- Support rich text formatting
- Attach photos to entries
- Implement full-text search
- Support daily prompts
- Export to encrypted PDF
- Respect user privacy completely

## Penalties

- Cloud sync or backups
- Unencrypted data storage
- Server-based sentiment analysis
- Missing biometric lock
- Poor search performance
- No data export
- Ignoring encryption best practices

## Rewards

- Military-grade encryption
- Fast on-device sentiment analysis
- Beautiful, calming interface
- Comprehensive emotional insights
- Full-text search across entries
- Privacy-first architecture
- High test coverage (>85%)

## Goal State

Deliver a secure personal journal that:
- Creates daily entries with rich text and photos
- Analyzes sentiment using Natural Language framework
- Encrypts all entries with AES-256 via CryptoKit
- Requires Face ID/Touch ID to access
- Tracks emotional patterns over time
- Provides daily writing prompts
- Enables full-text search
- Exports to encrypted PDF
- Maintains >85% test coverage

## Acceptance Criteria

1. **GIVEN** app launches **WHEN** biometric auth required **THEN** Face ID/Touch ID prompt appears
2. **GIVEN** entry created **WHEN** saved **THEN** sentiment score (positive/neutral/negative) calculated
3. **GIVEN** viewing history **WHEN** browsing **THEN** sentiment chart shows emotional trends
4. **GIVEN** searching **WHEN** query entered **THEN** matching entries returned within 1 second
5. **GIVEN** export requested **WHEN** selecting date range **THEN** encrypted PDF generated

## Technical Notes

### Frameworks
- **Natural Language**: NLTagger for sentiment analysis
- **SwiftUI**: Rich text editor and entry list
- **SwiftData**: Encrypted entry storage
- **CryptoKit**: AES-256-GCM encryption
- **LocalAuthentication**: Face ID/Touch ID
- **PhotosUI**: Photo picker for attachments

### Data Model
```
JournalEntry: id, date, content (encrypted), sentiment, photos (encrypted), tags
Tag: id, name, color
Prompt: id, text, dateUsed
```

### Security
- Encrypt content with user-derived key
- Store encryption key in Keychain
- Require biometric auth on launch
- Auto-lock after inactivity
- Secure delete with overwrite

### Testing
- Unit tests for encryption/decryption
- NLP sentiment accuracy tests
- Biometric mock tests
- Search performance tests
- PDF export validation
