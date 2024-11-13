# Question Unit 5. Persistent Data Storage
_____

### **1. Scenario: Basic Data Storage**
   - **Question**: You’re building a notes app that should save notes even after the app is closed. What type of data storage in Android should you use, and why?
   - **Answer**: You should use SQLite because it is a built-in database system that allows data to be stored persistently on the device. This way, notes will still be available when the app is reopened, even if it was previously closed.

---

### **2. Scenario: Data Sharing**
   - **Question**: You want your notes app to allow other apps on the device to access its notes. What Android feature would you use to make this possible?
   - **Answer**: You would use a Content Provider, specifically creating one in your app. A content provider allows data to be shared securely with other apps by exposing it through a unique URI. This way, only authorized apps can access the notes.

---

### **3. Scenario: Adding a New Note**
   - **Question**: You want to add a new note to your SQLite database in the notes app. How would you insert a new record, and what Java class would help you do this?
   - **Answer**: You can add a new note using the `ContentValues` class to store key-value pairs for each column in the database, like “title” and “content.” Then, you would use the `SQLiteDatabase` class's `insert()` method to insert these values into the “Notes” table. The `DatabaseHelper` class, which extends `SQLiteOpenHelper`, would help manage this process.

---

### **4. Scenario: Managing Multiple Screens**
   - **Question**: In your notes app, you want one screen to add notes and another screen to display the saved notes. How would you organize these screens in Android?
   - **Answer**: Each screen in Android can be represented by an Activity. For example, you could create one activity (AddNoteActivity) for adding notes and another (ViewNotesActivity) for displaying saved notes. You can switch between these screens using Intents.

---

### **5. Scenario: Setting Up Activity Layout**
   - **Question**: You need a screen where users can type a note title and content. Which UI components and layout would be best suited for this, and where would you define these elements?
   - **Answer**: A `LinearLayout` would be useful to organize the UI vertically. Inside the layout, use `EditText` components for the title and content fields and a `Button` for the “Save” action. These components would be defined in an XML layout file, typically called `activity_add_note.xml`.

---

### **6. Scenario: Handling Database Upgrades**
   - **Question**: You added a new feature to your notes app that requires a new column in the SQLite database. How do you ensure users’ databases are updated without losing data?
   - **Answer**: Increase the `DATABASE_VERSION` variable in your `DatabaseHelper` class. Then, override the `onUpgrade()` method to specify the SQL command for adding the new column without deleting the existing data. The `onUpgrade()` method will automatically update users’ databases to the new structure.

---

### **7. Scenario: Manifest Configuration**
   - **Question**: You created a new activity for adding notes and a content provider to share notes data. How would you configure them so they work properly?
   - **Answer**: You need to declare each component in the `AndroidManifest.xml` file. For the activity, add an `<activity>` tag with the class name. For the content provider, use a `<provider>` tag and specify the authority and other permissions if needed.

---

### **8. Scenario: Viewing Database Content on Device**
   - **Question**: After running your app, you want to see the actual SQLite database created by the app to verify that data is being stored correctly. How would you access the database file?
   - **Answer**: You can use Android Studio’s Device File Explorer to navigate to your app’s data folder on the emulator or a connected device. There, you can locate the SQLite database file under `data/data/<your.package.name>/databases/`. You can download this file and use a database viewer to inspect the contents.

---

### **9. Scenario: Retrieving Specific Notes**
   - **Question**: In your app, you want to retrieve notes that contain specific keywords in the title. What SQL command would you use in your `DatabaseHelper` class?
   - **Answer**: You would use a `SELECT` command with a `WHERE` clause that uses the `LIKE` operator to match keywords in the title. For example:
   ```java
   Cursor cursor = db.query("Notes", null, "title LIKE ?", new String[]{"%keyword%"}, null, null, null);
   ```
   This command finds notes where the title contains the specified keyword.

---

### **10. Scenario: Adding Permissions in the Manifest**
   - **Question**: Your app needs to read and write files to external storage to save large notes that might exceed SQLite limits. How would you request permission for this?
   - **Answer**: In the `AndroidManifest.xml` file, add the following permissions:
   ```xml
   <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
   ```
   Also, starting with Android 6.0, request these permissions at runtime if needed.

