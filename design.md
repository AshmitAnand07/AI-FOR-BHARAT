# Krishi Jeevan – Technical Design Document

## 1. System Overview

Krishi Jeevan is architected as an offline-first, voice-enabled mobile application that simulates financial scenarios for farmer education. The system employs a layered architecture with clear separation between presentation, business logic, data persistence, and backend services.

The design prioritizes:
- Offline-first operation with eventual consistency
- Low resource consumption for entry-level devices
- Modular components for independent scaling and testing
- Voice interaction as primary interface
- Secure local data storage with cloud synchronization

## 2. High-Level Architecture

### 2.1 Architectural Layers

```
┌─────────────────────────────────────────────────────────┐
│                  Presentation Layer                      │
│  (Voice UI, Touch UI, Navigation, State Management)     │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                   Application Layer                      │
│  (Simulation Engine, Voice Engine, Game Controller)     │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                    Business Logic Layer                  │
│  (Financial Calculator, Risk Generator, Loan Manager)   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                      Data Layer                          │
│  (Local DB, Cache, Sync Queue, Encryption)              │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                    Backend Services                      │
│  (API Gateway, Analytics, Content Management, Auth)     │
└─────────────────────────────────────────────────────────┘
```

### 2.2 Architectural Principles

- **Offline-First**: All core functionality works without network
- **Event-Driven**: Components communicate via events for loose coupling
- **Stateless Backend**: Backend services are stateless for horizontal scaling
- **Eventual Consistency**: Data syncs when connectivity available
- **Fail-Safe**: Graceful degradation when services unavailable

## 3. Component Breakdown

### 3.1 Frontend (Android Application)

**Technology Stack**:
- Kotlin for application code
- Jetpack Compose for UI
- Coroutines for async operations
- Room for local database
- WorkManager for background sync

**Sub-Components**:

**3.1.1 Voice Interface Module**
- Speech-to-Text (STT) using on-device ML Kit
- Text-to-Speech (TTS) using Android TTS engine
- Voice command parser and intent recognition
- Audio input quality detector
- Fallback to text input on failure

**3.1.2 UI Components**
- Dashboard (balance, loans, savings overview)
- Loan marketplace (compare options)
- Transaction history
- Risk event dialogs
- Season summary screens
- Settings and profile management

**3.1.3 Navigation Controller**
- Screen routing based on game state
- Deep linking support
- Back stack management
- State preservation across configuration changes

**3.1.4 State Management**
- ViewModel for UI state
- Repository pattern for data access
- LiveData/Flow for reactive updates
- Saved state handling

### 3.2 Simulation Engine

**Core Responsibilities**:
- Season progression logic
- Time advancement (days, months)
- Event scheduling and triggering
- Game state management
- Win/loss condition evaluation

**Sub-Components**:

**3.2.1 Season Manager**
```kotlin
class SeasonManager {
    fun startSeason(config: SeasonConfig): Season
    fun advanceTime(days: Int): List<Event>
    fun endSeason(): SeasonSummary
    fun calculatePerformance(): FinancialHealthScore
}
```

**3.2.2 Financial State Tracker**
- Current balance
- Active loans with interest accrual
- Savings account balance
- Insurance policies
- Transaction ledger

**3.2.3 Decision Engine**
- Present choices to user
- Validate user decisions
- Apply consequences
- Track decision history

### 3.3 Voice Engine

**Architecture**:

```
User Speech → STT Engine → Intent Parser → Command Executor → Response Generator → TTS Engine → Audio Output
```

**Components**:

**3.3.1 Speech-to-Text Processor**
- On-device ML Kit Speech Recognition
- Language model per supported language
- Noise cancellation preprocessing
- Confidence scoring
- Retry mechanism for low confidence

**3.3.2 Intent Recognition**
- Natural language understanding for farmer context
- Command mapping (take loan, check balance, repay, etc.)
- Entity extraction (amounts, loan types, time periods)
- Contextual disambiguation

