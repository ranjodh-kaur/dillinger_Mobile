# UNIT 5. Persistent Data Storage

***

**SQLite**

* Android Built in SQLite content provider
* Modifying data using your android application
* Creating basic activity
* Configuring manifest
* Packaging and managing SQLite with android app

***

**What is Persistent Data Storage?**

Think of persistent data storage as a **notebook** you keep at home. Whenever you want to save important information that you'll need later (like a shopping list or your favorite recipes), you write it in this notebook so it doesn't get lost, even if you close it and come back days later.

Imagine your phone is like a small library, and **SQLite** is a librarian who helps you organize, store, and retrieve books (data) without needing internet access or a connection to a larger, external library (cloud database).

Here's how this analogy works:

1. **Database = Library Room**: The entire SQLite database is like a small library room where all your information (books) is stored in an organized way.
2. **Tables = Bookshelves**: Within the library, you have bookshelves for different categories. Each SQLite table is like a shelf with specific information—for instance, a "Contacts" shelf for phone numbers and names or a "Notes" shelf for your saved notes.
3. **Rows = Books**: Each row in a table is like an individual book with all the relevant details. For example, each book on the "Contacts" shelf would have a name, phone number, and address.
4. **Columns = Book Sections**: Within each book (row), there are specific sections (columns) like the title, author, and summary. In SQLite, these columns could be the name, phone number, and email fields in a "Contacts" table.
5. **Query = Asking the Librarian**: Whenever you need a specific book (data), you ask the librarian (SQLite) with a query, like "find all contacts with the name 'John'." SQLite fetches the exact information you need without you needing to search the entire library.
6. **CRUD Operations = Adding, Updating, Removing Books**:
  - **Create**: Add new books to the shelf (insert new data).
  - **Read**: Retrieve existing books (get data).
  - **Update**: Edit book details if information has changed.
  - **Delete**: Remove books that are no longer needed.
7. **Content Providers = Library's Interlibrary Loan**: When other libraries (apps) need data from your library, they use a formal system called content providers, like an "interlibrary loan" to safely share information.

In short, SQLite is like having a personal librarian in your phone to keep all your data organized, easily accessible, and stored locally without needing internet access.

***

### **SQLite in Android**

### **Theory** :

* **What is SQLite?**: SQLite is a lightweight database engine included in Android. It helps you store data on the device so the app can remember information even after it's closed.
* **When to Use It?**: Use SQLite for small to medium-sized data storage needs (e.g., saving user preferences, storing notes, or to-do lists). When you want to store things like user data or app settings that should remain saved even after the app is closed, SQLite is a great solution.
* **Example:**
Suppose you're building a Notes app. You want to save each note so the user can view or edit it later, even if they close and reopen the app. You can use SQLite to create a database that stores each note with a title, content, and date.

## What is `DBHelper.java`?

`DBHelper` is a Java class that **extends** the `SQLiteOpenHelper` class provided by Android.  
This gives you all the tools you need to manage SQLite databases.

---

## Basic Structure of `DBHelper.java`

```java
public class DBHelper extends SQLiteOpenHelper {
    
    private static final String DATABASE_NAME = "StudentDB";
    private static final int DATABASE_VERSION = 1;

    public DBHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        // Called when database is created for the FIRST time
        db.execSQL("CREATE TABLE students(id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, marks INTEGER)");
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        // Called when DATABASE_VERSION is increased
        db.execSQL("DROP TABLE IF EXISTS students");
        onCreate(db);
    }

}
```

---

##  Detailed Breakdown

### Class Declaration

```java
public class DBHelper extends SQLiteOpenHelper
```
This means DBHelper **inherits** SQLiteOpenHelper, giving access to database methods.

---

### Constructor

```java
public DBHelper(Context context) {
    super(context, DATABASE_NAME, null, DATABASE_VERSION);
}
```
- `DATABASE_NAME`: Name of the `.db` file that stores the database
- `DATABASE_VERSION`: Used for versioning and upgrades
- `null`: CursorFactory (not needed for most cases)

---

### onCreate()

```java
@Override
public void onCreate(SQLiteDatabase db) {
    db.execSQL("CREATE TABLE students(id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, marks INTEGER)");
}
```
- **Triggered only once**: When the database is created for the first time
- **Creates a table** called `students` with 3 columns:
  - `id` → auto-incremented primary key
  - `name` → student’s name (TEXT)
  - `marks` → student’s marks (INTEGER)

