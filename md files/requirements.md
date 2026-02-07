# Requirements Document

## Introduction

Bharat Tax Mitra is a Progressive Web App (PWA) designed to help salaried individuals in India understand, calculate, and prepare their income tax filings safely. The system must work offline, handle slow internet connections, and serve users across urban and rural India with varying levels of technical expertise and connectivity.

## Glossary

- **PWA**: Progressive Web App - a web application that uses modern web capabilities to deliver an app-like experience
- **Form_16**: Official salary certificate issued by employers containing salary and tax deduction details
- **OCR_Engine**: Optical Character Recognition system for extracting text from images/PDFs
- **Tax_Calculator**: Deterministic rule-based engine for computing income tax
- **AI_Assistant**: Amazon Bedrock-powered conversational interface for explanations and Q&A
- **Sync_Engine**: System for synchronizing data between local IndexedDB and cloud DynamoDB
- **Filing_Generator**: Component that creates government-compliant JSON for tax filing
- **PII**: Personally Identifiable Information including PAN, salary details, and personal data

## Requirements

### Requirement 1: Progressive Web Application

**User Story:** As a user in India, I want to access the tax assistant through any device with a web browser, so that I can prepare my taxes without installing native apps.

#### Acceptance Criteria

1. THE PWA SHALL be installable on mobile and desktop devices
2. THE PWA SHALL work on devices with limited storage and processing power
3. THE PWA SHALL provide a native app-like experience with proper navigation and UI patterns
4. THE PWA SHALL support both portrait and landscape orientations on mobile devices
5. THE PWA SHALL maintain consistent functionality across Chrome, Firefox, Safari, and Edge browsers

### Requirement 2: Offline Functionality and Data Storage

**User Story:** As a user with intermittent internet connectivity, I want to use the tax assistant offline, so that I can work on my tax preparation regardless of network availability.

#### Acceptance Criteria

1. THE PWA SHALL support core tax calculation and data review features offline after initial load and data sync
2. WHEN offline, THE PWA SHALL store all user data in IndexedDB locally
3. THE PWA SHALL cache essential application resources for offline use
4. WHEN connectivity is restored, THE Sync_Engine SHALL synchronize local data with cloud storage
5. THE PWA SHALL display clear indicators when operating in offline mode
6. WHEN offline for more than 30 days, THE PWA SHALL display a "stale rules" warning for tax calculations

### Requirement 3: Form-16 Data Extraction

**User Story:** As a salaried employee, I want to upload my Form-16 and have the system extract relevant data automatically, so that I don't have to manually enter all tax information.

#### Acceptance Criteria

1. THE PWA SHALL accept Form-16 uploads in PDF and image formats (JPG, PNG)
2. WHEN a Form-16 is uploaded, THE OCR_Engine SHALL extract salary, deductions, and HRA data using Amazon Textract
3. THE PWA SHALL highlight extracted fields with low confidence scores using red-box UI indicators
4. THE PWA SHALL allow manual verification and correction of all extracted data
5. THE PWA SHALL provide manual data entry option when OCR is unavailable or offline
6. THE PWA SHALL auto-delete uploaded documents after session completion for privacy

### Requirement 4: Tax Calculation Engine

**User Story:** As a taxpayer, I want accurate tax calculations based on current Indian tax laws, so that I can understand my tax liability correctly.

#### Acceptance Criteria

1. THE Tax_Calculator SHALL use deterministic Python rule-based logic for all calculations
2. THE Tax_Calculator SHALL support current Indian tax slabs, limits, and deduction categories
3. THE Tax_Calculator SHALL load tax rules from configuration files without requiring code changes
4. THE Tax_Calculator SHALL provide offline JavaScript calculations with identical results to server-side calculations
5. THE Tax_Calculator SHALL validate all input data before performing calculations
6. WHEN tax rules are outdated, THE Tax_Calculator SHALL display appropriate warnings to users

### Requirement 5: AI-Powered Explanations and Education

**User Story:** As a user unfamiliar with tax processes, I want AI-powered explanations and guidance, so that I can understand tax concepts and make informed decisions.

