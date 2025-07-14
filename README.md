# SMS Sender App 📱

A comprehensive Android application for managing SMS marketing campaigns with advanced automation, analytics, and multi-workspace collaboration features.
![Dashboard](https://github.com/user-attachments/assets/efea1af8-255b-4c23-9f68-50d8140b2967)

## 🎯 Project Overview

The SMS Sender App is a sophisticated Android application designed for businesses and marketers who need to manage large-scale SMS campaigns. It provides a complete solution for creating, scheduling, and monitoring SMS marketing campaigns with real-time analytics and automation capabilities.

## ✨ Key Features

### 📊 Campaign Management
- **Campaign Creation**: Create and manage multiple SMS campaigns
- **Scheduling**: Set up automated sending schedules with delays
- **Status Tracking**: Monitor campaign progress (Draft, Active, Paused, Completed)
- **Daily Limits**: Configure daily message limits to comply with regulations
- **Contact Management**: Import and organize contact lists

### 📈 Analytics & Dashboard
- **Real-time Metrics**: Track messages sent, contacts reached, and campaign performance
- **Daily Statistics**: Monitor daily message counts and remaining limits
- **Activity Log**: View detailed activity history and campaign events
- **Progress Tracking**: Visual progress indicators for ongoing campaigns

### 🔐 Authentication & Security
- **Firebase Authentication**: Secure user login with email/password
- **Multi-workspace Support**: Collaborate with team members across workspaces
- **Data Encryption**: Secure storage of sensitive campaign data
- **Permission Management**: Granular control over SMS and notification permissions

### 🤖 Automation & Background Processing
- **Background SMS Sending**: Automated message delivery in the background
- **Scheduled Campaigns**: Set up campaigns to run at specific times
- **Auto-resume**: Automatic campaign resumption after device restarts
- **Error Handling**: Robust error handling and retry mechanisms

### 🎨 User Interface
- **Modern UI**: Clean, intuitive interface with Material Design 3
- **Dark Mode**: Full dark mode support for better user experience
- **Responsive Design**: Optimized for various screen sizes
- **Jetpack Compose**: Modern declarative UI framework

## 🏗️ Technical Architecture

### Architecture Pattern
- **MVVM (Model-View-ViewModel)**: Clean separation of concerns
- **Repository Pattern**: Centralized data access layer
- **Dependency Injection**: Hilt for dependency management
- **Reactive Programming**: Kotlin Coroutines and Flow for async operations

### Key Technologies
- **Android SDK**: Native Android development
- **Kotlin**: Modern programming language
- **Jetpack Compose**: Modern UI toolkit
- **Room Database**: Local data persistence
- **Firebase**: Backend services (Auth, Firestore)
- **WorkManager**: Background task scheduling
- **Hilt**: Dependency injection

### Data Layer
```
📁 Data Structure
├── Campaigns (Campaign management)
├── Contacts (Contact lists and phone numbers)
├── Campaign Lists (Organized contact groups)
├── Dashboard Metrics (Analytics data)
└── Settings (User preferences)
```

## 📱 App Screens & Navigation

### Main Navigation
1. **Dashboard**: Overview of campaigns and metrics
2. **Campaigns**: Create and manage SMS campaigns
3. **Contact Lists**: Organize and import contacts
4. **Settings**: App configuration and user preferences

### Key Screens
- **Dashboard Screen**: Real-time metrics and active campaigns
- **Campaign Editor**: Create and edit campaign details
- **Campaign Details**: Detailed view with progress tracking
- **Contact Lists**: Manage contact groups and imports
- **Settings**: User preferences and app configuration

## 🔧 Development Setup

### Prerequisites
- Android Studio Hedgehog (2023.1.1) or newer
- JDK 17 or newer
- Android SDK 26 or newer
- Firebase project setup

### Build Variants
- **composeDebug**: Jetpack Compose UI version
- **originalDebug**: XML-based UI version
- **release**: Production builds

### Firebase Configuration
1. Create Firebase project
2. Add Android app with package `com.example.smssenderapp`
3. Download `google-services.json`
4. Enable Authentication and Firestore services
5. Configure security rules

## 📊 Database Schema

### Core Entities
- **Campaign**: Campaign metadata and status
- **Contact**: Individual contact information
- **CampaignList**: Organized contact groups
- **DashboardMetrics**: Analytics and statistics
- **Settings**: User preferences and configuration

### Relationships
- Campaigns have multiple Contacts
- CampaignLists contain multiple Contacts
- DashboardMetrics track daily statistics
- Settings store user preferences

## 🚀 Key Features in Detail

### Campaign Management
- Create campaigns with custom names and descriptions
- Set daily message limits for compliance
- Track campaign status (Draft, Active, Paused, Completed)
- Monitor progress with real-time metrics

### Contact Management
- Import contacts from various sources
- Organize contacts into lists
- Validate phone numbers
- Handle duplicate contacts

### Automation
- Background SMS sending service
- Scheduled campaign execution
- Auto-resume after device restarts
- Error handling and retry logic

### Analytics
- Real-time dashboard metrics
- Daily message count tracking
- Campaign performance analytics
- Activity logging and history

## 🔒 Security & Permissions

### Required Permissions
- `SEND_SMS`: Send SMS messages
- `READ_PHONE_STATE`: Access device information
- `FOREGROUND_SERVICE`: Background processing
- `POST_NOTIFICATIONS`: Display notifications (Android 13+)

### Security Features
- Firebase Authentication
- Encrypted local storage
- Secure API communication
- Permission validation

## 📈 Performance & Optimization

### Performance Features
- Efficient database queries with Room
- Background processing with WorkManager
- Optimized UI rendering with Compose
- Memory-efficient contact handling

### Scalability
- Support for large contact lists
- Efficient campaign processing
- Background task management
- Optimized data synchronization

## 🛠️ Development Guidelines

### Code Quality
- Clean architecture principles
- Comprehensive error handling
- Extensive logging for debugging
- Unit tests for critical components

### UI/UX Standards
- Material Design 3 guidelines
- Accessibility compliance
- Responsive design patterns
- Intuitive navigation flow

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

This is a private project showcasing advanced Android development techniques and SMS campaign management capabilities.

---

**Note**: This app demonstrates advanced Android development concepts including background services, database management, Firebase integration, and modern UI development with Jetpack Compose. 