---

### onUpgrade()

```java
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    db.execSQL("DROP TABLE IF EXISTS students");
    onCreate(db);
}
```
- Called when the version number is changed.
- Deletes the old table and recreates it.
- Useful when you want to change the schema (like adding/removing columns).

---

## Summary

| Method              | Purpose                              |
|---------------------|--------------------------------------|
| `onCreate()`        | Create table for the first time      |
| `onUpgrade()`       | Modify table structure (if version updated) |

---
Create a **simple Student App** where you can:

- Add student details (name & email)
- View all students in a list

---

## Task:

- **Activity**: MainActivity.java
- **Database Helper**: DBHelper.java
- **UI**: `activity_main.xml` (for input and buttons)

---

##  File 1: `activity_main.xml`  
Located in: `res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/editName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Name" />

    <EditText
        android:id="@+id/editEmail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Email"
        android:inputType="textEmailAddress" />

    <Button
        android:id="@+id/btnInsert"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Insert" />

    <Button
        android:id="@+id/btnView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="View All" />

    <TextView
        android:id="@+id/textViewResult"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Results will appear here"
        android:textSize="16sp"
        android:paddingTop="10dp" />
</LinearLayout>
```

---

## File 2: `DBHelper.java`  
Located in: `java/com.yourpackagename/DBHelper.java`

```java
package com.example.sqliteexample;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DBHelper extends SQLiteOpenHelper {

    public static final String DBNAME = "StudentDB.db";

    public DBHelper(Context context) {
        super(context, DBNAME, null, 1);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE Student(id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, email TEXT)");
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS Student");
        onCreate(db);
    }

    public boolean insertData(String name, String email) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();

        values.put("name", name);
        values.put("email", email);

        long result = db.insert("Student", null, values);
        return result != -1;  // true if success
    }

    public Cursor getAllData() {
        SQLiteDatabase db = this.getReadableDatabase();
        return db.rawQuery("SELECT * FROM Student", null);
    }
}
```

---

##  File 3: `MainActivity.java`  
Located in: `java/com.yourpackagename/MainActivity.java`

```java
package com.example.sqliteexample;

import androidx.appcompat.app.AppCompatActivity;
import android.database.Cursor;
import android.os.Bundle;
import android.view.View;
import android.widget.*;

public class MainActivity extends AppCompatActivity {

    EditText editName, editEmail;
    Button btnInsert, btnView;
    TextView textViewResult;
    DBHelper db;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editName = findViewById(R.id.editName);
        editEmail = findViewById(R.id.editEmail);
        btnInsert = findViewById(R.id.btnInsert);
        btnView = findViewById(R.id.btnView);
        textViewResult = findViewById(R.id.textViewResult);

        db = new DBHelper(this);

        btnInsert.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String name = editName.getText().toString();
                String email = editEmail.getText().toString();

                boolean inserted = db.insertData(name, email);
                if (inserted) {
                    Toast.makeText(MainActivity.this, "Inserted Successfully", Toast.LENGTH_SHORT).show();
                } else {
                    Toast.makeText(MainActivity.this, "Insertion Failed", Toast.LENGTH_SHORT).show();
                }
            }
        });

        btnView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Cursor res = db.getAllData();
                if (res.getCount() == 0) {
                    textViewResult.setText("No data found");
                    return;
                }

                StringBuilder buffer = new StringBuilder();
                while (res.moveToNext()) {
                    buffer.append("ID: ").append(res.getInt(0)).append("\n");
                    buffer.append("Name: ").append(res.getString(1)).append("\n");
                    buffer.append("Email: ").append(res.getString(2)).append("\n\n");
                }

                textViewResult.setText(buffer.toString());
            }
        });
    }
}
```


## Output

| Input Form                     | Result |
|-------------------------------|--------|
| Name: John <br> Email: j@x.com | Inserted Successfully |
| Press “View All”              | Shows list of students |

---

### **Android Built-in SQLite Content Provider**

#### **Theory** :

* **What is a Content Provider?**: A content provider lets apps share data with other apps in a secure way. For example, your app could use a content provider to access contacts stored on the device.
* **Analogy:** Think of the content provider as index tabs in your filing cabinet, helping you and other people find what they need quickly. It allows different apps to share and access certain information (like contacts or calendar events) easily.
  - Imagine your Android device as a building with several different rooms (apps), each room holding its own library (database) of information. The Android Built-in **SQLite Content Provider** acts like a secure receptionist stationed in each room, responsible for carefully sharing library resources with other rooms, only when needed and allowed.

