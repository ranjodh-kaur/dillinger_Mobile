# Unit 4. Apps Interactivity in Android
______
**Topics:**
- **Android Fragment:** Fragment Class, Fragment Life Cycle
- **Android Intent Class:** Intent types, Intent Filters, Instantiating Intent Object, Android Context Class
- **Event Processing:** Events, Event Listener, Event Handler
_____
 **Android Fragments** with a new example!

##### Android Fragment: Fragment Class and Fragment Life Cycle
**Analogy**: Think of your phone as a *television*. The television has a big screen (this represents the *activity* in Android), and the *fragments* are like *channels* on that screen. Each channel shows different content, such as news, sports, or a movie. You can switch between these channels (fragments) while still using the same screen (activity).

Now, when you switch between channels, sometimes the content on the channel is paused, or new content is loaded. This is similar to the *fragment life cycle* in Android, where each fragment goes through different stages, like being shown, paused, or stopped.

#### **1. Fragment Class**:

- A *Fragment Class* in Android is like the blueprint or the design of a specific channel (content) you want to show on the screen. It defines what you will see on the screen (the layout and behavior) when you switch to that channel.

- Just like each channel on the TV has its own content (like news, weather, or sports), each fragment can have its own UI and logic.

**Code Example for Fragment Class**:
Here’s a simple example of a *Fragment Class* that displays a text message.

```java
public class MyFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflating (setting up) the layout for this fragment
        return inflater.inflate(R.layout.fragment_my, container, false);
    }
}
```

This *MyFragment* class would display a layout (UI) defined in `fragment_my.xml`. It’s like setting up a specific channel (fragment) with content (layout) you want to show.

#### **2. Fragment Life Cycle**:

- Just like switching a TV channel, a *Fragment Life Cycle* describes the different stages a fragment goes through. Imagine your channel (fragment) going through different stages:

1. **onCreate()**: When you first turn on the TV and select a channel. The content starts loading.
2. **onCreateView()**: The screen (UI) of the channel starts showing.
3. **onStart()**: The channel starts showing content on the screen.
4. **onResume()**: The channel is fully active, and you can interact with it.
5. **onPause()**: If you change the channel, the current channel goes to the background but doesn’t disappear completely.
6. **onStop()**: If you completely change the channel, the old channel is stopped and no longer visible.
7. **onDestroy()**: If you turn off the TV, the channel (fragment) is completely destroyed.

Think of these stages as managing what happens when you switch channels or change content on your TV.

#### **Example of Fragment Life Cycle**:

Here’s how the fragment goes through its life cycle:

```java
public class MyFragment extends Fragment {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d("Fragment", "Fragment Created");
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        Log.d("Fragment", "Fragment View Created");
        return inflater.inflate(R.layout.fragment_my, container, false);
    }

    @Override
    public void onStart() {
        super.onStart();
        Log.d("Fragment", "Fragment Started");
    }

    @Override
    public void onResume() {
        super.onResume();
        Log.d("Fragment", "Fragment Resumed");
    }

    @Override
    public void onPause() {
        super.onPause();
        Log.d("Fragment", "Fragment Paused");
    }

    @Override
    public void onStop() {
        super.onStop();
        Log.d("Fragment", "Fragment Stopped");
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.d("Fragment", "Fragment Destroyed");
    }
}
```

In this code:
- **Log statements** track when each stage of the fragment life cycle happens.
- The fragment goes through different stages (e.g., onCreate, onStart, onPause) just like a TV channel changes its state when you switch between channels.

#### **What Happens When You Switch Between Fragments?**

Imagine you are watching TV, and you switch from Channel 1 to Channel 2. Here’s how the *fragment life cycle* would play out:

- **Fragment 1 (Channel 1)**: When you switch to Channel 2, Fragment 1 gets paused and then stopped. It’s not removed immediately — it's just not visible to you.
- **Fragment 2 (Channel 2)**: When you switch to Channel 2, Fragment 2 is created and starts showing content on the screen. It goes through the **onCreate**, **onStart**, and **onResume** methods.

