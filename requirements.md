# Krishi Jeevan – Requirements Document

## 1. Introduction

Krishi Jeevan is a voice-first, offline financial life simulator designed as a mobile game to empower small and marginal farmers in rural India with practical financial intelligence. The application provides a safe, simulated environment where farmers can learn to manage seasonal income, evaluate credit options, build savings, and understand financial risks without real-world consequences.

## 2. Problem Statement

Small and marginal farmers in rural India face significant financial challenges:

- **Seasonal Income Volatility**: Income arrives in lump sums post-harvest, requiring careful management across lean months
- **Predatory Lending**: Limited access to formal credit drives farmers toward high-interest informal lenders
- **Low Financial Literacy**: Lack of understanding about interest rates, loan terms, and financial planning leads to debt traps
- **Risk Vulnerability**: Unexpected shocks (health emergencies, crop failures, price crashes) devastate household finances
- **Limited Digital Access**: Low digital literacy, low-end devices, and poor connectivity restrict access to financial education tools

Traditional financial literacy programs fail to engage farmers effectively due to abstract concepts, language barriers, and lack of practical application.

## 3. Product Vision

Krishi Jeevan transforms financial education into an engaging, experiential learning journey. By simulating real farming seasons with authentic financial scenarios, the app enables farmers to:

- Experience the consequences of financial decisions in a risk-free environment
- Develop intuition about interest rates, loan repayment, and savings
- Build confidence in evaluating credit sources
- Understand the value of insurance and emergency funds
- Make informed financial decisions in their real lives

The product aims to reach 1 million+ farmers across rural India, delivered through an accessible, voice-first, offline-capable mobile application.

## 4. User Personas

### Persona 1: Ramesh – The Marginal Farmer
- **Age**: 38 years
- **Land**: 2 acres
- **Education**: 5th grade
- **Digital Literacy**: Can make calls, send basic messages
- **Device**: Entry-level Android phone (2GB RAM)
- **Connectivity**: Intermittent 2G/3G
- **Language**: Hindi (primary), limited English
- **Financial Behavior**: Takes loans from local moneylender, struggles with repayment
- **Goals**: Understand loan interest, avoid debt trap, save for children's education

### Persona 2: Lakshmi – The Small Farmer
- **Age**: 45 years
- **Land**: 3 acres
- **Education**: 8th grade
- **Digital Literacy**: Uses WhatsApp, watches YouTube videos
- **Device**: Mid-range Android phone (3GB RAM)
- **Connectivity**: 3G/4G available in village center
- **Language**: Tamil (primary), basic Hindi
- **Financial Behavior**: Has bank account but rarely uses formal credit
- **Goals**: Learn about cooperative loans, build emergency savings

### Persona 3: Suresh – The Progressive Farmer
- **Age**: 32 years
- **Land**: 5 acres
- **Education**: 12th grade
- **Digital Literacy**: Comfortable with apps, online payments
- **Device**: Good Android phone (4GB RAM)
- **Connectivity**: Regular 4G access
- **Language**: Kannada, Hindi, basic English
- **Financial Behavior**: Uses bank loans, interested in crop insurance
- **Goals**: Optimize credit mix, understand insurance benefits

## 5. Functional Requirements

### 5.1 User Management
- **FR-1.1**: The system shall allow users to create profiles using voice input for name and basic details
- **FR-1.2**: The system shall support profile creation without requiring internet connectivity
- **FR-1.3**: The system shall store user profiles securely in local encrypted storage
- **FR-1.4**: The system shall sync user profiles to cloud when connectivity is available
- **FR-1.5**: The system shall support multiple user profiles on a single device

### 5.2 Voice Interaction
- **FR-2.1**: The system shall convert user speech to text in regional languages (Hindi, Tamil, Telugu, Kannada, Bengali, Marathi)
- **FR-2.2**: The system shall provide voice responses using text-to-speech in the user's selected language
- **FR-2.3**: The system shall process voice commands with latency under 2 seconds
- **FR-2.4**: The system shall function offline using on-device speech recognition
- **FR-2.5**: The system shall provide visual text alternatives for all voice interactions
- **FR-2.6**: The system shall allow users to switch between voice and touch input modes