Here's how this analogy breaks down:

1. **Library Rooms = Individual Apps**: Each app on your device has its own private library of data, like contacts, notes, or photos. By default, each library is private and not accessible to other apps directly.
2. **Content Provider = Receptionist**: A content provider acts as a receptionist or a front desk in each app's "room." The receptionist's job is to determine who is allowed to access specific data from the app and to handle any requests securely.
3. **URI = A Library Card ID**: Each piece of data has a unique "library card" ID, called a URI (Uniform Resource Identifier). If another app wants to access data from a specific library, it must present the right URI to the content provider.
4. **Requesting Data = Borrowing a Book**: If an app (say, a messaging app) needs access to contacts, it will approach the receptionist (Content Provider) with a URI that specifies "I need contact details." The Content Provider checks if the requester has permission to access this data and, if allowed, retrieves the specific data without exposing other data in the app's library.
5. **CRUD Operations = Borrowing, Returning, Updating Books**:
  - **Create**: Another app can request to add new information to the library (like adding a contact).
  - **Read**: The app can request to view a specific book or a set of books (like reading all contacts).
  - **Update**: It might request changes, like updating an address.
  - **Delete**: An app could request to remove information (like deleting a contact).
6. **Permissions = Security Checkpoints**: The receptionist (Content Provider) only grants access to other apps if they have the proper permissions. For example, an app must have permission to access contacts before it can read or modify contact data. This security layer ensures data privacy and prevents unauthorized access.
7. **Inter-App Data Sharing = Controlled Book Loan**: Just as a receptionist ensures only authorized people can borrow books from a room, the content provider ensures data is shared in a controlled and secure manner. Each request is handled independently, so only specific data is accessed without exposing other information.

So, the Android Built-in SQLite Content Provider is like a vigilant receptionist, managing and monitoring all requests for access to an app's library and ensuring data security and controlled sharing across apps.

* **Why Use It?**: Content providers help with data sharing between apps or parts of the Android system, especially when data is stored in SQLite.
* **Example:**<br>
Let's say you want to create an app that shows all contacts from the phone. Android's built-in Contacts content provider lets your app access and display contacts stored in the phone's SQLite database. You don't have to create a new database for contacts; you can read the shared contact data through the content provider.

#### **Practical Steps** :

1. **Create a Content Provider Class**: Extend the `ContentProvider` class to implement methods for querying, inserting, updating, and deleting data.

    ```java
    public class NotesProvider extends ContentProvider {
    private DatabaseHelper dbHelper;

    @Override
    public boolean onCreate() {
        dbHelper = new DatabaseHelper(getContext());
        return true;
    }

    // Override methods for data access: query, insert, update, delete
    }
    ```
2. **Register the Content Provider** in the `AndroidManifest.xml` file:

    ```xml
    <provider
    android:name=".NotesProvider"
    android:authorities="com.example.notesprovider"
    android:exported="true" />
    ```

***

### **Modifying Data Using Your Android Application**

#### **Theory** :

* **Why Modify Data?**: To make your app more interactive, users need the ability to add, edit, or delete data.
* **How SQLite Manages Data**: SQLite uses SQL commands to manage data (like `INSERT`, `UPDATE`, and `DELETE`).

#### **Practical Steps** :

1. **Insert Data Example**:

    ```java
    public void addNote(String title, String content) {
    SQLiteDatabase db = dbHelper.getWritableDatabase();
    ContentValues values = new ContentValues();
    values.put("title", title);
    values.put("content", content);
    db.insert("Notes", null, values);
    }
    ```
2. **Update and Delete Data**:

    ```java
    public void updateNote(int id, String newTitle) {
    SQLiteDatabase db = dbHelper.getWritableDatabase();
    ContentValues values = new ContentValues();
    values.put("title", newTitle);
    db.update("Notes", values, "id=?", new String[]{String.valueOf(id)});
    }

    public void deleteNote(int id) {
    SQLiteDatabase db = dbHelper.getWritableDatabase();
    db.delete("Notes", "id=?", new String[]{String.valueOf(id)});
    }
     ```
________

### **Creating a Basic Activity**

#### **Theory** :