#### **Summary**:

- **Fragment Class** is like a channel you create for your app, each channel showing different content.
- **Fragment Life Cycle** is the set of states (like turning on the TV, watching, or pausing) that each fragment goes through.

By using fragments, you can make your app more flexible and modular, just like you can have many different channels on a TV, and switch between them seamlessly without turning off the TV itself.

#### Example of fragment in Android

Suppose you have an app with a screen showing:

- A list of items on the left side.

- The details of the selected item on the right side.

Here:

- ListFragment → shows the list of items.

- DetailFragment → shows the details of the selected item.

Both fragments live inside one Activity.

#### Benefits of Fragments

- Reusable UI components.

- Better support for different screen sizes (mobile vs tablet).

- Can add/replace fragments dynamically at runtime.

_____________________________
A **basic Fragment example** using **XML + Java**.

---

## 1. Create a Fragment Class

```java
// ExampleFragment.java
package com.example.myapp;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import androidx.fragment.app.Fragment;

public class ExampleFragment extends Fragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_example, container, false);
    }
}
```

---

## 2. Create Fragment Layout (XML)

📄 `res/layout/fragment_example.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="20dp">

    <TextView
        android:id="@+id/fragmentText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, I am a Fragment!"
        android:textSize="18sp"
        android:textColor="@android:color/black" />

</LinearLayout>
```

---

## 3. Add a Container for Fragment in Activity Layout

📄 `res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

---

## 4. Load Fragment in Activity

📄 `MainActivity.java`

```java
package com.example.myapp;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Load fragment dynamically
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .replace(R.id.fragment_container, new ExampleFragment())
                    .commit();
        }
    }
}
```

---

## Output

When you run the app, your **MainActivity** will show the **Fragment UI** with:
`"Hello, I am a Fragment!"`

---


* You can add **multiple fragments** inside one activity.
* You can also replace fragments at runtime (useful for navigation, tabs, etc.).


---

#### **Example: ListFragment + DetailFragment**

---

#### 1. **Create ListFragment** (shows a list of items)

📄 `ListFragmentExample.java`

```java
package com.example.myapp;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import androidx.fragment.app.Fragment;

public class ListFragmentExample extends Fragment {

    String[] items = {"Apple", "Banana", "Cherry", "Mango"};

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {

        View view = inflater.inflate(R.layout.fragment_list, container, false);

        ListView listView = view.findViewById(R.id.listView);

        ArrayAdapter<String> adapter = new ArrayAdapter<>(
                getActivity(),
                android.R.layout.simple_list_item_1,
                items
        );

        listView.setAdapter(adapter);

        // Handle item click
        listView.setOnItemClickListener((parent, view1, position, id) -> {
            String selected = items[position];

            // Replace DetailFragment and pass data
            DetailFragmentExample detailFragment = DetailFragmentExample.newInstance(selected);

            getActivity().getSupportFragmentManager().beginTransaction()
                    .replace(R.id.fragment_container, detailFragment)
                    .addToBackStack(null) // allows going back
                    .commit();
        });

        return view;
    }
}
```

---

## 2. **Layout for ListFragment**

📄 `res/layout/fragment_list.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</FrameLayout>
```

---

## 3. **Create DetailFragment** (shows details of clicked item)

📄 `DetailFragmentExample.java`

```java
package com.example.myapp;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.fragment.app.Fragment;

public class DetailFragmentExample extends Fragment {

    private static final String ARG_ITEM = "selected_item";
    private String selectedItem;

    // Factory method to pass data
    public static DetailFragmentExample newInstance(String item) {
        DetailFragmentExample fragment = new DetailFragmentExample();
        Bundle args = new Bundle();
        args.putString(ARG_ITEM, item);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getArguments() != null) {
            selectedItem = getArguments().getString(ARG_ITEM);
        }
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_detail, container, false);

        TextView textView = view.findViewById(R.id.detailText);
        textView.setText("You selected: " + selectedItem);

        return view;
    }
}
```

---

## 4. **Layout for DetailFragment**

📄 `res/layout/fragment_detail.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="20dp">

    <TextView
        android:id="@+id/detailText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Details will appear here"
        android:textSize="20sp"
        android:textColor="@android:color/holo_blue_dark"/>