#### Acceptance Criteria

1. THE AI_Assistant SHALL integrate with Amazon Bedrock for natural language explanations
2. THE AI_Assistant SHALL provide personalized responses using calculated tax summary context
3. THE AI_Assistant SHALL appear as a Copilot-style side panel during Form-16 viewing and tax summary review
4. WHEN sending data to AI services, THE PWA SHALL mask all PII including PAN numbers and salary details
5. THE AI_Assistant SHALL focus ONLY on explanations, education, and Q&A - never on tax calculations
6. THE AI_Assistant SHALL provide responses in both English and Hindi languages

### Requirement 6: Filing Automation and Government Integration

**User Story:** As a taxpayer, I want to generate filing-ready documents that comply with government requirements, so that I can submit my taxes directly to official portals.

#### Acceptance Criteria

1. THE Filing_Generator SHALL create JSON output following official Income Tax Department schema
2. THE Filing_Generator SHALL validate all generated data against government requirements
3. THE PWA SHALL generate a filing-ready JSON that users can manually upload to the official Income Tax Department portal
4. WHEN downloading filing documents, THE PWA SHALL require mandatory user verification checkbox
5. THE Filing_Generator SHALL include all required fields and calculations in the output format
6. THE PWA SHALL provide clear instructions for manual filing when direct upload is unavailable

### Requirement 7: Data Synchronization and Reliability

**User Story:** As a user switching between devices, I want my tax data to be synchronized safely across platforms, so that I can continue my work from any device.

#### Acceptance Criteria

1. THE Sync_Engine SHALL perform atomic synchronization between IndexedDB and DynamoDB
2. THE Sync_Engine SHALL use checksums to verify data integrity during sync operations
3. WHEN sync conflicts occur, THE Sync_Engine SHALL prioritize local data and notify the user
4. THE Sync_Engine SHALL handle network interruptions gracefully with retry mechanisms
5. THE PWA SHALL provide clear sync status indicators to users
6. THE Sync_Engine SHALL encrypt all data before cloud storage

### Requirement 8: Privacy and Security

**User Story:** As a user handling sensitive financial information, I want my data to be protected and private, so that my personal tax information remains secure.

#### Acceptance Criteria

1. THE PWA SHALL handle PAN numbers and salary information as sensitive data with appropriate encryption
2. THE PWA SHALL auto-delete uploaded Form-16 documents after session completion
3. WHEN sending data to external services, THE PWA SHALL mask or remove all PII
4. THE PWA SHALL store data locally by default and sync to cloud only with user consent
5. THE PWA SHALL provide clear privacy notices about data handling and storage
6. THE PWA SHALL implement secure authentication for cloud data access

### Requirement 9: Cost Control and Resource Management

**User Story:** As a system administrator, I want to control AWS costs and resource usage, so that the service remains sustainable within budget constraints.

#### Acceptance Criteria

1. THE PWA SHALL implement usage limits compatible with AWS free tier constraints
2. THE PWA SHALL monitor and limit Amazon Textract API calls per user session
3. THE PWA SHALL implement rate limiting for Amazon Bedrock AI assistant requests
4. WHEN approaching usage limits, THE PWA SHALL notify users and gracefully degrade functionality
5. THE PWA SHALL provide cost monitoring dashboard for administrators
6. THE PWA SHALL cache frequently requested AI responses to reduce API costs

### Requirement 10: Accessibility and Localization

**User Story:** As a user from rural India with limited English proficiency, I want the application to be accessible and available in my preferred language, so that I can use it effectively.

#### Acceptance Criteria

1. THE PWA SHALL support both English and Hindi user interfaces
2. THE PWA SHALL provide high contrast mode for users with visual impairments
3. THE PWA SHALL support keyboard navigation for all interactive elements
4. THE PWA SHALL use simple, clear language appropriate for users with varying education levels
5. THE PWA MAY provide audio guidance for key tax concepts and processes as a future enhancement
6. THE PWA SHALL work effectively on low-resolution screens and older devices