---

### **Scenario 1: Storing Contact Information**

**Question**: Imagine you’re setting up a small library on your phone to store contact information for friends and family. How would SQLite help you organize this contact information, and where would it be stored?

**Answer**: SQLite would act as my "librarian" to store and organize each contact’s information. I would create a table (or "bookshelf") called “Contacts” in SQLite. Each contact (like a "book" on the shelf) would be stored as a row with columns (sections) for information like the contact’s name, phone number, and email. This data would be saved locally on my device, allowing me to access it anytime without needing the internet.

---

### **Scenario 2: Finding Specific Information**

**Question**: In your library, you have hundreds of contacts saved. You need to find a friend named "John." How does SQLite help you locate this information quickly?

**Answer**: I can ask SQLite to run a query, like asking the librarian to find all books (contacts) with the name "John." SQLite would look through my "Contacts" table and fetch only the rows (contacts) where the name column matches "John." This way, I don't need to search through each contact manually, and it saves time.

---

### **Scenario 3: Updating Information**

**Question**: Suppose a friend changed their phone number, and you need to update it in your library’s contacts. How does SQLite handle this change?

**Answer**: To update the information, I’d tell SQLite to locate the specific contact (like finding the exact book on the shelf) and then change the phone number in that row. This is like having the librarian go to a specific book, cross out the old phone number, and write in the new one, so I have the latest information in my library.

---

### **Scenario 4: Deleting Information**

**Question**: You no longer need to keep a contact’s information in your library. How would you remove it using SQLite?

**Answer**: I would ask SQLite to "delete" that contact’s row in the "Contacts" table. This is similar to telling the librarian to remove that book from the shelf and make space for other books (data). The contact’s data would be permanently removed from my library.

---

### **Scenario 5: Sharing Data with Other Apps**

**Question**: You want to share your contact list with another app that needs access to your "Contacts" data. How can SQLite and Content Providers help with this?

**Answer**: I would use a content provider, which is like an interlibrary loan system that allows other apps (libraries) to borrow information safely. The content provider securely manages which specific data can be shared and accessed by the other app, so only the required information is shared without exposing the whole database.

---

### **Scenario 6: Organizing Data for a Notes App**

**Question**: You decide to create a Notes section in your library to save different notes you’ve written. What kind of structure would you set up in SQLite?

**Answer**: I would create a new table, or “bookshelf,” called "Notes" in SQLite. Each note I write would be a row in this table, with columns (sections) for information like the note’s title, content, and date created. This setup allows me to keep my notes organized and access specific notes when I need them.

---

### **Scenario 7: Handling Database Changes**

**Question**: You realize you want to add an extra column in your Contacts table to store each person’s address. How does SQLite handle this change?

**Answer**: In SQLite, I would modify the table structure by adding a new "address" column. This is like adding a new section to each book on the Contacts shelf to store the address. SQLite will use the "onUpgrade" method to update the database version, so all my contacts will have a place to store address details moving forward.

---


---

### **Scenario 1: Setting Up a Simple Notes App with SQLite**

**Question**:  
Imagine you’re building a “Notes” app where users can save text notes. How would you set up the SQLite database to store these notes? Outline the steps to create a `Notes` table with columns `id`, `title`, and `content`.

**Answer**:  
1. **Create a Database Helper Class**:
   - Extend `SQLiteOpenHelper` and define the `Notes` table with columns for `id` (INTEGER and PRIMARY KEY), `title` (TEXT), and `content` (TEXT).

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

2. **Instantiate Database Helper** in your activity to initialize and manage the database.
3. Now, you can use this helper to perform CRUD operations on the `Notes` table.

---

### **Scenario 2: Using Content Providers to Access Contacts**

**Question**:  
Your app needs to access the device’s contacts to fetch and display contact names in a list. How would you set up an Android Content Provider to fetch contact names from the device’s contact list?

**Answer**:  
1. **Use the Contacts Content Provider**: The Android system provides a built-in content provider for accessing contacts.
   
