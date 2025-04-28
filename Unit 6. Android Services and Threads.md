# Unit 6. Android Services and Threads

**Topics:**
   - Android service class:
      - Controlling services
      - Spawning process
      - Process Life Cycle
      - Thread Caveats
      - Background Processing Services
_______


### Android Services and Threads Analogy

1. **Android Service Class**: Think of an Android Service as a dedicated coffee machine in your shop that can keep making coffee even when you're not watching it. It can keep running in the background, doing tasks without needing to display anything on screen.
   
    **What is a Service?**
      - An **Android Service** is like a **background worker**.
      - It **runs in the background** without a user interface (no screen).
      - It can keep doing tasks **even when you switch apps** or **lock the phone**.

   **Basic Example:**   A Service might:

      - Play music in the background
      - Download a file
      - Upload something to a server
      - Track your location for a GPS app
      - check messages
      - Handling background tasks like cleaning or syncing data
      - Sending/Receiving data over the network


  
       When you start the Service, it will show:
   
         - ➡️ "Download Started..." (Toast)

       After 5 seconds, it will show:
   
         - ➡️ "Download Complete!" (Toast)

       Then it will automatically stop itself and show:
   
         - ➡️ "Service Destroyed" (Toast)

3. **Controlling Services**: Controlling services is like turning the coffee machine on or off. You decide when it starts (makes coffee) or stops (takes a break) to manage how much coffee it brews.

   Imagine you have a coffee machine in your shop.
   - You turn it ON when you have customers coming.
   - You turn it OFF when there are no orders, to save energy.

   In Android, controlling a service means:
   - Starting a service when you need it (start making coffee)
   - Stopping a service when the job is done (stop the coffee machine)

4. **Spawning Process**: Imagine each coffee order starts a separate process (task) in the coffee machine. Each time someone orders coffee, the machine starts a process to handle that order until it's done.

      In the context of an Android Service, "spawning a process" means creating a new task or thread to handle a specific operation, similar to how a coffee machine handles each order by starting a new task for every customer. Each new order (task) runs independently from the others, ensuring that the process can complete without interrupting or affecting others.

      Let’s break it down:

      1. **The Customer Orders Coffee (Initiating a Task)**: Every time a customer walks in and places an order, the coffee machine starts a new brewing process specifically for that order. This task operates separately from other brewing processes.

      2. **Brewing Process (Spawning a Process)**: The coffee machine creates a separate "brewing process" for each order. While one order is brewing, other orders can still be handled simultaneously, without interference.

      3. **Completion (Task Finishing)**: When the coffee is brewed, the process for that order is complete, and the machine is free to handle the next order.

   **In Android Service:**

   When you spawn a process in Android, it’s like creating a separate **task** (thread or process) to handle a specific job that may take a while, such as downloading a file, playing music, or fetching data. The new process allows the app to continue running smoothly without being blocked by long-running operations.



5. **Process Life Cycle**: Every coffee order has a life cycle:
   - **Start**: The order is received and begins.
   - **Running**: The coffee is being made.
   - **Pause or Cancel**: If something interrupts the coffee order (like the machine overheating), it stops temporarily or cancels.
   - **Stop**: Once the order is fulfilled or canceled, the process ends.

6. **Thread Caveats**: In a coffee shop, having one person make all the coffees one at a time would be slow. Threads allow multiple baristas (workers) to work at the same time, so the coffee shop (app) doesn’t slow down. But if two baristas use the same machine without coordinating, it might break! So, **thread management** is like making sure baristas don’t interfere with each other while working.

7. **Background Processing Services**: These are like batch orders that happen when the shop is quieter, like preparing snacks during off-hours. Background processing services handle tasks behind the scenes, so the main customer service (UI) stays free to serve customers without delay.

   Imagine a busy coffee shop:
   - while customers are coming and going, you don't want your main barista (the UI) getting stuck preparing large snack orders
   - Instead, you assign another chef in the back kitchen (the background service) to prepare those snacks during quieter hours.  