</FrameLayout>
```

---

## 5. **Main Activity**

📄 `MainActivity.java`

```java
package com.example.myapp;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if (savedInstanceState == null) {
            // Start with ListFragment
            getSupportFragmentManager().beginTransaction()
                    .replace(R.id.fragment_container, new ListFragmentExample())
                    .commit();
        }
    }
}
```

---

## 6. **Activity Layout**

📄 `res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

---

#### Output

1. App starts → shows **ListFragment** with `Apple, Banana, Cherry, Mango`.
2. Click `Mango` → **DetailFragment** replaces it and shows →
   `"You selected: Mango"`.
3. Press back → returns to list.


_________________
#### Android Intent Class: Intent Types, Intent Filters, Instantiating Intent Object, and Android Context Class
__________________

Let’s break this down with a simple analogy!

**Analogy**: Think of an *Intent* as a *messenger* delivering a *message* (data or instructions) to a different part of your app or even to a different app. The *intent* tells the system, "I need this job done, can you pass this message along?"

##### **1. Intent Types**:
An *Intent* can be thought of as a request. There are two types of requests:

1. **Explicit Intent**: This is like telling the messenger exactly who to deliver the message to. You specify exactly where the message should go (like telling someone, "Go to John and tell him something").
   
   **Example**: You want to go from one screen to another in your app.
   ```java
   Intent intent = new Intent(this, SecondActivity.class);
   startActivity(intent);
   ```

2. **Implicit Intent**: This is like telling the messenger, "I need you to deliver this message, but I don't care who gets it, just find anyone who can do it." The system will figure out which app or component can handle the request.

   **Example**: Opening a webpage using any browser app.
   ```java
   Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.example.com"));
   startActivity(intent);
   ```

##### **2. Intent Filters**:
An *Intent Filter* is like setting up a rule for the messenger to figure out which kind of messages they should deliver. It helps to identify what types of requests can be handled by a component in your app.

- For example, if you want your app to be able to handle a specific kind of action, like opening a webpage or taking a photo, you add an *Intent Filter* to tell Android which actions your app can perform.

   **Example of an Intent Filter in the AndroidManifest.xml**:
   ```xml
   <activity android:name=".BrowserActivity">
       <intent-filter>
           <action android:name="android.intent.action.VIEW" />
           <category android:name="android.intent.category.DEFAULT" />
           <data android:scheme="http" android:host="www.example.com" />
       </intent-filter>
   </activity>
   ```

- This setup means your app is prepared to handle requests to open "http://www.example.com" in the browser.

##### **3. Instantiating an Intent Object**:
When you create an *Intent*, you’re essentially creating the message that will be sent. You can instantiate the *Intent* object like this:

- **Explicit Intent Example** (going from one activity to another):
   ```java
   Intent intent = new Intent(CurrentActivity.this, NextActivity.class);
   startActivity(intent);
   ```

- **Implicit Intent Example** (opening a URL in a browser):
   ```java
   Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.example.com"));
   startActivity(intent);
   ```

##### **4. Android Context Class**:
Now, in our analogy, the *Context* is like the "environment" where the messenger operates. Think of the *Context* as the *office manager* who handles the process of sending the message and determining what happens next.

- The *Context* gives the *Intent* the environment it needs to execute tasks. It’s like the messenger needing an office manager (context) to know where to go and how to get things done.

In Android, *Context* refers to the current state or environment of the application or activity. It provides access to system services and resources.

**Example**:
```java
Context context = getApplicationContext();
```
- *getApplicationContext()* is used to get the global context, which is useful when you need the application-level environment for your intent or other tasks.

### **Putting It All Together**:

Imagine you want your messenger to deliver a message in your app:

1. **Create the Intent (Message)**: You create an *Intent* (the message) to specify what task needs to be done. This could be launching a new screen or opening a URL.
   