**3.3.3 Text-to-Speech Generator**
- Android TTS with regional language support
- Response templating system
- Dynamic content insertion
- Speech rate optimization for comprehension

**3.3.4 Voice Command Library**
```kotlin
sealed class VoiceCommand {
    data class CheckBalance(val accountType: AccountType) : VoiceCommand()
    data class TakeLoan(val source: LoanSource, val amount: Double) : VoiceCommand()
    data class RepayLoan(val loanId: String, val amount: Double) : VoiceCommand()
    data class ViewLoans(val filter: LoanFilter?) : VoiceCommand()
    object AdvanceTime : VoiceCommand()
}
```

### 3.4 Financial Simulation Engine

**3.4.1 Loan Calculator**
```kotlin
class LoanCalculator {
    fun calculateInterest(
        principal: Double,
        rate: Double,
        period: Int,
        compoundingFrequency: CompoundingFrequency
    ): Double
    
    fun calculateEMI(principal: Double, rate: Double, tenure: Int): Double
    
    fun generateAmortizationSchedule(loan: Loan): List<Payment>
    
    fun calculateTotalCost(loan: Loan): Double
}
```

**3.4.2 Loan Source Models**
```kotlin
data class LoanSource(
    val type: LoanType, // BANK, COOPERATIVE, INPUT_DEALER, MONEYLENDER
    val interestRate: Double,
    val processingFee: Double,
    val maxAmount: Double,
    val minTenure: Int,
    val maxTenure: Int,
    val collateralRequired: Boolean,
    val approvalTime: Int, // in days
    val penaltyRate: Double
)
```

**3.4.3 Interest Accrual Engine**
- Daily interest calculation
- Compound interest logic
- Penalty calculation for missed payments
- Grace period handling

**3.4.4 Repayment Manager**
- Track payment schedules
- Process partial/full payments
- Update loan balances
- Generate payment receipts
- Handle early repayment

### 3.5 Risk Event System

**3.5.1 Event Generator**
```kotlin
class RiskEventGenerator {
    fun generateEvents(season: Season, profile: FarmerProfile): List<RiskEvent>
    
    private fun calculateProbability(
        eventType: RiskEventType,
        seasonPhase: SeasonPhase,
        userHistory: UserHistory
    ): Double
}
```

**3.5.2 Risk Event Types**
```kotlin
sealed class RiskEvent {
    data class HealthEmergency(val cost: Double, val severity: Severity) : RiskEvent()
    data class CropLoss(val percentage: Double, val cause: Cause) : RiskEvent()
    data class PriceCrash(val priceReduction: Double) : RiskEvent()
    data class EquipmentFailure(val repairCost: Double) : RiskEvent()
    data class LivestockLoss(val count: Int, val value: Double) : RiskEvent()
}
```

**3.5.3 Probability Model**
- Base probability per event type
- Seasonal modifiers (monsoon → higher crop loss risk)
- User behavior modifiers (no insurance → higher impact)
- Realistic frequency (1-2 events per season)

**3.5.4 Impact Calculator**
- Financial impact on balance
- Insurance mitigation calculation
- Savings buffer protection
- Forced decision triggers

### 3.6 Data Layer

**3.6.1 Local Database Schema (Room)**