This way:  
- Customers get quick service at the counter (UI stays fast ✅)  
- Big tasks are done behind the scenes (Background Service ✅)
---


| Topic | Key Point |
|:---|:---|
|**Service Class**| Base class to handle background tasks.|
| Controlling Services | Start, stop, bind services |
| Spawning Process | Services can run in a different process |
| Process Life Cycle | Services can be killed if memory is low |
| Thread Caveats | Always use a background thread in Services |
| Background Processing Services | Use WorkManager or JobScheduler for safe background tasks |
---
  
   **Basic Java Code for a Service**

```java
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import androidx.annotation.Nullable;

public class MyService extends Service {
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null; // We don't bind in this simple example
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // Your background task code here
        return START_STICKY; // Tells system to restart service if killed
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        // Code to clean up when service is destroyed
    }
}
```

**How to Start a Service**
From your Activity:

```java
Intent serviceIntent = new Intent(this, MyService.class);
startService(serviceIntent);
```


# Example: Basic Music Playing Service

### Step 1: Create a new Service class:  
**`MyMusicService.java`**

```java
package com.example.myserviceapp;

import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.widget.Toast;
import androidx.annotation.Nullable;

public class MyMusicService extends Service {

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        // We won't bind this service
        return null;
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // Show a Toast when service starts
        Toast.makeText(this, "🎵 Music Service Started", Toast.LENGTH_SHORT).show();

        // You can add code here to start playing music

        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        // Show a Toast when service stops
        Toast.makeText(this, "🛑 Music Service Stopped", Toast.LENGTH_SHORT).show();

        // Stop music if playing
    }
}
```

---

### Step 2: Register the Service in **AndroidManifest.xml**

Add this inside the `<application>` tag:

```xml
<service android:name=".MyMusicService" />
```

---

### Step 3: Start and Stop the Service from your Activity

Example code inside your `MainActivity.java`:

```java
package com.example.myserviceapp;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import androidx.appcompat.app.AppCompatActivity;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    Button startButton, stopButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        startButton = findViewById(R.id.startServiceButton);
        stopButton = findViewById(R.id.stopServiceButton);

        startButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent serviceIntent = new Intent(MainActivity.this, MyMusicService.class);
                startService(serviceIntent);
            }
        });

        stopButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent serviceIntent = new Intent(MainActivity.this, MyMusicService.class);
                stopService(serviceIntent);
            }
        });
    }
}
```

---

### Step 4: Design your `activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="20dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/startServiceButton"
        android:text="Start Music Service 🎵"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"/>

    <Button
        android:id="@+id/stopServiceButton"
        android:text="Stop Music Service 🛑"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:layout_marginTop="20dp"/>

</LinearLayout>
```


---

**To add MP3 file instead of toast message**

---

## Step 1: Add an MP3 File

- Copy an MP3 file (e.g., `sample_music.mp3`) into your app’s **`res/raw/`** folder.  
  (If `raw` folder doesn’t exist, create it inside `res`.)

👉 Example location:  
`app/src/main/res/raw/sample_music.mp3`

---

## Step 2: Update the Service Code

**`MyMusicService.java`**

```java
package com.example.myserviceapp;

import android.app.Service;
import android.content.Intent;
import android.media.MediaPlayer;
import android.os.IBinder;
import android.widget.Toast;
import androidx.annotation.Nullable;

public class MyMusicService extends Service {

    private MediaPlayer mediaPlayer;

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null; // No binding needed
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Toast.makeText(this, "🎵 Music Service Started", Toast.LENGTH_SHORT).show();

        if (mediaPlayer == null) {
            // Create the MediaPlayer with your music file
            mediaPlayer = MediaPlayer.create(this, R.raw.sample_music);
            mediaPlayer.setLooping(true); // Repeat music
        }

