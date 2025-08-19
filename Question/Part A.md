**Questions**

Q1. Identify the process, if a user wants to communicate between more than one activities.  (2)

Q2. Discuss the steps involved to design an application on Android Studio in detail. Mention the process involved in creation of an Android Virtual Device for displaying the results on screen.(12)

Q3. Define the following term: Android Platform and Android SDK (Software Development Kit) (2)

Q4. What is Android API (Application Programming Interface)? How do developers use them? (2)

- In Android, an API is like a set of ready-made tools and instructions that Google provides to developers so they can build apps.
- Instead of creating everything from scratch (like camera, Bluetooth, location, notifications, etc.), developers can use Android APIs to access these features safely and easily.

**Example:**
If your app needs to take a photo, you don’t write code to control the camera hardware directly. Instead, you use the Camera API, and Android takes care of the rest.

**How do developers use Android APIs?**

1. Import the right library/package: 
Developers include Android libraries in their project (like android.location for GPS, android.media for audio, etc.).

2. Call the API methods: 
They use the methods (functions) provided by the API.

Example: To get GPS location → use LocationManager class methods.

3. Get the output/use the service: 
API gives back results (like current location, camera image, sensor data) that the app can use.

**Example for better clarity**

Suppose you want to build a Maps app:

- Without API → You would have to code GPS signal reading, map rendering, route finding (impossible for one developer).

- With API → Just use Google Maps API and Android Location API → boom! Your app can show maps and locations.