### 5.3 Financial Simulation Engine
- **FR-3.1**: The system shall simulate farming seasons with configurable duration (3-12 months)
- **FR-3.2**: The system shall generate seasonal harvest income based on crop type and land size
- **FR-3.3**: The system shall simulate monthly household expenses with realistic variation
- **FR-3.4**: The system shall track user's cash balance in real-time throughout the simulation
- **FR-3.5**: The system shall allow users to save money in a virtual savings account
- **FR-3.6**: The system shall calculate and display interest earned on savings

### 5.4 Loan Ecosystem
- **FR-4.1**: The system shall provide four loan sources: bank, cooperative, input dealer, moneylender
- **FR-4.2**: The system shall display interest rates, repayment terms, and total cost for each loan option
- **FR-4.3**: The system shall calculate compound interest accurately for all loan types
- **FR-4.4**: The system shall track loan principal, interest accrued, and total outstanding amount
- **FR-4.5**: The system shall allow partial and full loan repayments
- **FR-4.6**: The system shall impose penalties for missed repayment deadlines
- **FR-4.7**: The system shall restrict access to formal credit if previous loans default
- **FR-4.8**: The system shall explain loan terms in simple, voice-friendly language

### 5.5 Risk Event System
- **FR-5.1**: The system shall generate random risk events (health emergency, crop loss, price crash, equipment failure)
- **FR-5.2**: The system shall assign realistic probabilities to each risk event type
- **FR-5.3**: The system shall calculate financial impact of risk events on user's balance
- **FR-5.4**: The system shall provide decision points during risk events (take loan, use savings, seek help)
- **FR-5.5**: The system shall demonstrate insurance protection when applicable
- **FR-5.6**: The system shall show consequences of having/not having emergency savings

### 5.6 Insurance Mechanism
- **FR-6.1**: The system shall offer crop insurance and health insurance options
- **FR-6.2**: The system shall deduct premium amounts from user's balance
- **FR-6.3**: The system shall provide payouts when insured events occur
- **FR-6.4**: The system shall explain insurance value through comparative scenarios

### 5.7 Game Progression
- **FR-7.1**: The system shall track user progress across multiple seasons
- **FR-7.2**: The system shall provide performance feedback at season end (debt level, savings, financial health score)
- **FR-7.3**: The system shall unlock advanced scenarios based on user performance
- **FR-7.4**: The system shall maintain a history of user's financial decisions
- **FR-7.5**: The system shall provide replay capability for learning from mistakes

### 5.8 Educational Content
- **FR-8.1**: The system shall provide contextual tips during gameplay
- **FR-8.2**: The system shall explain financial concepts using farmer-relevant examples
- **FR-8.3**: The system shall offer a glossary of financial terms in regional languages
- **FR-8.4**: The system shall provide summary lessons at key decision points

### 5.9 Offline Capability
- **FR-9.1**: The system shall function fully without internet connectivity
- **FR-9.2**: The system shall store all game data locally
- **FR-9.3**: The system shall queue sync operations when offline
- **FR-9.4**: The system shall sync data automatically when connectivity is restored
- **FR-9.5**: The system shall handle sync conflicts gracefully

### 5.10 Multi-Language Support
- **FR-10.1**: The system shall support 6+ regional languages
- **FR-10.2**: The system shall allow language selection at first launch
- **FR-10.3**: The system shall allow language switching mid-game
- **FR-10.4**: The system shall localize all UI text, voice prompts, and content

## 6. Non-Functional Requirements

### 6.1 Performance
- **NFR-1.1**: Voice response latency shall not exceed 2 seconds
- **NFR-1.2**: App launch time shall not exceed 3 seconds on 2GB RAM devices
- **NFR-1.3**: Screen transitions shall complete within 500ms
- **NFR-1.4**: Financial calculations shall complete within 100ms
- **NFR-1.5**: Database queries shall return results within 200ms

### 6.2 Scalability
- **NFR-2.1**: Backend architecture shall support 1 million+ concurrent users
- **NFR-2.2**: Database shall handle 10 million+ user profiles
- **NFR-2.3**: API response time shall remain under 500ms at peak load
- **NFR-2.4**: System shall auto-scale based on traffic patterns

### 6.3 Availability
- **NFR-3.1**: Backend services shall maintain 99.5% uptime
- **NFR-3.2**: Offline functionality shall be available 100% of the time
- **NFR-3.3**: Sync failures shall not impact offline gameplay

### 6.4 Security
- **NFR-4.1**: User data shall be encrypted at rest using AES-256
- **NFR-4.2**: Data transmission shall use TLS 1.3
- **NFR-4.3**: User authentication shall use secure token-based mechanism
- **NFR-4.4**: Personal information shall not be shared with third parties
- **NFR-4.5**: App shall comply with data protection regulations

