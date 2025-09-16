
____
# Question Unit 2. Android Development Environment
___

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
_______

### **Easy Questions**

1. **What is an Activity in an Android application?**
   - *Answer*: An Activity represents a single screen with a user interface. Examples include the Home Screen, Login Screen, or Settings Screen.

2. **What is the purpose of a Service in Android?**
   - *Answer*: A Service runs background tasks, like playing music or downloading files, even when the user switches to another app.

3. **What does a Broadcast Receiver do in an Android application?**
   - *Answer*: A Broadcast Receiver listens for system-wide events, such as battery low or network changes, and allows the app to respond to those events.

4. **Why would you use a Content Provider in an Android app?**
   - *Answer*: A Content Provider is used to share data between different applications, such as sharing contacts or photos.

5. **What is an Intent in Android?**
   - *Answer*: An Intent is used to start activities, services, or send broadcasts. It can also pass data between components.

6. **How do Fragments help in Android UI design?**
   - *Answer*: Fragments allow for modular sections of a UI within an activity, enabling flexible layouts, especially for tablets or multi-pane screens.

7. **What information is typically stored in the Android Manifest file?**
   - *Answer*: The Android Manifest file defines the app's components, permissions, and configurations, such as the app's activities and services.

---

### **Scenario-Based Questions**

1. **Imagine you are creating a music player app that continues to play music when the user opens another app. Which Android component would you use, and why?**
   - *Answer*: You would use a **Service** because it allows tasks to run in the background, like playing music, even when the user switches to another app.

2. **Suppose you need to display a settings screen and a profile screen in the same app. Which Android component would you use for each screen, and how would you manage them?**
   - *Answer*: Each screen can be an **Activity** (e.g., SettingsActivity and ProfileActivity). You would navigate between them using **Intents**.

3. **Your app needs to notify users when their battery is low. What Android component can you use to detect this event, and how would it work?**
   - *Answer*: Use a **Broadcast Receiver** to listen for the "battery low" broadcast from the system and respond with a notification to the user.

4. **You are building a messaging app and want to check if new messages are available every few minutes in the background. What component should you use, and what is the main advantage of using it?**
   - *Answer*: Use a **Service** because it can run in the background and perform tasks like checking for new messages without needing a user interface.

5. **If you wanted to allow other apps to access your app's data, such as saved notes or contacts, which Android component would you implement?**
   - *Answer*: Use a **Content Provider** to safely manage and share your app's data with other apps.

6. **In your app, you have two screens—a list of items and a detailed view of an item when clicked. You want to reuse the list on multiple screens. Which component is best for this and why?**
   - *Answer*: Use a **Fragment** for the list because it is modular and can be reused in multiple screens, allowing a flexible UI design.

7. **You are developing an Android app where the user can add items to a shopping list. When a user rotates the device, how would you manage the activity life cycle to prevent data loss?**
   - *Answer*: Use the **onSaveInstanceState** and **onRestoreInstanceState** methods in the **Activity** life cycle to save and restore the user's data when the device rotates.

8. **If you want to start another screen in your app with some data, like an email app opening a compose screen with a draft, how would you do it?**
   - *Answer*: Use an **Intent** with extra data to start the new screen, such as using `intent.putExtra("message", "Draft message")` to pass the draft text.

9. **Your app displays a contact list, and you want it to access the user’s contacts stored on the phone. Which component and Android feature do you need to access this information?**
   - *Answer*: Use a **Content Provider** to access the contacts and request the **Contacts permission** in the **Android Manifest** file.

10. **What would happen if an app didn't declare a required permission in its Manifest but tried to access restricted data?**
    - *Answer*: The app would crash or throw a security exception, as Android checks that all necessary permissions are declared in the **Manifest** file before accessing restricted data.

11. **A mobile banking app needs to ensure high security and seamless user experience. Analyze which Android components should be prioritized.**

Solution:

Activities → For secure login screens and transactions.

Services → For background processing like transaction updates.

Content Providers → For secure database access and data sharing.

Broadcast Receivers → For push notifications (OTP, fraud alerts).
✅ Priority should be on Activities + Services + Content Providers because they directly affect security and user trust.
---