```sql
-- User Profile
CREATE TABLE user_profile (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    language TEXT NOT NULL,
    created_at INTEGER NOT NULL,
    last_played INTEGER,
    total_seasons INTEGER DEFAULT 0
);

-- Season Data
CREATE TABLE season (
    id TEXT PRIMARY KEY,
    user_id TEXT NOT NULL,
    season_number INTEGER NOT NULL,
    start_date INTEGER NOT NULL,
    end_date INTEGER,
    initial_balance REAL NOT NULL,
    final_balance REAL,
    status TEXT NOT NULL, -- ACTIVE, COMPLETED, ABANDONED
    FOREIGN KEY (user_id) REFERENCES user_profile(id)
);

-- Transactions
CREATE TABLE transaction (
    id TEXT PRIMARY KEY,
    season_id TEXT NOT NULL,
    type TEXT NOT NULL, -- INCOME, EXPENSE, LOAN_TAKEN, LOAN_REPAID, SAVINGS
    amount REAL NOT NULL,
    balance_after REAL NOT NULL,
    description TEXT,
    timestamp INTEGER NOT NULL,
    FOREIGN KEY (season_id) REFERENCES season(id)
);

-- Loans
CREATE TABLE loan (
    id TEXT PRIMARY KEY,
    season_id TEXT NOT NULL,
    source_type TEXT NOT NULL,
    principal REAL NOT NULL,
    interest_rate REAL NOT NULL,
    outstanding_principal REAL NOT NULL,
    outstanding_interest REAL NOT NULL,
    taken_date INTEGER NOT NULL,
    due_date INTEGER NOT NULL,
    status TEXT NOT NULL, -- ACTIVE, REPAID, DEFAULTED
    FOREIGN KEY (season_id) REFERENCES season(id)
);

-- Risk Events
CREATE TABLE risk_event (
    id TEXT PRIMARY KEY,
    season_id TEXT NOT NULL,
    event_type TEXT NOT NULL,
    impact_amount REAL NOT NULL,
    occurred_date INTEGER NOT NULL,
    user_response TEXT,
    FOREIGN KEY (season_id) REFERENCES season(id)
);

-- Sync Queue
CREATE TABLE sync_queue (
    id TEXT PRIMARY KEY,
    entity_type TEXT NOT NULL,
    entity_id TEXT NOT NULL,
    operation TEXT NOT NULL, -- INSERT, UPDATE, DELETE
    payload TEXT NOT NULL,
    created_at INTEGER NOT NULL,
    synced INTEGER DEFAULT 0
);
```

**3.6.2 Encryption Layer**
- AES-256 encryption for sensitive data
- SQLCipher for database encryption
- Secure key storage using Android Keystore
- Encrypted SharedPreferences for settings

**3.6.3 Cache Manager**
- In-memory cache for active season data
- LRU cache for frequently accessed data
- Cache invalidation on data updates
- Memory-efficient caching for low-end devices

### 3.7 Backend Services

**Technology Stack**:
- Node.js with Express for API
- PostgreSQL for relational data
- Redis for caching and session management
- AWS S3 for content storage
- AWS Lambda for serverless functions

**3.7.1 API Gateway**
```
POST   /api/v1/auth/register
POST   /api/v1/auth/login
GET    /api/v1/user/profile
PUT    /api/v1/user/profile
POST   /api/v1/sync/upload
GET    /api/v1/sync/download
POST   /api/v1/analytics/events
GET    /api/v1/content/scenarios
GET    /api/v1/content/language-pack/:lang
```

**3.7.2 Sync Service**
- Receives sync queue from mobile app
- Resolves conflicts using timestamp-based strategy
- Merges data into cloud database
- Returns updated data to client
- Handles batch operations for efficiency

**3.7.3 Analytics Service**
- Collects anonymized gameplay data
- Tracks user progress and learning outcomes
- Generates insights on financial behavior patterns
- Provides dashboards for program evaluation

**3.7.4 Content Management Service**
- Manages scenario configurations
- Stores localized content
- Provides versioned content updates
- Supports A/B testing of scenarios

**3.7.5 Authentication Service**
- Token-based authentication (JWT)
- Refresh token mechanism
- Device fingerprinting
- Rate limiting

## 4. Data Flow Description

### 4.1 Typical User Session Flow