        mediaPlayer.start(); // Start playing
        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        if (mediaPlayer != null) {
            mediaPlayer.stop();
            mediaPlayer.release(); // Free the resources
            mediaPlayer = null;
        }
        Toast.makeText(this, "🛑 Music Service Stopped", Toast.LENGTH_SHORT).show();
    }
}
```

---
**create a MediaPlayer** linked to your MP3 file.
- **Start** the music when Service starts.
- **Stop and release** the music when Service stops.

---

## Step 3: MainActivity Code (Same as before)

`MainActivity.java` already contains two buttons:  
- **Start Music Service** 🎵  
- **Stop Music Service** 🛑

No change needed.

---
## Step 4: Manifest Reminder

Make sure the Service is registered:

```xml
<service android:name=".MyMusicService" />
```

---


You can change  
```java
mediaPlayer.setLooping(true);
```  
to  
```java
mediaPlayer.setLooping(false);
```  
if you **don't** want the music to repeat.

---
____

### 1. **Android Service Class**

The Android `Service` class is a component that runs in the background to perform long-running tasks. A service does not have a user interface and can continue running even if the user switches to another app. 

**What is a Service?**
- A **Service** is a part of an Android app that **runs operations in the background**.
- It **does not** have a **User Interface (UI)**.
- Example: Play music, download files, check messages, etc., even if the app is closed!

- **Controlling Services**: We can start a service using `startService()` or `bindService()`.

```java
// Starting a service
Intent intent = new Intent(this, MyService.class);
startService(intent); // Starts the service
```

- **Stopping a Service**: You can stop the service manually or let it stop itself after finishing its task.

```java
stopService(intent); // Stops the service manually
```


**Controlling Services**

You control a Service in three main ways:

| Action | Method Used |
|:---|:---|
| Start a Service | `startService(Intent intent)` |
| Stop a Service | `stopService(Intent intent)` |
| Connect (Bind) to a Service | `bindService(Intent intent, ServiceConnection conn, int flags)` |

🔵 **Started Service**:  
- Launched by calling `startService()`.  
- Runs independently until stopped.

🔵 **Bound Service**:  
- Other apps or components can connect to it using `bindService()`.
- Dies when no one is connected.

---

### 2. **Spawning Process and Process Lifecycle**

When you start a service, it typically runs on the **main UI thread** by default, which can lead to issues if the service performs heavy tasks. To prevent blocking the main thread, Android allows you to create separate threads or processes within a service. 

To run tasks independently, create a new thread within the service.

**Example: Running a Service in a Separate Thread**

```java
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.util.Log;

public class MyService extends Service {
    private Thread myThread;

    @Override
    public void onCreate() {
        super.onCreate();
        // Create a separate thread to run background tasks
        myThread = new Thread(new Runnable() {
            @Override
            public void run() {
                performBackgroundTask();
            }
        });
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        myThread.start(); // Start the thread when the service starts
        return START_STICKY; // Service will restart if it’s killed
    }

    private void performBackgroundTask() {
        // Task logic here
        Log.d("MyService", "Performing background task");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        myThread.interrupt(); // Stop the thread when the service is destroyed
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}
```

**Spawning Process**

When you run an Android app:

- **By default**, your app and its Service **run in the same process** (same memory space).

**Spawning Process** means:
- You can make your Service **run in a different process** (separate from your main app).
- Useful if Service is heavy (e.g., downloading big files) — it won’t slow down the app UI.

🔵 How to spawn a separate process?
In `AndroidManifest.xml`:
```xml
<service
    android:name=".MyService"
    android:process=":remote" />
```
👉 This `:remote` tells Android: "Run this Service in a new process!"

---

### 3. **Process Lifecycle**

The lifecycle of a service is managed by the Android system. There are several important methods:

- **`onCreate()`**: Called when the service is first created.
- **`onStartCommand()`**: Called each time a client starts the service using `startService()`.
- **`onDestroy()`**: Called when the service is no longer in use and is being destroyed.

Here’s how Android manages the life of your Service and app:

| Priority | Process | Example |
|:---|:---|:---|
| 1 | Foreground | User interacting with an Activity |
| 2 | Visible | Activity not in front but still visible |
| 3 | Service | A background Service doing work |
| 4 | Cached | Apps/services kept in memory for quick reopening |

💥 **Important**:
- If memory gets low, Android may **kill Services** (lower priority) to **save space**.
- Foreground services (like music playing) are **less likely to be killed**.

---



### 4. **Thread Caveats**

When working with threads, remember:

- Avoid creating too many threads, as this can lead to memory issues.
- Use a **HandlerThread** or **AsyncTask** if the tasks are short-lived.
- Use **IntentService** for simpler tasks, as it automatically manages background work in a separate thread and stops when done.

**Example with IntentService**

```java
import android.app.IntentService;
import android.content.Intent;
import android.util.Log;

public class MyIntentService extends IntentService {