* **What is an Activity?**: An Activity is a screen in the app where users can see and interact with the app's features.
* **Why Create Multiple Activities?**: Different activities handle different tasks, like creating a note, viewing notes, or editing a note.

#### **Practical Steps** :

1. **Create an XML Layout** for the activity (activity_add_note.xml):

    ```xml
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <EditText
        android:id="@+id/titleEditText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Title" />

    <EditText
        android:id="@+id/contentEditText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Content" />

    <Button
        android:id="@+id/saveButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Save" />
    </LinearLayout>
    ```
2. **Create the Java Activity Class** (AddNoteActivity.java):

    ```java
    public class AddNoteActivity extends AppCompatActivity {
    private EditText titleEditText, contentEditText;
    private DatabaseHelper dbHelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_note);

        titleEditText = findViewById(R.id.titleEditText);
        contentEditText = findViewById(R.id.contentEditText);
        dbHelper = new DatabaseHelper(this);

        findViewById(R.id.saveButton).setOnClickListener(view -> {
            String title = titleEditText.getText().toString();
            String content = contentEditText.getText().toString();
            addNoteToDatabase(title, content);
        });
    }

    private void addNoteToDatabase(String title, String content) {
        // Use dbHelper to add the note to the database
    }
    }
    ```

***

### **Configuring Manifest**

#### **Theory** :

* **What is the Manifest?**: `AndroidManifest.xml` is a configuration file that defines the components of your app (activities, services, content providers, etc.) and any required permissions.
* **Definition:** The manifest is a configuration file that tells Android about all the different parts of your app, including permissions and activities.
* **Analogy:** Think of the manifest as a table of contents in your notebook. It lists everything in the app and lets Android know what it can do, like asking for permission to save data or access files.

#### **Practical Steps** :

1. **Add Activities and Providers**:

    ```xml
    <application>
    <activity android:name=".AddNoteActivity" />
    <provider android:name=".NotesProvider" />
    </application>
    ```
2. **Add Permissions (if needed)**, like Internet access or storage access.

**Configuring the Manifest**
**1. Add Permissions and Activity Declaration:**
- Open AndroidManifest.xml and add:

    ```xml
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.yourapp">

    <application
        android:allowBackup="true"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">
        <activity android:name=".AddNoteActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <provider
            android:name=".NotesProvider"
            android:authorities="com.example.notesprovider"
            android:exported="true" />
    </application>
    </manifest>
    ```
_____

### **Packaging and Managing SQLite with Android App**

#### **Theory** :

* **Definition:** Packaging means setting up the SQLite database so it's ready to work smoothly with the app, storing and retrieving data as needed.
* **Example:**

In the Notes app, you might create a DatabaseHelper class that extends SQLiteOpenHelper. This class handles database setup and management, like creating tables and handling upgrades when your app changes.
* **Database Helper Role**: The `DatabaseHelper` class manages SQLite database operations and is critical for organizing your app's data.
* **Handling Upgrades**: Increment `DATABASE_VERSION` in the helper class to handle database updates or changes.

    ```java
  public class DatabaseHelper extends SQLiteOpenHelper {
  private static final String DATABASE_NAME = "notes_db";
  private static final int DATABASE_VERSION = 1;
  
  public DatabaseHelper(Context context) {
      super(context, DATABASE_NAME, null, DATABASE_VERSION);
  }
  
  @Override
  public void onCreate(SQLiteDatabase db) {
      String createTable = "CREATE TABLE Notes (id INTEGER PRIMARY KEY, title TEXT, content TEXT)";
      db.execSQL(createTable);
  }
  
  @Override
  public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
      db.execSQL("DROP TABLE IF EXISTS Notes");
      onCreate(db);
       }
  }
    ```

#### **Practical Steps** :
1. **Create a Database Helper** to centralize data handling.
2. **Handle Database Versioning** in `DatabaseHelper`:
    ```java
    public class DatabaseHelper extends SQLiteOpenHelper {
    private static final int DATABASE_VERSION = 1;

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        // Handle database upgrade here
    }
    }
    ```
3. **Inspect and Debug** the SQLite database by connecting to the emulator or device with Android Studio's Device File Explorer.

* **Database Versioning:** As you develop the app and add new features, you may need to change the database structure (like adding tables or columns). Update _DATABASE_VERSION_ in _DatabaseHelper_ to reflect these changes and handle them in _onUpgrade()_.

***