2. **Query the Contacts Content Provider**:
   ```java
   Cursor cursor = getContentResolver().query(
       ContactsContract.CommonDataKinds.Phone.CONTENT_URI,
       new String[]{ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME},
       null, null, null);

   if (cursor != null) {
       while (cursor.moveToNext()) {
           String name = cursor.getString(
               cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
           Log.d("Contact Name", name);
       }
       cursor.close();
   }
   ```
3. **Permissions**: Request `READ_CONTACTS` permission in `AndroidManifest.xml`.

---

### **Scenario 3: Updating Notes in SQLite**

**Question**:  
Suppose your Notes app allows users to edit an existing note’s title. How would you implement a function to update the note’s title by its `id`?

**Answer**:  
1. **Define an Update Method** in `DatabaseHelper`:
   ```java
   public void updateNoteTitle(int id, String newTitle) {
       SQLiteDatabase db = this.getWritableDatabase();
       ContentValues values = new ContentValues();
       values.put("title", newTitle);
       db.update("Notes", values, "id=?", new String[]{String.valueOf(id)});
       db.close();
   }
   ```

2. **Call the Update Method** from your activity when the user updates the note title. Pass in the `id` of the note and the new title text.

---

### **Scenario 4: Creating and Registering a New Activity**

**Question**:  
You need a new activity in your Notes app where users can add a new note. What are the steps to create this activity, including setting up the layout file and registering it in the manifest?

**Answer**:  
1. **Create a Layout for the Add Note Activity** (`activity_add_note.xml`):
   ```xml
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">

       <EditText android:id="@+id/titleEditText"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:hint="Title" />

       <EditText android:id="@+id/contentEditText"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:hint="Content" />

       <Button android:id="@+id/saveButton"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="Save" />
   </LinearLayout>
   ```

2. **Create the Java Activity Class**:
   ```java
   public class AddNoteActivity extends AppCompatActivity {
       private DatabaseHelper dbHelper;

       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_add_note);

           dbHelper = new DatabaseHelper(this);

           findViewById(R.id.saveButton).setOnClickListener(view -> {
               String title = ((EditText) findViewById(R.id.titleEditText)).getText().toString();
               String content = ((EditText) findViewById(R.id.contentEditText)).getText().toString();
               addNoteToDatabase(title, content);
           });
       }

       private void addNoteToDatabase(String title, String content) {
           ContentValues values = new ContentValues();
           values.put("title", title);
           values.put("content", content);
           dbHelper.getWritableDatabase().insert("Notes", null, values);
       }
   }
   ```

3. **Register the Activity in `AndroidManifest.xml`**:
   ```xml
   <activity android:name=".AddNoteActivity" />
   ```

---

### **Scenario 5: Configuring the Manifest for Database Permissions**

**Question**:  
You want to save a backup of your SQLite database to external storage. What steps do you need to configure in `AndroidManifest.xml` for this to work?

**Answer**:  
1. **Add Permissions for External Storage**:
   ```xml
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
   ```

2. **Handle Permissions in Your Activity** (for Android 6.0+):
   - Request permissions at runtime if they aren’t already granted.

---

### **Scenario 6: Exporting and Managing SQLite Database Files**

**Question**:  
You want to allow users to back up and restore their notes from the SQLite database. How would you copy the database file from the app’s internal storage to external storage?

**Answer**:  
1. **Export Database File to External Storage**:
   ```java
   public void backupDatabase() {
       try {
           File dbFile = new File(getDatabasePath("notes_db").getAbsolutePath());
           FileInputStream fis = new FileInputStream(dbFile);
           File backupDir = new File(Environment.getExternalStorageDirectory(), "NotesBackup");
           if (!backupDir.exists()) {
               backupDir.mkdirs();
           }
           File backupFile = new File(backupDir, "backup_notes.db");

           FileOutputStream fos = new FileOutputStream(backupFile);
           byte[] buffer = new byte[1024];
           int length;
           while ((length = fis.read(buffer)) > 0) {
               fos.write(buffer, 0, length);
           }
           fos.flush();
           fos.close();
           fis.close();
       } catch (IOException e) {
           e.printStackTrace();
       }
   }
   ```

2. **Request Storage Permissions** as in Scenario 5.

3. **Call `backupDatabase()`** when the user wants to export data.

---