### 6.5 Usability
- **NFR-5.1**: App shall be usable by users with 5th grade education
- **NFR-5.2**: Voice commands shall have 90%+ recognition accuracy
- **NFR-5.3**: UI shall follow accessibility guidelines for low-literacy users
- **NFR-5.4**: Onboarding shall complete within 5 minutes

### 6.6 Compatibility
- **NFR-6.1**: App shall run on Android 7.0 and above
- **NFR-6.2**: App shall function on devices with 2GB RAM minimum
- **NFR-6.3**: App size shall not exceed 50MB
- **NFR-6.4**: App shall work on screen sizes from 4.5" to 7"

### 6.7 Maintainability
- **NFR-7.1**: Code shall follow modular architecture principles
- **NFR-7.2**: Components shall be independently testable
- **NFR-7.3**: System shall support over-the-air content updates
- **NFR-7.4**: Logging shall capture errors and performance metrics

### 6.8 Localization
- **NFR-8.1**: Language packs shall be downloadable separately
- **NFR-8.2**: New languages shall be addable without code changes
- **NFR-8.3**: Cultural context shall be maintained in translations

## 7. Constraints

### 7.1 Technical Constraints
- Android-first development (iOS not in scope for MVP)
- Must work on low-end devices (2GB RAM, limited storage)
- Must function completely offline
- App size limited to 50MB for easy distribution
- Voice processing must work on-device (no cloud dependency)

### 7.2 Resource Constraints
- Hackathon timeline (limited development time)
- Small development team
- Limited budget for cloud infrastructure
- No access to large-scale user testing pre-launch

### 7.3 User Constraints
- Target users have low digital literacy
- Limited or no English proficiency
- Intermittent or no internet connectivity
- Low-end Android devices
- Limited time for app engagement (15-30 minutes per session)

### 7.4 Regulatory Constraints
- Must comply with data protection laws
- Cannot provide actual financial advice
- Must include disclaimers about simulation nature
- Cannot collect sensitive personal information without consent

## 8. Assumptions

- Users have access to Android smartphones (version 7.0+)
- Users can speak one of the supported regional languages
- Users understand basic farming concepts and seasonal cycles
- Device has functional microphone and speaker
- Users have occasional access to internet for initial download and periodic syncs
- Users are motivated to improve financial literacy
- Local storage of 100MB is available on user devices
- Users can dedicate 15-30 minutes per session

## 9. Edge Cases

### 9.1 Connectivity Edge Cases
- **EC-1.1**: User loses connectivity mid-sync → Queue operations, retry when online
- **EC-1.2**: User never connects to internet → Full offline functionality maintained
- **EC-1.3**: Sync conflict (local and cloud data differ) → Use latest timestamp, preserve both versions

### 9.2 Voice Interaction Edge Cases
- **EC-2.1**: Background noise interferes with speech recognition → Provide visual fallback, retry prompt
- **EC-2.2**: User speaks unsupported language → Detect and prompt for language selection
- **EC-2.3**: Speech recognition fails repeatedly → Switch to touch-based input mode
- **EC-2.4**: Device lacks microphone → Disable voice features, use text input only

### 9.3 Financial Simulation Edge Cases
- **EC-3.1**: User accumulates excessive debt → Trigger bankruptcy scenario, restart season with lessons
- **EC-3.2**: User achieves negative balance → Prevent further spending, force loan decision
- **EC-3.3**: Multiple risk events in single season → Cap total impact at realistic threshold
- **EC-3.4**: User attempts to repay more than owed → Accept payment, return excess to balance

### 9.4 Storage Edge Cases
- **EC-4.1**: Device storage full → Prompt user to clear space, prevent data loss
- **EC-4.2**: App data corrupted → Restore from last valid backup, notify user
- **EC-4.3**: User clears app cache → Preserve core game data, re-download assets

### 9.5 Multi-User Edge Cases
- **EC-5.1**: Multiple users share device → Maintain separate profiles, require profile selection
- **EC-5.2**: User forgets which profile is theirs → Provide profile hints (creation date, progress level)

## 10. Acceptance Criteria

### 10.1 Core Functionality
- **AC-1.1**: User can complete full farming season simulation without internet
- **AC-1.2**: User can take loan, track interest, and repay using voice commands
- **AC-1.3**: Risk events occur with appropriate frequency (1-2 per season)
- **AC-1.4**: Financial calculations are accurate to 2 decimal places
- **AC-1.5**: User can save progress and resume later

