# SMS Sender App - Technical Architecture

## 🏗️ Architecture Overview

The SMS Sender App follows a modern Android architecture pattern with clean separation of concerns, reactive programming, and robust data management.

## 📐 Architecture Pattern

### MVVM (Model-View-ViewModel)
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│      View       │    │   ViewModel     │    │      Model      │
│                 │◄──►│                 │◄──►│                 │
│ • UI Components │    │ • Business Logic│    │ • Data Sources  │
│ • User Actions  │    │ • State Mgmt    │    │ • Repositories  │
│ • Data Binding  │    │ • Data Flow     │    │ • Entities      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Key Principles
- **Single Responsibility**: Each component has a specific purpose
- **Separation of Concerns**: UI, business logic, and data are separated
- **Reactive Programming**: Data flows reactively through the system
- **Dependency Injection**: Loose coupling through DI

## 🗂️ Project Structure

```
📁 SMS Sender App
├── 📁 app/
│   ├── 📁 src/main/java/com/example/smssenderapp/
│   │   ├── 📁 data/                    # Data Layer
│   │   │   ├── 📁 dao/                 # Data Access Objects
│   │   │   ├── 📁 entities/            # Database Entities
│   │   │   ├── 📁 firebase/            # Firebase Services
│   │   │   ├── 📁 models/              # Data Models
│   │   │   └── AppDatabase.kt          # Room Database
│   │   ├── 📁 di/                      # Dependency Injection
│   │   │   ├── AppModule.kt
│   │   │   ├── DatabaseModule.kt
│   │   │   ├── FirebaseModule.kt
│   │   │   └── WorkerModule.kt
│   │   ├── 📁 ui/                      # Presentation Layer
│   │   │   ├── 📁 compose/             # Jetpack Compose UI
│   │   │   ├── 📁 dashboard/           # Dashboard UI
│   │   │   ├── 📁 lists/               # Contact Lists UI
│   │   │   ├── 📁 manage/              # Campaign Management UI
│   │   │   ├── 📁 send/                # Send Message UI
│   │   │   ├── 📁 settings/            # Settings UI
│   │   │   └── 📁 theme/               # UI Theme
│   │   ├── 📁 service/                 # Background Services
│   │   │   ├── JobManager.kt
│   │   │   └── SmsService.kt
│   │   ├── 📁 workers/                 # WorkManager Workers
│   │   │   ├── AutoResumeWorker.kt
│   │   │   ├── CampaignStateSyncWorker.kt
│   │   │   └── CampaignSyncWorker.kt
│   │   ├── 📁 utils/                   # Utility Classes
│   │   │   ├── AlarmScheduler.kt
│   │   │   ├── AppUtils.kt
│   │   │   ├── DatabaseUtils.kt
│   │   │   ├── HashUtils.kt
│   │   │   └── NotificationHelper.kt
│   │   └── 📁 receivers/               # Broadcast Receivers
│   │       ├── AutoStartAlarmReceiver.kt
│   │       └── BootCompletedReceiver.kt
│   └── 📁 res/                         # Resources
└── 📁 gradle/                          # Build Configuration
```

## 🗄️ Data Layer Architecture

### Database Schema (Room)
```sql
-- Core Tables
campaigns (id, name, description, status, totalContacts, messagesSent, dailyLimit, contactsReached, todayMessagesSent)
contacts (id, campaignId, phoneNumber, message, delay, status, sentAt, listContactId)
campaign_lists (id, name, description, createdAt)
contact_lists (id, campaignListId, phoneNumber, name, createdAt)
dashboard_metrics (id, dailyMessageCount, dailyLimit, totalCampaigns, activeCampaigns)
settings (id, key, value)

-- Relationships
campaigns 1:N contacts
campaign_lists 1:N contact_lists
```

### Data Access Objects (DAOs)
- **CampaignDao**: Campaign CRUD operations
- **ContactDao**: Contact management
- **CampaignListDao**: Contact list operations
- **DashboardMetricsDao**: Analytics data access
- **SettingsDao**: User preferences

### Repository Pattern
```kotlin
interface CampaignRepository {
    suspend fun getCampaigns(): Flow<List<Campaign>>
    suspend fun createCampaign(campaign: Campaign): Long
    suspend fun updateCampaign(campaign: Campaign)
    suspend fun deleteCampaign(campaignId: Long)
}
```

## 🔄 Reactive Data Flow

### State Management
```kotlin
// ViewModel State
data class DashboardUiState(
    val isLoading: Boolean = false,
    val metrics: DashboardMetrics = DashboardMetrics(),
    val activeCampaigns: List<Campaign> = emptyList(),
    val activities: List<ActivityItem> = emptyList(),
    val error: String? = null
)

// Data Flow
Repository → ViewModel → UI State → Compose UI
```

### Coroutines & Flow
- **Coroutines**: Asynchronous operations
- **Flow**: Reactive data streams
- **StateFlow**: UI state management
- **SharedFlow**: Event broadcasting

