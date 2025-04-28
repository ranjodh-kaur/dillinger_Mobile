# Unit 7. Android Security and Debugging
__________
##### Topics
- Requesting permissions
- Creating custom Permissions
- Securing application for publication and execution
- Tools for debugging
- Eclipse Java Editor: 
    - Java errors
    - Debugger
    - Logcat
    - Android Debug Bridge (adb)
- DDMS:
    - Dalvik Debug monitor service
    - Traceview
---
# Analogy
### 1. **Requesting Permissions**

Think of permissions like asking for a friend’s permission to use their car. Before your app can access sensitive features (like the camera, microphone, or location), it must first ask for permission from the user.

#### Example: Requesting Camera Permission

In your app’s `AndroidManifest.xml` file, declare that you want to use the camera:

```xml
<uses-permission android:name="android.permission.CAMERA"/>
```

Then, in your code, you’ll need to ask the user for permission:

```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CAMERA}, 1);
}
```

---

### 2. **Creating Custom Permissions**

Creating custom permissions is like making your own set of rules for who can enter specific rooms in your house. In Android, if you want only certain apps to access specific functions of your app, you can create custom permissions.

#### Example: Defining a Custom Permission

In your `AndroidManifest.xml` file:

```xml
<permission android:name="com.example.myapp.MY_CUSTOM_PERMISSION"
    android:protectionLevel="normal" />
```

Now, any other app that wants to use this feature in your app will need to declare this permission.

---

### 3. **Securing Application for Publication and Execution**

When you’re ready to publish your app, securing it is like locking your house before you leave. You want to ensure no one can tamper with it or misuse it.

- **Sign the app**: All Android apps must be digitally signed with a certificate to verify authenticity.
- **Use ProGuard**: This is a tool that shrinks, obfuscates (makes the code harder to read), and optimizes your code, making it more secure.

---

### 4. **Tools for Debugging**

Debugging tools help you identify and fix issues in your app, like checking why your car isn’t starting or running smoothly.

---

### 5. **Eclipse Java Editor**

The Eclipse Java Editor was an older, popular development tool for Android (before Android Studio).

- **Java Errors**: These are coding errors that Eclipse will highlight as you write your code.
- **Debugger**: Think of this as a mechanic's diagnostic tool that allows you to go through your code line by line, checking each part to see where it might be breaking.
- **Logcat**: This is like a real-time report that shows what’s happening in your app. It displays messages about errors, warnings, and other status updates.

---

### 6. **Android Debug Bridge (adb)**

Imagine adb as a remote control that lets you interact with your Android device from your computer. You can install/uninstall apps, view device logs, and even control the device remotely.

#### Example: Basic adb Command

To install an APK file on your connected device, you’d use:

```bash
adb install myApp.apk
```

---

### 7. **DDMS (Dalvik Debug Monitor Service)**

DDMS is like a full diagnostic tool for your app and your device. It allows you to check memory usage, track threads, and simulate calls or text messages.

- **Traceview**: Traceview is like a timeline that shows you how long each function in your app takes to run. This helps you identify slow parts of your code.

---
_____
**Detailed Topics:** Here’s an overview of each concept in Android Security and Debugging along with code examples for common tasks.



### 1. **Requesting Permissions**

In Android, you must request permission to access sensitive user data or device features (like location or camera). There are two types of permissions:

1. **Normal Permissions**: Granted automatically.
2. **Dangerous Permissions**: Requires explicit user approval.

#### Code Example: Requesting Camera Permission