```
1. App Launch
   → Load user profile from local DB
   → Check for pending sync operations
   → Initialize voice engine
   → Load active season or prompt new season

2. Voice Interaction
   → User speaks command
   → STT converts to text
   → Intent parser identifies command
   → Command executor invokes business logic
   → State updates in memory and DB
   → Response generated
   → TTS speaks response

3. Financial Transaction
   → User initiates transaction (loan, repayment, savings)
   → Validation checks (sufficient balance, loan eligibility)
   → Financial calculator computes impact
   → Transaction recorded in local DB
   → Balance updated
   → UI refreshed
   → Confirmation provided

4. Risk Event
   → Event generator triggers event based on probability
   → Event details presented to user
   → User makes decision
   → Impact calculated and applied
   → Event logged
   → Learning moment provided

5. Season End
   → Calculate final balance, debt, savings
   → Generate performance score
   → Provide feedback and insights
   → Unlock next level scenarios
   → Prompt for new season

6. Background Sync (when online)
   → WorkManager triggers sync job
   → Fetch items from sync queue
   → Upload to backend API
   → Receive server response
   → Update local DB with server data
   → Mark items as synced
   → Clear sync queue
```

### 4.2 Offline-to-Online Transition

```
Offline State:
- All operations write to local DB
- Changes queued in sync_queue table
- User continues gameplay uninterrupted

Connectivity Detected:
- WorkManager triggers sync worker
- Sync worker batches queued operations
- API calls made with retry logic
- Server processes and responds
- Local DB updated with server timestamps
- Conflicts resolved (latest timestamp wins)
- Sync queue cleared

Sync Failure:
- Operations remain in queue
- Retry with exponential backoff
- User notified only if critical
- Offline functionality continues
```

## 5. Offline-First Synchronization Mechanism

### 5.1 Sync Strategy

**Eventual Consistency Model**:
- Local DB is source of truth for user
- Cloud DB aggregates data from all users
- Sync happens opportunistically
- Conflicts resolved automatically

**Sync Queue Design**:
```kotlin
data class SyncOperation(
    val id: String,
    val entityType: EntityType,
    val entityId: String,
    val operation: Operation, // INSERT, UPDATE, DELETE
    val payload: String, // JSON serialized entity
    val timestamp: Long,
    val synced: Boolean = false
)
```

### 5.2 Conflict Resolution

**Timestamp-Based Strategy**:
- Each entity has `created_at` and `updated_at` timestamps
- On conflict, latest timestamp wins
- Both versions logged for audit
- User notified only for critical conflicts

**Conflict Scenarios**:
1. User edits profile on device A, then device B
   → Latest edit wins, older discarded

2. User plays season on device A offline, then device B
   → Both seasons preserved, user chooses which to continue

3. Backend updates content while user offline
   → User gets update on next sync, current session unaffected

### 5.3 Sync Optimization

- Batch operations to reduce API calls
- Compress payload for bandwidth efficiency
- Delta sync (only changed data)
- Priority queue (critical data synced first)
- Background sync during idle time

## 6. Financial Simulation Engine Design

### 6.1 Core Calculation Logic

**Interest Calculation**:
```kotlin
fun calculateCompoundInterest(
    principal: Double,
    annualRate: Double,
    timeInDays: Int,
    compoundingFrequency: Int // times per year
): Double {
    val rate = annualRate / 100.0
    val time = timeInDays / 365.0
    val n = compoundingFrequency.toDouble()
    return principal * Math.pow(1 + rate/n, n * time) - principal
}
```

**Loan Comparison Engine**:
```kotlin
data class LoanComparison(
    val source: LoanSource,
    val requestedAmount: Double,
    val tenure: Int,
    val totalInterest: Double,
    val totalRepayment: Double,
    val monthlyEMI: Double,
    val effectiveRate: Double
)

fun compareLoanOptions(
    amount: Double,
    tenure: Int,
    sources: List<LoanSource>
): List<LoanComparison> {
    return sources.map { source ->
        val interest = calculateCompoundInterest(...)
        val total = amount + interest
        val emi = total / tenure
        LoanComparison(source, amount, tenure, interest, total, emi, ...)
    }.sortedBy { it.totalRepayment }
}
```

### 6.2 Realistic Financial Modeling

**Income Modeling**:
- Seasonal harvest income (lump sum)
- Variable based on crop type and land size
- Market price fluctuations (±20%)
- Bonus income opportunities (labor, side business)