## 🔧 Dependency Injection (Hilt)

### Module Structure
```kotlin
@Module
@InstallIn(SingletonComponent::class)
object DatabaseModule {
    @Provides
    fun provideDatabase(@ApplicationContext context: Context): AppDatabase
    
    @Provides
    fun provideCampaignDao(database: AppDatabase): CampaignDao
}

@Module
@InstallIn(SingletonComponent::class)
object FirebaseModule {
    @Provides
    fun provideFirebaseAuth(): FirebaseAuth
    
    @Provides
    fun provideFirestore(): FirebaseFirestore
}
```

### Injection Points
- **Activities**: @AndroidEntryPoint
- **Fragments**: @AndroidEntryPoint
- **ViewModels**: @HiltViewModel
- **Workers**: @HiltWorker

## 🎨 UI Architecture

### Jetpack Compose Structure
```kotlin
@Composable
fun DashboardScreen(
    onNavigateToSettings: () -> Unit,
    onMenuClick: () -> Unit,
    viewModel: DashboardViewModel = hiltViewModel()
) {
    val uiState by viewModel.uiState.collectAsState()
    
    Scaffold(
        topBar = { DashboardTopBar() },
        content = { DashboardContent(uiState) }
    )
}
```

### UI Components
- **Screens**: Main app screens
- **Components**: Reusable UI components
- **ViewModels**: State management
- **Navigation**: Screen navigation

## 🔄 Background Processing

### WorkManager Architecture
```kotlin
// Worker Classes
class CampaignSyncWorker @AssistedInject constructor(
    @Assisted context: Context,
    @Assisted workerParams: WorkerParameters,
    private val campaignRepository: CampaignRepository
) : CoroutineWorker(context, workerParams)
```

### Service Architecture
```kotlin
// Background Services
class SmsService : Service {
    private val jobManager: JobManager by inject()
    
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // Handle SMS sending in background
    }
}
```

## 🔐 Security Architecture

### Authentication Flow
1. **Firebase Auth**: User authentication
2. **Token Management**: Secure token storage
3. **Permission Validation**: Runtime permissions
4. **Data Encryption**: Sensitive data protection

### Permission Management
```kotlin
// Required Permissions
val permissions = arrayOf(
    Manifest.permission.SEND_SMS,
    Manifest.permission.READ_PHONE_STATE,
    Manifest.permission.FOREGROUND_SERVICE,
    Manifest.permission.POST_NOTIFICATIONS // Android 13+
)
```

## 📊 Performance Optimization

### Database Optimization
- **Indexing**: Strategic database indexing
- **Query Optimization**: Efficient Room queries
- **Background Processing**: Async database operations
- **Caching**: Smart data caching

### UI Performance
- **Lazy Loading**: Efficient list rendering
- **State Hoisting**: Optimized state management
- **Compose Optimization**: Efficient recomposition
- **Memory Management**: Proper resource cleanup

## 🧪 Testing Architecture

### Testing Strategy
- **Unit Tests**: Business logic testing
- **Integration Tests**: Database and API testing
- **UI Tests**: Compose UI testing
- **WorkManager Tests**: Background task testing

### Test Structure
```kotlin
@RunWith(AndroidJUnit4::class)
class CampaignRepositoryTest {
    @get:Rule
    val instantExecutorRule = InstantTaskExecutorRule()
    
    private lateinit var database: AppDatabase
    private lateinit var campaignDao: CampaignDao
    private lateinit var repository: CampaignRepository
}
```

## 🔄 Data Synchronization

### Firebase Sync
- **Real-time Updates**: Firestore listeners
- **Offline Support**: Local-first architecture
- **Conflict Resolution**: Smart data merging
- **Background Sync**: Automatic synchronization

### Local-First Architecture
1. **Local Database**: Primary data source
2. **Background Sync**: Firebase synchronization
3. **Conflict Resolution**: Smart merging strategies
4. **Offline Support**: Full offline functionality

## 📱 Platform Integration

### Android Components
- **Activities**: Main app screens
- **Fragments**: Modular UI components
- **Services**: Background processing
- **Receivers**: System event handling
- **WorkManager**: Background task scheduling

### System Integration
- **SMS API**: Native SMS sending
- **Notification API**: User notifications
- **Alarm Manager**: Scheduled tasks
- **Boot Receiver**: Auto-start functionality

## 🚀 Scalability Considerations

### Architecture Scalability
- **Modular Design**: Easy feature addition
- **Clean Architecture**: Maintainable codebase
- **Dependency Injection**: Loose coupling
- **Reactive Programming**: Efficient data flow

### Performance Scalability
- **Database Optimization**: Efficient queries
- **Background Processing**: Non-blocking operations
- **Memory Management**: Resource optimization
- **UI Responsiveness**: Smooth user experience

---

This architecture provides a solid foundation for a scalable, maintainable, and performant SMS campaign management application. 