First, add the permission in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA"/>
```

Then, request the permission at runtime (required for permissions like Camera from Android 6.0 and above):

```java
import android.Manifest;
import android.content.pm.PackageManager;
import android.os.Bundle;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private static final int CAMERA_PERMISSION_CODE = 100;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Check if permission is not granted
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
                != PackageManager.PERMISSION_GRANTED) {
            // Request permission
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.CAMERA}, CAMERA_PERMISSION_CODE);
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        if (requestCode == CAMERA_PERMISSION_CODE) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                Toast.makeText(this, "Camera Permission Granted", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(this, "Camera Permission Denied", Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

---

### 2. **Creating Custom Permissions**

If you want certain functions in your app to be accessible only by certain apps, you can create custom permissions.

#### Code Example: Defining a Custom Permission in `AndroidManifest.xml`

```xml
<permission android:name="com.example.myapp.CUSTOM_PERMISSION"
    android:protectionLevel="signature" />

<application ... >
    <activity
        android:name=".MyActivity"
        android:permission="com.example.myapp.CUSTOM_PERMISSION">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
</application>
```

Other applications wanting to access this feature must declare the same permission in their `AndroidManifest.xml`.

---

### 3. **Securing Application for Publication and Execution**

To secure an Android app:

1. **Sign the Application**: All Android apps must be digitally signed before they can be installed. You can generate a signing key in Android Studio.

2. **Use ProGuard**: This tool obfuscates code (making it harder to read) and removes unused code.

Enable ProGuard in `build.gradle`:

```gradle
buildTypes {
    release {
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
    }
}
```

**Securing an Android application for publication and execution** is critical to protect user data, prevent unauthorized access, and ensure that your app is trusted by users and platforms. Here’s a simplified guide on how to secure your app at each stage:

##### 1. **Requesting Necessary Permissions Only**

Limit permissions to only what’s essential for your app’s functionality, as excessive permissions can compromise security and make users wary.

- **Example**: If your app needs access to the device’s location, declare only the location permission in the `AndroidManifest.xml`:
    ```xml
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    ```

##### 2. **Using Secure Network Connections**

Always use HTTPS to encrypt network requests and protect data from being intercepted. Android 9 (Pie) and higher require HTTPS by default.

- **Example**: To enforce HTTPS, update your server to support HTTPS, and use APIs with secure connections.

##### 3. **Securing API Keys and Sensitive Data**

Avoid hard-coding API keys, passwords, or other sensitive information in your app code. Instead, consider storing them securely on your server or using Android's `EncryptedSharedPreferences` for secure, local storage.

##### 4. **Enabling ProGuard and R8 for Code Obfuscation**

Obfuscate your code using ProGuard or R8 to make it difficult for attackers to decompile and reverse-engineer your application.

- **How to Enable ProGuard**:
  - In `build.gradle`, enable ProGuard for release builds:
    ```gradle
    buildTypes {
        release {
            minifyEnabled true
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    ```
  - Customize ProGuard rules in `proguard-rules.pro` to specify which parts of your code to obfuscate and which to keep intact.

##### 5. **Signing the App for Publication**

Before publishing your app, you need to sign it with a secure signing key. This key verifies that the app comes from you and has not been tampered with.

- **Steps to Sign an APK in Android Studio**:
  - Go to **Build > Generate Signed Bundle/APK**.
  - Create or use an existing **key store** and **key**.
  - Select **release** build type for added security.
  - Keep your keystore secure, as you’ll need it for future updates.

##### 6. **Securing Data Storage with Encryption**

If your app stores sensitive user data, use encryption to protect it.

- **Use Encrypted SharedPreferences**: This stores data in a file that's encrypted with the Android keystore.
  
    ```java
    SharedPreferences encryptedPrefs = EncryptedSharedPreferences.create(
        "secure_prefs",
        MasterKeys.getOrCreate(MasterKeys.AES256_GCM_SPEC),
        context,
        EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
        EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
    );
    ```

##### 7. **Using Security Libraries**

Android offers built-in libraries for encryption, authentication, and integrity checks. For instance:
- **Jetpack Security** for encrypted storage.
- **Play Integrity API** to detect tampering and verify authenticity.

##### 8. **Testing for Vulnerabilities**

Before release, test your app for potential security vulnerabilities:
- **Use Android Lint**: Analyze your app with Android Studio’s Lint tool to catch security issues.
- **Third-Party Testing Tools**: Consider using tools like **OWASP ZAP** or **Burp Suite** to check for security issues.

---

##### 9. **Publishing in Google Play Console**

When publishing to Google Play:
- **Set up App Signing by Google Play**: Google securely manages your app’s signing key, ensuring app updates are from the original developer.
- **Use Play Integrity API**: Protect your app from unauthorized interactions with the Google Play Store.

### Summary Checklist for Securing Your App:
1. Request only necessary permissions.
2. Use HTTPS for all network communications.
3. Avoid hard-coding sensitive data; store securely.
4. Enable ProGuard/R8 for code obfuscation.
5. Sign your app with a secure key.
6. Encrypt sensitive data on the device.
7. Test for vulnerabilities.
8. Enable App Signing by Google Play.

Following these steps ensures your app is more secure and ready for publication.



---

### 4. **Tools for Debugging**

Android provides several debugging tools to identify and fix errors:

- **Java Errors**: The Android Studio editor highlights syntax errors as you type.
- **Debugger**: Allows you to step through code and inspect variables at each step.
- **Logcat**: Displays real-time logs of app operations, warnings, and errors.

##### Example: Adding Logcat Logs

```java
import android.util.Log;

public class MainActivity extends AppCompatActivity {
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.d(TAG, "App Started Successfully");
    }
}
```

In Android development, the `Log` class provides different log levels to categorize and control the flow of debugging information, making it easier to find relevant information in large sets of log output. Here’s a breakdown of each log level and when to use it:

##### 1. **Log.v (Verbose)**

- **Purpose**: Logs detailed information that may be helpful during debugging. Often used for method entry and exit points.
- **Use Case**: Typically used for logging comprehensive data that might be too much information for normal debugging but is useful for detailed troubleshooting.
  
**Example**:
```java
Log.v("MainActivity", "Verbose log: Detailed information here");
```

##### 2. **Log.d (Debug)**

- **Purpose**: Logs information useful for debugging the app during development. 
- **Use Case**: Typically used to log messages that help developers track application flow or inspect variables during development.
  
**Example**:
```java
Log.d("MainActivity", "Debug log: Variable X = " + x);
```

##### 3. **Log.i (Info)**

- **Purpose**: Logs general information about app operation, often related to successful actions.
- **Use Case**: Useful for logging high-level milestones, such as successful completion of certain tasks or initialization of key components.
  
**Example**:
```java
Log.i("MainActivity", "Info log: Successfully loaded data.");
```

##### 4. **Log.w (Warning)**

- **Purpose**: Logs potentially harmful situations that are not errors but may lead to future issues.
- **Use Case**: Used to highlight situations that could cause issues, like using deprecated methods or performance concerns.
  
**Example**:
```java
Log.w("MainActivity", "Warning log: Deprecated method used.");
```

##### 5. **Log.e (Error)**

- **Purpose**: Logs errors that have caused some functionality to fail, used for handling exceptions.
- **Use Case**: Used to capture exceptions and errors that may prevent certain parts of the app from functioning correctly.
  
**Example**:
```java
try {
    // some code that might throw an exception
} catch (Exception e) {
    Log.e("MainActivity", "Error log: Exception occurred", e);
}
```

---

### Putting It All Together

Here’s an example Android activity using each of these log levels for various parts of the code:

```java
public class MainActivity extends AppCompatActivity {
    private static final String TAG = "MainActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Log.v(TAG, "Verbose log: Entering onCreate");  // Verbose - entry point for method

        int x = 5;
        Log.d(TAG, "Debug log: Value of x = " + x);     // Debug - useful for checking variable values

        if (x > 0) {
            Log.i(TAG, "Info log: x is positive");     // Info - noting successful operation
        } else {
            Log.w(TAG, "Warning log: x is not positive"); // Warning - something worth noting
        }

        try {
            int result = 10 / 0;  // This will cause an exception
        } catch (ArithmeticException e) {
            Log.e(TAG, "Error log: Division by zero", e); // Error - logs the exception
        }

        Log.v(TAG, "Verbose log: Exiting onCreate");  // Verbose - exit point for method
    }
}
```

Using these log levels consistently helps keep your logs organized and makes debugging faster by allowing you to filter the logcat output based on log level.

---

### 5. **Android Debug Bridge (adb)**

`adb` is a command-line tool for interacting with Android devices or emulators from your computer.

**Example Commands**:
- **Install an app**: `adb install myApp.apk`
- **View device logs**: `adb logcat`
- **Uninstall an app**: `adb uninstall com.example.myapp`

The Android Debug Bridge (ADB) is a powerful command-line tool that allows developers to communicate with Android devices or emulators from a computer. It’s part of the Android SDK and is primarily used for debugging, testing, and performing various tasks directly on an Android device. 

### Key Features of ADB

1. **Installing and Uninstalling Apps**: ADB can quickly install or uninstall APKs, making it convenient for development and testing.

2. **Accessing Device File System**: ADB allows you to explore and transfer files between your computer and Android device.

3. **Debugging Apps**: ADB can be used to run debug commands, collect logs, and monitor device status.

4. **Running Shell Commands**: ADB provides a shell interface to execute commands on the Android device directly, giving you deeper control over it.

5. **System Monitoring**: ADB lets you capture screenshots, screen recordings, and even system logs (Logcat) for debugging purposes.

### Setting Up ADB

1. **Install Android SDK**: Make sure you have the Android SDK installed, which includes ADB.
2. **Enable Developer Options**: On your Android device, go to **Settings > About Phone > Build Number**, and tap it several times to enable developer options.
3. **Enable USB Debugging**: In **Developer Options**, enable **USB Debugging** to allow ADB to communicate with your device.

### Basic ADB Commands

- **Connect to the Device**: First, connect your device via USB and use the following command to ensure it’s connected.
  ```bash
  adb devices
  ```
  This command lists all connected devices. If you see your device’s serial number, it means ADB is connected.

- **Install an APK**:
  ```bash
  adb install path/to/app.apk
  ```
  This installs an APK on your connected device.

- **Uninstall an App**:
  ```bash
  adb uninstall com.example.app
  ```
  Replace `com.example.app` with the package name of the app you want to remove.

- **Transfer Files**:
  ```bash
  adb push local/path/to/file /sdcard/destination
  ```
  Use `adb push` to send files from your computer to the device, and `adb pull` to get files from the device.

- **Open a Shell on the Device**:
  ```bash
  adb shell
  ```
  This opens a command-line interface on the device, allowing you to run commands as if you were directly using the device.

- **Logcat (View Logs)**:
  ```bash
  adb logcat
  ```
  This command outputs system and app logs, useful for tracking down errors and understanding app behavior.

### Practical Example

Here’s a sequence of commands for debugging an issue in an app:

1. Connect the device via USB and confirm connection:
   ```bash
   adb devices
   ```

2. Launch the app directly with:
   ```bash
   adb shell am start -n com.example.app/.MainActivity
   ```

3. Capture logs for debugging:
   ```bash
   adb logcat > logs.txt
   ```

4. Analyze logs and track errors or warnings related to your app.

### Summary

ADB is an essential tool for Android development, providing powerful commands for app installation, file management, system monitoring, and debugging. Mastering ADB commands can make your Android development and debugging workflows faster and more effective.

---

### 6. **DDMS (Dalvik Debug Monitor Service)**

DDMS was a debugging tool in older Android versions, though now most of its functionality is integrated into Android Studio. It helped with tasks like monitoring memory usage and tracking threads.

- **Traceview**: A profiling tool to analyze your app’s performance. You can start profiling directly in Android Studio by selecting **Profiler** from the tools.

To use Traceview, add tracing code:

```java
import android.os.Debug;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Start tracing
        Debug.startMethodTracing("myTrace");

        // Run some code to trace
        myMethod();

        // Stop tracing
        Debug.stopMethodTracing();
    }

    private void myMethod() {
        // Code to trace
    }
}
```

Trace files can then be viewed using Android Studio's **Profiler** or `Traceview`.


The Dalvik Debug Monitor Service (DDMS) was an essential tool for Android developers, particularly before Android Studio integrated similar features. DDMS helped in debugging and monitoring various aspects of Android applications. Although DDMS has been largely replaced by **Android Studio’s Device Monitor** and **Android Profiler**, understanding its functions can still be valuable when debugging apps in different environments or for historical context.

### Key Features of DDMS

1. **Process Management**: DDMS lists all processes running on an Android device, allowing developers to select and debug a specific process.
2. **Logcat**: Provides real-time log output (system and app logs), useful for monitoring errors, warnings, or debugging information while the app runs.
3. **Thread and Heap Management**: DDMS provides insights into the current threads and heap usage of the application, helping to identify performance bottlenecks.
4. **Network Traffic Monitoring**: Tracks the network activity of your app, useful for debugging data usage and network-related issues.
5. **File Explorer**: Provides access to the device’s file system, enabling the examination and manipulation of app files on the device.
6. **Emulator Control**: Allows simulation of various device states and events, such as sending SMS messages, calls, or simulating location data.
7. **Screen Capture**: Captures the current screen of the Android device, useful for creating snapshots during debugging or testing.

### How to Access DDMS in Android Studio

While DDMS used to be a standalone tool in Eclipse, Android Studio has integrated its features:
1. **Logcat** can be accessed through **View > Tool Windows > Logcat**.
2. **Device File Explorer** is available through **View > Tool Windows > Device File Explorer**.
3. **Network Profiler, Memory Profiler, and CPU Profiler** can be accessed through **View > Tool Windows > Profiler**.

### Practical Use of DDMS Features in Android Studio

#### Example 1: Using Logcat for Debugging
Logcat is one of the most widely used tools in Android development. You can filter logs by tags, severity, or process IDs to focus only on relevant information.

- **Command Example**:
  ```bash
  adb logcat
  ```

#### Example 2: Examining Heap Usage and Threads
With DDMS, you can examine how much memory your app is using in real-time. Monitoring heap usage and checking thread activity helps identify memory leaks and excessive memory usage.

#### Example 3: Taking a Screen Capture
DDMS allows you to take screenshots of the app running on the device/emulator.

- **How to Do It**:
  - In Android Studio, go to **View > Tool Windows > Logcat**.
  - Click on the camera icon to capture the device screen.

### Summary of DDMS Tools

- **Process and Thread Management**: Observe all app processes and threads.
- **Logcat**: Track real-time logs and filter them to monitor app behavior.
- **Heap Management**: View memory usage to optimize app performance.
- **Network Traffic Monitoring**: Monitor data usage by the app.
- **File Explorer**: Access the app’s files on the device.
- **Screen Capture**: Take screenshots of the app for documentation or testing.

While DDMS itself is deprecated, Android Studio’s integrated tools offer similar capabilities, making it easier to debug and monitor apps comprehensively.

---