**Expense Modeling**:
- Fixed monthly household expenses
- Variable expenses (festivals, social obligations)
- Input costs (seeds, fertilizer, labor)
- Unexpected expenses (repairs, health)

**Savings Modeling**:
- Simple interest on savings account (3-4% annual)
- Daily interest accrual
- No withdrawal penalties
- Encouragement through visual progress

## 7. Risk Event Logic Model

### 7.1 Probability Distribution

```kotlin
enum class RiskEventType(val baseProbability: Double) {
    HEALTH_EMERGENCY(0.15),      // 15% per season
    CROP_LOSS_MINOR(0.25),       // 25% per season
    CROP_LOSS_MAJOR(0.08),       // 8% per season
    PRICE_CRASH(0.20),           // 20% per season
    EQUIPMENT_FAILURE(0.12),     // 12% per season
    LIVESTOCK_LOSS(0.10)         // 10% per season
}
```

### 7.2 Event Timing

- Events distributed across season
- Higher probability in vulnerable phases (monsoon for crop loss)
- Minimum gap between events (15 days)
- Maximum events per season (2-3)

### 7.3 Impact Calculation

```kotlin
fun calculateEventImpact(
    event: RiskEvent,
    userState: FinancialState
): EventImpact {
    val baseCost = event.baseCost
    
    // Insurance mitigation
    val insuranceCoverage = if (userState.hasInsurance(event.type)) {
        baseCost * 0.70 // 70% covered
    } else {
        0.0
    }
    
    // Savings buffer
    val savingsProtection = min(userState.savings, baseCost * 0.30)
    
    val netImpact = baseCost - insuranceCoverage - savingsProtection
    
    return EventImpact(
        grossCost = baseCost,
        insuranceCovered = insuranceCoverage,
        savingsUsed = savingsProtection,
        netCost = netImpact,
        forcedLoan = netImpact > userState.balance
    )
}
```

## 8. Database Schema Overview

### 8.1 Entity Relationships

```
user_profile (1) ──→ (N) season
season (1) ──→ (N) transaction
season (1) ──→ (N) loan
season (1) ──→ (N) risk_event
loan (1) ──→ (N) loan_payment
```

### 8.2 Indexing Strategy

```sql
CREATE INDEX idx_season_user ON season(user_id, season_number);
CREATE INDEX idx_transaction_season ON transaction(season_id, timestamp);
CREATE INDEX idx_loan_season_status ON loan(season_id, status);
CREATE INDEX idx_sync_queue_synced ON sync_queue(synced, created_at);
```

### 8.3 Data Retention

- Active season data: Kept indefinitely
- Completed seasons: Kept for 2 years
- Sync queue: Cleared after successful sync
- Analytics events: Aggregated and archived monthly

## 9. API Design Overview

### 9.1 Authentication Flow

```
1. Register/Login
   POST /api/v1/auth/register
   Body: { "deviceId": "...", "name": "...", "language": "..." }
   Response: { "token": "...", "refreshToken": "...", "userId": "..." }

2. Subsequent Requests
   Header: Authorization: Bearer <token>

3. Token Refresh
   POST /api/v1/auth/refresh
   Body: { "refreshToken": "..." }
   Response: { "token": "...", "refreshToken": "..." }
```

### 9.2 Sync API

```
POST /api/v1/sync/upload
Header: Authorization: Bearer <token>
Body: {
  "operations": [
    {
      "entityType": "season",
      "operation": "INSERT",
      "payload": { ... },
      "timestamp": 1234567890
    },
    ...
  ]
}
Response: {
  "synced": 15,
  "conflicts": [],
  "serverTimestamp": 1234567900
}
```

### 9.3 Content API

```
GET /api/v1/content/scenarios?version=1.0&language=hi
Response: {
  "version": "1.1",
  "scenarios": [
    {
      "id": "season_1",
      "difficulty": "beginner",
      "config": { ... }
    }
  ]
}
```

### 9.4 Analytics API

