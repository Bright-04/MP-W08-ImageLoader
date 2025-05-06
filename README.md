# Image Loader Android Application

## Student Information
- **Name**: Nguyen Nhat Quang
- **ID**: 22110065
- **Course**: Mobile Programming (HCMUTE)

## Overview
This Android application demonstrates various approaches to loading images from URLs, handling network connectivity, and implementing background services. The app allows users to enter an image URL, load the image, and display it in an ImageView.

## Features

### 1. Image Loading with AsyncTask
- **Implementation**: The initial version uses AsyncTask to fetch images from a URL
- **Process**: 
  - A background thread downloads the image while showing a loading message
  - Upon completion, the image is displayed in the UI thread
  - Error handling shows appropriate messages for failed downloads

### 2. Improved Image Loading with AsyncTaskLoader
- **Implementation**: Refactored to use AsyncTaskLoader through the LoaderManager API
- **Benefits**:
  - Better handling of activity lifecycle
  - Automatic management of background tasks
  - Image persistence across configuration changes (e.g., screen rotation)
- **Usage**: LoaderManager is initialized in onCreate() and restarted when loading a new image

### 3. Network Connectivity Monitoring
- **Implementation**: BroadcastReceiver (NetworkReceiver) monitors network state changes
- **Features**:
  - Automatically disables the "Load Image" button when no internet connection is available
  - Updates status message based on connection type (Wi-Fi or other)
  - Re-enables functionality when connectivity is restored

### 4. Background Service with Notifications
- **Implementation**: ImageLoaderService runs as a foreground service
- **Features**:
  - Displays an initial notification when the service starts
  - Periodically (every 5 minutes) shows additional notifications
  - Uses NotificationChannel for Android 8.0+ compatibility
  - Service can be started when loading an image

### 5. Permission Handling
- **Required Permissions**:
  - `INTERNET`: For downloading images
  - `ACCESS_NETWORK_STATE`: For monitoring network connectivity
  - `POST_NOTIFICATIONS`: For Android 13+ notification support
  - `FOREGROUND_SERVICE`: For running the background service
  - `FOREGROUND_SERVICE_DATA_SYNC`: For specifying service type
- **Runtime Permission Handling**: Implementation for notification permissions on Android 13+

## User Interface
The app features a clean and responsive UI with:
- An EditText field to enter image URLs
- A "Load Image" button to initiate the download
- A status TextView showing the current state (loading, success, error, network status)
- An ImageView that displays the downloaded image
- Appropriate feedback during different states of operation

## Running the Application
1. Clone the repository
2. Open the project in Android Studio
3. Build and run on an emulator or physical device (minimum SDK 26)
4. Enter an image URL (e.g., `https://picsum.photos/500/500`)
5. Tap "Load Image" to download and display the image

## Technical Implementation

### AsyncTask Implementation
The original implementation used AsyncTask's three methods:
- `onPreExecute()`: Updates UI to show loading state
- `doInBackground()`: Performs network operation to download the image
- `onPostExecute()`: Updates UI with the image or error message

### AsyncTaskLoader Implementation
The loader pattern separates loading tasks from UI components:
- `onCreateLoader()`: Creates a new loader for the given URL
- `loadInBackground()`: Performs the actual image download
- `onLoadFinished()`: Updates UI when the loader completes
- `onLoaderReset()`: Handles cleanup when the loader is reset

### Network Receiver Implementation
The NetworkReceiver:
- Registers for `CONNECTIVITY_ACTION` broadcasts
- Uses `ConnectivityManager` to check connection status
- Adapts to different Android versions with appropriate APIs
- Communicates status changes through a callback function

### Service Implementation
The ImageLoaderService:
- Creates a notification channel for Android 8.0+
- Runs in the foreground to avoid system limitations
- Uses a Handler with postDelayed to schedule periodic notifications
- Properly handles lifecycle events (onCreate, onStartCommand, onDestroy)

## Dependencies
- AndroidX Core and AppCompat libraries
- AndroidX Lifecycle components
- RecyclerView (for potential list implementations)

## Notes
- The application demonstrates both traditional approaches (AsyncTask) and modern recommended patterns (AsyncTaskLoader)
- Code includes proper deprecation handling for backward compatibility
- Error handling is implemented throughout the application

## License
This project is licensed under the MIT License - see the LICENSE file for details.