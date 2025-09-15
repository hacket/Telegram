# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the official open-source Android application for Telegram messenger, written primarily in Java with native C/C++ components. The project uses Android Gradle Plugin with multiple modules for different distribution channels.

## Architecture

### Multi-Module Structure
- **TMessagesProj**: Core library module containing all shared functionality
- **TMessagesProj_App**: Main Google Play Store application variant
- **TMessagesProj_AppHuawei**: Huawei AppGallery variant with HMS services
- **TMessagesProj_AppHockeyApp**: Internal testing/beta distribution variant
- **TMessagesProj_AppStandalone**: Standalone APK variant without Google services
- **TMessagesProj_AppTests**: Test module

### Core Components (in TMessagesProj/src/main/java/org/telegram/messenger/)
- **MessagesController**: Main controller handling message operations and server communication
- **MessagesStorage**: Database layer using SQLite for local message/data storage
- **MediaController**: Handles media playback, recording, and processing
- **NotificationsController**: Push notifications and system notification management
- **FileLoader/DownloadController**: File download/upload operations and caching
- **ContactsController**: Contact synchronization and management
- **LocationController**: GPS and location sharing functionality
- **voip/**: Voice/video calling implementation
- **ApplicationLoader**: Main application entry point and initialization

### Key Configuration
- **BuildVars.java**: Contains API keys, feature flags, and build configuration
- **SharedConfig**: Application settings and user preferences storage
- **LocaleController**: Internationalization and language management

## Development Commands

### Building
```bash
# Build debug APK for main app
./gradlew TMessagesProj_App:assembleDebug

# Build release APK for main app
./gradlew TMessagesProj_App:assembleRelease

# Build all variants
./gradlew assembleDebug
./gradlew assembleRelease

# Build specific variant (Huawei, Standalone, etc.)
./gradlew TMessagesProj_AppHuawei:assembleDebug
./gradlew TMessagesProj_AppStandalone:assembleDebug
```

### Installation and Testing
```bash
# Install debug APK to connected device
./gradlew TMessagesProj_App:installDebug

# Run lint checks
./gradlew lint

# Clean build artifacts
./gradlew clean
```

### Native Components
Native code is located in `TMessagesProj/jni/` and requires Android NDK for compilation. Native libraries are built automatically during the Gradle build process.

## Important Setup Requirements

Before building:
1. Obtain your own API credentials from https://core.telegram.org/api/obtaining_api_id
2. Replace dummy values in `TMessagesProj/src/main/java/org/telegram/messenger/BuildVars.java`
3. Configure Firebase services by placing `google-services.json` in the project root
4. Set up signing configuration in `gradle.properties` with your keystore details
5. For Huawei variant, configure HMS services

## Key Files to Understand

- `settings.gradle`: Defines all project modules
- `gradle.properties`: Build configuration and version information
- `TMessagesProj/build.gradle`: Core library dependencies and build logic
- `TMessagesProj_App/build.gradle`: Main app module configuration
- `TMessagesProj/src/main/java/org/telegram/messenger/BuildVars.java`: API configuration

## Testing

Tests are located in the `TMessagesProj_AppTests` module. The project primarily uses integration testing due to the complexity of the messaging functionality.

## Language and Conventions

- Primary language: Java (Android SDK level)
- Native components: C/C++ with JNI bindings
- Build system: Gradle with Android Gradle Plugin
- Minimum Android version: SDK 16 (Android 4.1)
- Target Android version: SDK 35 (Android 15)
- Uses AndroidX libraries and follows modern Android development practices