```
POST /api/v1/analytics/events
Body: {
  "events": [
    {
      "type": "season_completed",
      "userId": "...",
      "data": { "score": 75, "debt": 5000 },
      "timestamp": 1234567890
    }
  ]
}
```

## 10. Security Architecture

### 10.1 Data Protection

**At Rest**:
- SQLCipher for database encryption (AES-256)
- Android Keystore for encryption key management
- Encrypted SharedPreferences for sensitive settings
- No PII stored in plain text

**In Transit**:
- TLS 1.3 for all API communication
- Certificate pinning to prevent MITM attacks
- Request signing for integrity verification

### 10.2 Authentication & Authorization

- JWT tokens with 1-hour expiry
- Refresh tokens with 30-day expiry
- Device fingerprinting for anomaly detection
- Rate limiting (100 requests/minute per user)

### 10.3 Privacy

- Minimal data collection (no PII required)
- Anonymized analytics
- No third-party tracking
- User data deletion on request
- GDPR compliance

### 10.4 Code Security

- ProGuard obfuscation for release builds
- Root detection
- Tamper detection
- Secure random number generation
- Input validation and sanitization

## 11. Scalability Model

### 11.1 Mobile App Scalability

**Memory Management**:
- Lazy loading of resources
- Bitmap recycling
- Pagination for transaction history
- Background task optimization

**Storage Optimization**:
- Database vacuuming
- Old data archival
- Compressed assets
- Incremental content downloads

### 11.2 Backend Scalability

**Horizontal Scaling**:
```
Load Balancer
    ↓
API Server 1, API Server 2, ..., API Server N
    ↓
Database Read Replicas (PostgreSQL)
    ↓
Primary Database (PostgreSQL)
```

**Caching Strategy**:
- Redis for session data
- CDN for static content
- API response caching (5-minute TTL)
- Database query result caching

**Database Scaling**:
- Read replicas for analytics queries
- Partitioning by user_id
- Archival of old data
- Connection pooling

### 11.3 Performance Targets

| Metric | Target | Strategy |
|--------|--------|----------|
| API Response Time | < 500ms | Caching, indexing, query optimization |
| Database Query Time | < 200ms | Proper indexing, query optimization |
| Concurrent Users | 1M+ | Horizontal scaling, load balancing |
| Data Storage | 10M+ users | Partitioning, archival |
| Sync Throughput | 1000 ops/sec | Batch processing, async workers |

## 12. Deployment Architecture

### 12.1 Mobile App Deployment

**Distribution**:
- Google Play Store (primary)
- APK direct download (for areas with limited Play Store access)
- Progressive rollout (10% → 50% → 100%)

**Update Strategy**:
- Forced updates for critical security patches
- Optional updates for feature releases
- In-app update prompts
- Background download when on WiFi

### 12.2 Backend Deployment

**Infrastructure** (AWS):
```
Route 53 (DNS)
    ↓
CloudFront (CDN)
    ↓
Application Load Balancer
    ↓
ECS Fargate (API Containers)
    ↓
RDS PostgreSQL (Multi-AZ)
    ↓
ElastiCache Redis
    ↓
S3 (Content Storage)
```

**CI/CD Pipeline**:
```
Code Push → GitHub Actions → Build & Test → Docker Image → ECR → ECS Deploy
```

**Environments**:
- Development (dev.api.krishijeevan.in)
- Staging (staging.api.krishijeevan.in)
- Production (api.krishijeevan.in)

### 12.3 Disaster Recovery

- Automated daily backups (RDS)
- Point-in-time recovery (35-day retention)
- Multi-AZ deployment for high availability
- Backup region for disaster recovery
- Recovery Time Objective (RTO): 4 hours
- Recovery Point Objective (RPO): 1 hour

## 13. Monitoring and Analytics Strategy

### 13.1 Application Monitoring

**Mobile App**:
- Firebase Crashlytics for crash reporting
- Firebase Performance Monitoring
- Custom events for user actions
- ANR (Application Not Responding) tracking
- Network performance metrics