    public MyIntentService() {
        super("MyIntentService");
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        // Code to run in the background
        Log.d("MyIntentService", "Running background task in IntentService");
    }
}
```


**Thread Caveats (Important!)**

⚡ **BIG WARNING**: Services **run on the Main Thread** (UI thread) by default!

👉 This means:
- If you do heavy work inside a Service (like downloading files),
- It can **freeze your app** ❌.

🔵 Solution:
- Always **create a separate thread** inside your Service for heavy tasks!

Example:
```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    new Thread(new Runnable() {
        @Override
        public void run() {
            // Do heavy task here
        }
    }).start();
    return START_STICKY;
}
```

---

### 5. **Background Processing Services**


**Background Processing Services** allow apps to perform heavy tasks **without blocking the User Interface (UI)**.  
They work *quietly in the background* even when the user is doing something else or when the app is minimized.

---

# 🛠️ **Typical Background Tasks Examples:**
- Downloading large files 📥
- Uploading data to servers 🌐
- Syncing contacts/emails 🔄
- Processing images or videos 🎞️
- Regular checking of device's location 🧭

---

# 🔥 **How to Do Background Processing in Android:**
There are special types of Services built for background work:
- **Service** (basic background processing)
- **IntentService** (background worker that automatically stops)
- **WorkManager** (for guaranteed, scheduled background work)
- **JobIntentService** (legacy support for older Android versions)
- **Foreground Service** (background + notification)


| Type | Purpose |
|:---|:---|
| `IntentService` (deprecated) | Automatically handles background tasks on worker thread |
| `JobIntentService` | Safer background work, especially on newer Android versions |
| `JobScheduler` | Schedule background jobs for better battery usage |
| `WorkManager` | Best choice today — smart background work manager |

✅ Today, **WorkManager** is highly recommended for background tasks!

---

# 🧩 **Simple Diagram:**

```
          [User Opens App]
                 ↓
          [User Triggers Big Task]
                 ↓
         [Service Starts in Background]
                 ↓
       [Main UI remains free & fast ✅]
                 ↓
      [Service Completes Task Silently]
```

---

Since Android 8.0 (Oreo), background service limitations require us to use **JobScheduler** or **WorkManager** for background tasks.

**Example: Scheduling Background Work with WorkManager**

```java
import androidx.work.OneTimeWorkRequest;
import androidx.work.WorkManager;

OneTimeWorkRequest workRequest = new OneTimeWorkRequest.Builder(MyWorker.class).build();
WorkManager.getInstance(this).enqueue(workRequest);
```

**Worker Class for WorkManager:**

```java
import android.content.Context;
import androidx.annotation.NonNull;
import androidx.work.Worker;
import androidx.work.WorkerParameters;
import android.util.Log;

public class MyWorker extends Worker {

    public MyWorker(@NonNull Context context, @NonNull WorkerParameters params) {
        super(context, params);
    }

    @NonNull
    @Override
    public Result doWork() {
        // Background task
        Log.d("MyWorker", "Background task running");
        return Result.success();
    }
}
```