### 10.2 Performance Metrics
- **AC-2.1**: Voice response latency < 2 seconds in 95% of interactions
- **AC-2.2**: App launches in < 3 seconds on 2GB RAM device
- **AC-2.3**: App size remains under 50MB
- **AC-2.4**: Battery consumption < 5% per 30-minute session

### 10.3 Usability Metrics
- **AC-3.1**: 80% of test users complete onboarding without assistance
- **AC-3.2**: Voice recognition accuracy > 90% in quiet environment
- **AC-3.3**: Users understand loan interest differences after 1 season
- **AC-3.4**: 70% of users can explain insurance value after experiencing risk event

### 10.4 Technical Metrics
- **AC-4.1**: Zero data loss during offline-online transitions
- **AC-4.2**: Sync completes within 10 seconds for typical user data
- **AC-4.3**: App handles 100 consecutive seasons without performance degradation
- **AC-4.4**: Crash rate < 0.1% of sessions

### 10.5 Educational Impact
- **AC-5.1**: Users demonstrate improved understanding of interest rates (pre/post assessment)
- **AC-5.2**: Users can identify safer credit sources after 3 seasons
- **AC-5.3**: Users recognize value of emergency savings through gameplay

## 11. Future Scope

### 11.1 Phase 2 Features
- Multiplayer mode (farmers can help each other)
- Community lending circles simulation
- Government scheme integration (PM-KISAN, crop insurance)
- Advanced crop selection and market price dynamics
- Weather-based risk modeling
- Livestock and dairy income streams

### 11.2 Phase 3 Features
- iOS version
- Integration with actual financial institutions for real loan applications
- Personalized financial coaching based on gameplay patterns
- Social features (leaderboards, achievements, sharing)
- AR visualization of financial concepts
- Integration with farmer producer organizations (FPOs)

### 11.3 Platform Expansion
- USSD version for feature phones
- WhatsApp bot integration
- Web-based version for NGO facilitators
- Tablet version for group training sessions

### 11.4 Content Expansion
- Region-specific crop calendars
- State-specific government schemes
- Customized scenarios for different farming types (rain-fed, irrigated, horticulture)
- Women farmer-specific scenarios

## 12. Risk Analysis

### 12.1 Technical Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Voice recognition accuracy low on low-end devices | High | Medium | Provide text fallback, optimize models, test extensively |
| App size exceeds 50MB limit | Medium | Medium | Modular architecture, downloadable language packs, asset optimization |
| Offline sync conflicts cause data loss | High | Low | Robust conflict resolution, local backups, timestamp-based merging |
| Performance issues on 2GB RAM devices | High | Medium | Extensive testing, memory optimization, lazy loading |
| Battery drain from voice processing | Medium | Medium | Optimize voice engine, provide power-saving mode |

### 12.2 User Adoption Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Low digital literacy prevents app usage | High | Medium | Extensive onboarding, voice-first design, visual simplicity |
| Users don't trust simulation value | High | Medium | Real farmer testimonials, NGO partnerships, demonstration sessions |
| Language support insufficient | Medium | Medium | Prioritize top 6 languages, community translation support |
| Users lack motivation to complete seasons | Medium | High | Gamification, short sessions, immediate feedback, rewards |

### 12.3 Business Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Inability to reach target users | High | Medium | Partner with NGOs, government programs, farmer cooperatives |
| Funding constraints limit scaling | Medium | Medium | Demonstrate impact metrics, seek grants, CSR partnerships |
| Regulatory challenges with financial content | Medium | Low | Clear disclaimers, legal review, avoid actual financial advice |
| Competition from existing solutions | Low | Low | Focus on unique voice-first, offline, simulation approach |

### 12.4 Content Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Financial scenarios not realistic | High | Medium | Consult domain experts, farmer feedback, iterative refinement |
| Cultural insensitivity in content | Medium | Low | Local language experts, cultural consultants, user testing |
| Oversimplification reduces learning value | Medium | Medium | Balance simplicity with accuracy, progressive complexity |

### 13. System Boundaries
The system does not:
- Connect directly to banking systems (MVP phase)
- Store actual financial transaction data
---

**Document Version**: 1.0  
**Last Updated**: February 14, 2026  
**Status**: Hackathon Submission Ready