2. **Add Intent Filter (Messenger’s Instructions)**: You set up an *Intent Filter* to guide the system on how to deliver this message, so the right app or component can handle it.

3. **Use Context (Office Manager)**: The *Context* is the environment that helps the *Intent* deliver the message by accessing system services.

4. **Send the Intent (Deliver the Message)**: You send the message using `startActivity()`, and the system takes care of delivering it to the right place.

### **Code Example to Summarize**:

Let’s combine all the concepts into one example, where you open a webpage using an implicit intent and pass some extra data:

```java
// Step 1: Create an implicit intent to open a URL
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.example.com"));

// Step 2: Add some extra data to the message (optional)
intent.putExtra("message", "Hello, this is extra data!");

// Step 3: Use the context to start the activity (send the message)
Context context = getApplicationContext();
context.startActivity(intent);
```

### **Summary**:
- **Intent**: A message that tells the system to do something (like opening a webpage or starting a new activity).
- **Intent Filter**: A set of rules that helps the system understand what type of request an intent handles.
- **Context**: The environment or office manager that helps the intent get things done, like accessing system resources and services.
- **Explicit and Implicit Intents**: Explicit intents are like directing the messenger to a specific person, while implicit intents let the system decide who should handle the message.

By using these components, you can efficiently manage how activities communicate with each other in your Android app!

______________
#### Event Processing in Android: Events, Event Listeners, Event Handlers

#### **Analogy**:
Imagine you're at a concert, and the band is playing music (this represents the **event**). You, as a concertgoer, are the **event listener** because you're paying attention to what’s happening on stage (the event). The **event handler** is like the concert organizer or the band, who decides what happens when the music plays – they choose the actions to take, like starting a new song, or adjusting the volume.

### **1. Events**:
An **event** in Android is like a trigger or an action that happens when something occurs. For example:
- A **button** is clicked.
- The **screen is touched**.
- The **device is rotated**.

These actions are **events**, and they require a response.

**Example of an event**: 
When the user clicks a button, that click is the **event**.

### **2. Event Listener**:
An **event listener** is like the person who’s watching out for these events to happen. The listener is just waiting for something (like a button press) to occur, and when it does, it reacts to it. 

- In Android, an **event listener** is an object (like a **Button** or **TextView**) that waits for certain actions (events) to occur and then listens for those events.
  
**Example**: A **Button** listens for a user click event. The button doesn't do anything until the user clicks on it.

### **3. Event Handler**:
Once the event is detected, the **event handler** takes over. It's like the organizer at the concert who determines what to do when the song starts. It contains the **code** that specifies what should happen when the event occurs.

- For example, when a button is clicked, the event handler could change the text on the button or navigate to another screen.

**Example**: The event handler might change the color of the button when it is clicked.

### **Putting It All Together**:

Imagine you’re at a concert (Android app):
- **Event**: The band starts playing a new song (button clicked).
- **Event Listener**: You are watching the concert and waiting for the band to start playing (listening for the button click).
- **Event Handler**: The band adjusts the music volume or transitions to another song (action taken after the button is clicked).

#### **Code Example in Android**:

Let’s create a simple example where we have a **button**, and when it’s clicked, the **event handler** will change the text on the button.

```java
// In your MainActivity.java

// Step 1: Find the button by ID
Button myButton = findViewById(R.id.my_button);

// Step 2: Set up the event listener to listen for clicks
myButton.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // Step 3: Event Handler - What happens when the button is clicked
        myButton.setText("Button was clicked!");
    }
});
```

### **Explanation**:
- **Event**: The user clicks the button.
- **Event Listener**: `setOnClickListener()` method listens for that click event.
- **Event Handler**: When the button is clicked, the `onClick()` method executes, changing the button’s text to "Button was clicked!"

### **Summary**:
- **Event**: An action that happens (like a button click).
- **Event Listener**: A watcher waiting for the event to happen (listens for the button click).
- **Event Handler**: The action taken when the event happens (changes the text of the button).

This system is key to making your Android app interactive! When you use event listeners and handlers, you can ensure your app responds to user input and behaves as expected.


