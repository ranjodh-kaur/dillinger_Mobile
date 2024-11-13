# Unit 2. Android Development Environment
____
**Android Development Environment:**
1. Introduction to android
2. Advantage of Android over other development environment
3. Android execution environment
4. Components of android application
5. Android activity and service lifecycle
6. Android 7.0 nougat and comparison with older version
7. Assembling android 7 development workstation
8. Downloading and installing Android Studio2
9. Introduction to Android Studio IDE.
_____
#### **1. Introduction to Android**
**Analogy**: Android is like a platform or operating system that runs on phones, just like Windows runs on computers. It helps you run apps on mobile devices.

**Code**: To create an Android app, we need to build it using Android Studio and write code in Java or Kotlin. Here's a simple app example:

```java
package com.example.myfirstapp;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView textView = findViewById(R.id.textView);
        textView.setText("Hello, Android!");
    }
}
```

---

#### **2. Advantage of Android over Other Development Environments**
**Analogy**: Think of Android as a versatile tool, like a Swiss knife, that can work on many devices and brands. It is open-source, meaning anyone can use it and modify it for free, unlike iOS, which only works on Apple devices.

**Code**: You can use Android to develop apps for various devices, such as phones, tablets, watches, and TVs, using the same tools and languages.


| **Feature**                         | **Android**                                      | **Other Development Environments**                  |
|-------------------------------------|-------------------------------------------------|------------------------------------------------------|
| **Open Source**                     | Android is open-source, which means it is free to use, and its code can be modified. | Many other platforms (e.g., iOS) are closed-source and have licensing fees. |
| **Customization**                   | Android offers high customization of the user interface and device functionality. | Other platforms like iOS have limitations in terms of customization. |
| **Large User Base**                 | Android has a global user base, with over 2 billion active devices. | Other platforms, like iOS, have fewer global users. |
| **App Distribution**                | Android apps can be distributed through Google Play Store, and other third-party platforms. | Other platforms have more restricted app distribution options (e.g., iOS has only the Apple App Store). |
| **Variety of Devices**              | Android runs on a wide variety of devices (phones, tablets, wearables, etc.). | Other platforms are limited to fewer device types (e.g., iOS is restricted to Apple devices). |
| **Multi-Tasking**                   | Android supports multi-tasking and background processing well. | Other platforms may have limitations on multi-tasking (e.g., iOS used to have limitations). |
| **Cost of Development**             | Android development is cost-effective with no platform-specific costs for developers. | Developing for platforms like iOS may require purchasing expensive hardware and licenses. |
| **Flexible App Architecture**       | Android allows developers to choose from various architectures and libraries. | Other platforms may limit the architecture choices and libraries available. |
| **Development Tools**               | Android provides free and powerful development tools like Android Studio. | Other platforms may charge for their development environments (e.g., Xcode for iOS is free, but requires a macOS device). |
| **Programming Languages**           | Android supports Java, Kotlin, and C++. | Other platforms (e.g., iOS) use specific languages like Swift and Objective-C, which may have a steeper learning curve for new developers. |
| **Community Support**               | Android has a large developer community providing a wealth of resources, tutorials, and libraries. | While other platforms have support communities, Android's community is larger and more diverse. |
| **User Interface Design**           | Android offers flexibility in UI design with XML and different layout managers (e.g., RelativeLayout, ConstraintLayout). | Other platforms like iOS might have stricter design guidelines (e.g., iOS's use of SwiftUI). |

---

#### **3. Android Execution Environment**
**Analogy**: Android is like a car engine, and the Android runtime is the engine that powers all the apps. The apps run inside this engine, which helps them perform tasks like displaying text, clicking buttons, and making network calls.

**Code**: Android uses the Android Runtime (ART) to run apps. When you launch an app, it runs inside this environment, like how a car engine makes the wheels spin.

---

#### **4. Components of an Android Application**
**Analogy**: Building an Android app is like building a house. The walls (Activities), windows (Services), doors (Content Providers), and furniture (Broadcast Receivers) are the components that make the house (the app).

**Code**: The key components in an Android app are:
   - **Activity**: Represents a single screen.
   - **Service**: Runs in the background (like music playing).
   - **Content Provider**: Manages shared data.
   - **Broadcast Receiver**: Listens for system-wide events (like when the phone is charging).

Example of Activity:

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

---

### 1. **Activities**
   - **Purpose**: An **Activity** represents a single screen in an app. It's like a webpage in a website.
   - **Example**: The **Home Screen**, **Settings screen**, or **Login screen** are all examples of Activities.
   - **Life Cycle**: Activities have a specific life cycle, which means they can be created, paused, resumed, or destroyed based on the user's interactions.

   - **Example Code (Activity)**:
     ```java
     public class MainActivity extends AppCompatActivity {
         @Override
         protected void onCreate(Bundle savedInstanceState) {
             super.onCreate(savedInstanceState);
             setContentView(R.layout.activity_main); // Sets the layout for the activity
         }
     }
     ```

---

### 2. **Services**
   - **Purpose**: A **Service** is a component that runs in the background to perform long-running operations, such as downloading files or playing music, even if the user switches to another app.
   - **Example**: Playing music in the background or downloading files while the user is using other apps.
   - **Types of Services**:
     - **Foreground Service**: Runs with a higher priority and is visible to the user (e.g., music player).
     - **Background Service**: Runs silently in the background and does not need to interact with the user.

   - **Example Code (Service)**:
     ```java
     public class MyService extends Service {
         @Override
         public int onStartCommand(Intent intent, int flags, int startId) {
             // Service task goes here (e.g., downloading a file)
             return START_STICKY;
         }
         
         @Override
         public IBinder onBind(Intent intent) {
             return null; // Return null if service is not bound to any activity
         }
     }
     ```

---

### 3. **Broadcast Receivers**
   - **Purpose**: A **Broadcast Receiver** listens for global messages or events broadcast by the system or other applications, and responds accordingly. It acts like a listener for events such as battery low, Wi-Fi state change, or incoming SMS.
   - **Example**: Your app might listen for a change in network connectivity and react to it by updating the UI or showing a notification.
   
   - **Example Code (Broadcast Receiver)**:
     ```java
     public class MyReceiver extends BroadcastReceiver {
         @Override
         public void onReceive(Context context, Intent intent) {
             // Handle the event (e.g., battery low, screen on)
         }
     }
     ```

---

### 4. **Content Providers**
   - **Purpose**: A **Content Provider** manages shared data between different apps. It provides a way to access data stored in one app and use it in another. For example, accessing contacts or photos from the user's phone.
   - **Example**: Sharing contacts, calendar events, or media files with other applications.
   
   - **Example Code (Content Provider)**:
     ```java
     public class MyContentProvider extends ContentProvider {
         @Override
         public boolean onCreate() {
             // Initialize the content provider
             return true;
         }

         @Override
         public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
             // Return data from the provider (e.g., contacts)
             return null;
         }
         
         @Override
         public Uri insert(Uri uri, ContentValues values) {
             // Insert new data into the provider
             return null;
         }

         @Override
         public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
             // Update data in the provider
             return 0;
         }

         @Override
         public int delete(Uri uri, String selection, String[] selectionArgs) {
             // Delete data from the provider
             return 0;
         }

         @Override
         public String getType(Uri uri) {
             return null;
         }
     }
     ```

---

### 5. **Intents**
   - **Purpose**: An **Intent** is used to start activities, services, or broadcast receivers. It can also carry data between components, like sending a message or sharing a file.
   - **Example**: You can use an Intent to start a new Activity (screen) or send a message to a service.

   - **Example Code (Intent)**:
     ```java
     // Starting an Activity
     Intent intent = new Intent(this, SecondActivity.class);
     startActivity(intent);

     // Sending data through Intent
     intent.putExtra("message", "Hello from MainActivity");
     ```

---

### 6. **Fragments**
   - **Purpose**: A **Fragment** represents a part of an Activity, like a modular section of a screen that can be reused. It’s used to create flexible UIs, especially for tablets or multi-pane layouts.
   - **Example**: A fragment could be a navigation menu or a section showing a list of items within a larger screen.
   
   - **Example Code (Fragment)**:
     ```java
     public class MyFragment extends Fragment {
         @Override
         public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
             return inflater.inflate(R.layout.fragment_layout, container, false);
         }
     }
     ```

---

### 7. **Android Manifest**
   - **Purpose**: The **Manifest** file (`AndroidManifest.xml`) is like the blueprint for the app. It defines the structure of the app, including which components (Activities, Services, etc.) are available, the permissions the app needs, and other essential configurations.
   - **Example**: If your app needs access to the internet, you declare it in the manifest file.

   - **Example Code (Manifest)**:
     ```xml
     <manifest xmlns:android="http://schemas.android.com/apk/res/android"
         package="com.example.myapp">

         <application
             android:allowBackup="true"
             android:icon="@mipmap/ic_launcher"
             android:label="@string/app_name"
             android:theme="@style/AppTheme">
             
             <activity android:name=".MainActivity">
                 <intent-filter>
                     <action android:name="android.intent.action.MAIN" />
                     <category android:name="android.intent.category.LAUNCHER" />
                 </intent-filter>
             </activity>
             
             <service android:name=".MyService" />
         </application>
     </manifest>
     ```

---

### **Summary of Android Components:**

- **Activities**: Represent individual screens or user interfaces.
- **Services**: Run in the background to perform tasks like downloading or playing music.
- **Broadcast Receivers**: Listen for system-wide events and respond to them.
- **Content Providers**: Share data between different apps.
- **Intents**: Trigger actions, like starting activities or sending messages.
- **Fragments**: Modular sections of UI within an activity, used to create flexible layouts.
- **Manifest**: Describes the app's components and permissions.

These components come together to create a fully functional Android app that can interact with users, other apps, and the system.

---

#### **5. Android Activity and Service Lifecycle**
**Analogy**: Think of an Activity as a room in your house and the Activity Lifecycle as how you use that room: entering, using it, leaving, and then cleaning it up. A Service is like a background helper working in the kitchen even if you're not in the room.

**Code**:
   - **Activity Lifecycle** example (onCreate, onStart, onPause, etc.):

```java
@Override
protected void onStart() {
    super.onStart();
    Log.d("Lifecycle", "Activity is starting!");
}
```

   - **Service Lifecycle** example (onStartCommand, onDestroy):

```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    Log.d("Service", "Service Started");
    return START_STICKY;
}

@Override
public void onDestroy() {
    super.onDestroy();
    Log.d("Service", "Service Destroyed");
}
```

---

#### **6. Android 7.0 Nougat and Comparison with Older Versions**
**Analogy**: Android 7.0 Nougat is like a new version of a video game with better graphics and new features, such as split-screen multitasking, better battery management, and improved security, compared to older versions like Marshmallow.

**Key Features**:
   - **Split-Screen Mode**: Run two apps at the same time.
   - **Doze Mode**: Helps save battery when the phone is idle.
   - **Improved Notifications**: More control over how notifications are displayed.



**Android 7.0 Nougat** was a significant update to the Android operating system, offering improvements in multitasking, battery life, notifications, and overall performance. Here's a comparison of **Android 7.0 Nougat** with its predecessor **Android 6.0 Marshmallow**:

---

### **Key Features of Android 7.0 Nougat**

1. **Multitasking with Split-Screen Mode**:
   - **Nougat** introduced a **split-screen mode**, allowing users to run two apps at the same time on the same screen. This is particularly useful for tasks like watching a video while browsing the web.
   - **Example**: You can have your email app open on the top half of the screen and YouTube playing on the bottom half.

2. **Improved Battery Life (Doze on the Go)**:
   - **Nougat** enhanced the **Doze mode** (introduced in Marshmallow) to help conserve battery life. While **Doze** mode was available in Marshmallow when the device was stationary, **Nougat** expanded it to work even when the device is being carried around, reducing background activity and saving power.
   - **Example**: When you're not actively using your phone, apps use fewer resources and don’t drain your battery as much.

3. **Direct Reply for Notifications**:
   - **Nougat** introduced the ability to **reply directly from the notification** without having to open the app. This feature is particularly useful for replying to text messages or chats without leaving your current app.
   - **Example**: You can reply to a WhatsApp message from the notification bar without opening the app.

4. **Custom Quick Settings**:
   - **Nougat** allowed users to **customize the quick settings menu** to rearrange or add shortcuts for frequently used settings like Wi-Fi, Bluetooth, and Do Not Disturb.
   - **Example**: You can add a **screenshot shortcut** or **battery saver toggle** directly in the quick settings menu.

5. **Multi-User Support and Seamless Switching**:
   - **Nougat** improved **multi-user** and **guest mode** support, making it easier to switch between multiple accounts.
   - **Example**: If you share your phone with someone, you can quickly switch to their user profile without logging out of your own.

6. **Improved Graphics (Vulkan API)**:
   - **Nougat** introduced the **Vulkan API**, providing developers with better control over graphics rendering. This meant smoother, more detailed 3D graphics in apps and games.
   - **Example**: Games with heavy 3D graphics, like **Monument Valley**, will run smoother on Nougat.

---

### **Comparison with Android 6.0 Marshmallow**

| Feature                          | **Android 7.0 Nougat**                         | **Android 6.0 Marshmallow**                    |
|-----------------------------------|-----------------------------------------------|-----------------------------------------------|
| **Split-Screen Multitasking**     | Split-screen for running two apps side-by-side | Not available                                 |
| **Battery Life (Doze Mode)**     | Improved Doze mode, works while on the move    | Doze mode only works when the device is stationary |
| **Direct Reply Notifications**   | Reply directly from notifications             | Notifications only show basic information     |
| **Quick Settings Customization** | Customize and add shortcuts                   | No customization available                    |
| **Multi-User Support**           | Enhanced support for multiple users and guest mode | Basic multi-user support, not as seamless     |
| **Graphics (Vulkan API)**        | Vulkan API for improved graphics performance   | No Vulkan support                             |

---

### **Other Notable Features in Android 7.0 Nougat:**

- **Seamless System Updates**: Nougat allows updates to be installed in the background without interrupting the user experience, reducing downtime during updates.
- **File-based Encryption**: Improved security with file-based encryption, which encrypts individual files rather than the entire device, offering better performance and flexibility.
- **Java 8 Support**: Android 7.0 Nougat supports Java 8 features, allowing developers to use lambdas and other modern Java features.
- **Enhanced Notifications**: Nougat improved how notifications appear, allowing bundled notifications for easier viewing and management.
---

#### **7. Assembling Android 7 Development Workstation**
**Analogy**: Setting up a development workstation is like building your workbench. You need tools like Android Studio, a computer, and Java or Kotlin knowledge to build your app.

**Steps**:
   - Install Android Studio, which includes everything you need to develop Android apps.
   - Ensure your computer has the latest Java Development Kit (JDK).

---

#### **8. Downloading and Installing Android Studio**
**Analogy**: Downloading Android Studio is like getting a complete toolbox for building apps. You install it on your computer, and it gives you all the tools (like a code editor, emulator, and debugger) you need to develop.

**Steps**:
1. Go to the [Android Studio website](https://developer.android.com/studio).
2. Download the installer for your operating system.
3. Run the installer and follow the setup instructions.

---

#### **9. Introduction to Android Studio IDE**
**Analogy**: Android Studio is like your workshop, where you’ll be doing all the work. It’s where you write code, design your app's UI, and test your app.

**Features**:
   - **Code Editor**: Where you write your Java/Kotlin code.
   - **Layout Editor**: Where you design the app’s UI.
   - **Emulator**: Test your app on a virtual phone.
   - **Debugging Tools**: Find and fix bugs in your app.

---

### Example of Code for Android Studio Setup

**1. Setting up a simple app in Android Studio**:
```java
// MainActivity.java
package com.example.simpleapp;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        TextView textView = findViewById(R.id.textView);
        textView.setText("Welcome to Android Development!");
    }
}
```

**2. Simple XML layout for the app**:
```xml
<!-- activity_main.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:id="@+id/textView"
        android:text="Hello, World!"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout>
```