**Backend**:
- CloudWatch for infrastructure metrics
- Application logs (structured JSON)
- API latency tracking
- Error rate monitoring
- Database performance metrics

### 13.2 Business Analytics

**User Engagement**:
- Daily/Monthly Active Users (DAU/MAU)
- Session duration
- Seasons completed
- Feature usage rates
- Retention cohorts

**Learning Outcomes**:
- Financial decisions quality score
- Loan choice patterns
- Savings behavior
- Insurance adoption rate
- Risk response effectiveness

**Technical Metrics**:
- App crash rate
- API error rate
- Sync success rate
- Voice recognition accuracy
- App performance on device tiers

### 13.3 Alerting

**Critical Alerts** (PagerDuty):
- API error rate > 5%
- Database connection failures
- Sync service down
- Crash rate > 1%

**Warning Alerts** (Email):
- API latency > 1s
- Disk usage > 80%
- Memory usage > 85%
- Unusual traffic patterns

## 14. Technical Tradeoffs

### 14.1 Offline-First vs Cloud-First

**Decision**: Offline-First

**Rationale**:
- Target users have unreliable connectivity
- Core learning experience must work offline
- Reduces infrastructure costs
- Better user experience in rural areas

**Tradeoffs**:
- More complex sync logic
- Potential data conflicts
- Larger app size (includes all logic)
- Limited real-time features

### 14.2 Native Android vs Cross-Platform

**Decision**: Native Android (Kotlin)

**Rationale**:
- Better performance on low-end devices
- Full access to Android APIs (voice, storage)
- Smaller app size
- Better offline capabilities

**Tradeoffs**:
- No iOS version initially
- Longer development time for multi-platform
- Platform-specific expertise required

### 14.3 On-Device Voice vs Cloud Voice

**Decision**: On-Device Voice (ML Kit)

**Rationale**:
- Works offline (critical requirement)
- No latency from network calls
- Privacy (no voice data sent to cloud)
- No API costs

**Tradeoffs**:
- Lower accuracy than cloud services
- Larger app size (voice models)
- Limited language support
- Device resource consumption

### 14.4 SQLite vs NoSQL

**Decision**: SQLite (Room)

**Rationale**:
- Structured financial data fits relational model
- ACID transactions for data integrity
- Mature Android support
- Efficient for query patterns

**Tradeoffs**:
- Less flexible schema
- Requires migrations for schema changes
- Not ideal for unstructured data

### 14.5 Monolithic vs Microservices Backend

**Decision**: Modular Monolith

**Rationale**:
- Simpler deployment for MVP
- Lower operational complexity
- Sufficient for 1M users
- Can split into microservices later

**Tradeoffs**:
- Harder to scale individual components
- Tighter coupling initially
- Single point of failure

### 14.6 Real-Time Sync vs Batch Sync

**Decision**: Batch Sync

**Rationale**:
- Reduces API calls and costs
- Better battery life
- Acceptable for use case (not real-time game)
- Handles intermittent connectivity better

**Tradeoffs**:
- Data not immediately in cloud
- Potential for larger conflicts
- Delayed analytics insights

---

## 15. Implementation Roadmap

### Phase 1: MVP (Hackathon Scope)
- Core simulation engine
- Basic voice interaction (Hindi only)
- 4 loan sources with interest calculation
- 3 risk event types
- Local database with encryption
- Offline-first architecture
- Basic UI with essential screens

### Phase 2: Enhancement
- Multi-language support (6 languages)
- Advanced risk events
- Insurance mechanism
- Backend API and sync
- Analytics dashboard
- Performance optimization

### Phase 3: Scale
- iOS version
- Advanced scenarios
- Social features
- Integration with financial institutions
- Regional customization
- NGO partnership tools

## System Boundaries

The system does not:
- Connect to real banking systems (MVP)
- Store real financial transaction data
- Perform credit scoring
- Replace certified financial education programs

---

**Document Version**: 1.0  
**Last Updated**: February 14, 2026  
**Status**: Hackathon Submission Ready  
**Architecture Review**: Approved for Implementation
