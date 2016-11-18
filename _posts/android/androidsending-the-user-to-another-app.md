title: 'Android:Interacting with Other Apps'
id: 933
categories:
  - Android
date: 2013-07-03 22:07:51
tags:
---

# 1.Sending the User to Another App

This lesson shows you how to create an implicit intent for a particular action, and how to use it to start an activity that performs the action in another app.
<!--more-->

## Build an Implicit Intent

* * *

Implicit intents do not declare the class name of the component to start, but instead declare an action to perform.

The action specifies the thing you want to do, such as _view_, _edit_, _send_, or _get_ something.
<pre>Uri number = Uri.parse("tel:5551234");
Intent callIntent = new Intent(Intent.ACTION_DIAL, number);</pre>
View a map:
<pre>// Map point based on address
Uri location = Uri.parse("geo:0,0?q=1600+Amphitheatre+Parkway,+Mountain+View,+California");
// Or map point based on latitude/longitude
// Uri location = Uri.parse("geo:37.422219,-122.08364?z=14"); // z param is zoom level
Intent mapIntent = new Intent(Intent.ACTION_VIEW, location);</pre>
View a web page:
<pre>Uri webpage = Uri.parse("http://www.android.com");
Intent webIntent = new Intent(Intent.ACTION_VIEW, webpage);</pre>
Here are some more intents that add extra data to specify the desired action:

Send an email with an attachment:
<pre>Intent emailIntent = new Intent(Intent.ACTION_SEND);
// The intent does not have a URI, so declare the "text/plain" MIME type
emailIntent.setType(HTTP.PLAIN_TEXT_TYPE);
emailIntent.putExtra(Intent.EXTRA_EMAIL, new String[] {"jon@example.com"}); // recipients
emailIntent.putExtra(Intent.EXTRA_SUBJECT, "Email subject");
emailIntent.putExtra(Intent.EXTRA_TEXT, "Email message text");
emailIntent.putExtra(Intent.EXTRA_STREAM, Uri.parse("content://path/to/email/attachment"));
// You can also attach multiple items by passing an ArrayList of Uris</pre>
Create a calendar event:
<pre>Intent calendarIntent = new Intent(Intent.ACTION_INSERT, Events.CONTENT_URI);
Calendar beginTime = Calendar.getInstance().set(2012, 0, 19, 7, 30);
Calendar endTime = Calendar.getInstance().set(2012, 0, 19, 10, 30);
calendarIntent.putExtra(CalendarContract.EXTRA_EVENT_BEGIN_TIME, beginTime.getTimeInMillis());
calendarIntent.putExtra(CalendarContract.EXTRA_EVENT_END_TIME, endTime.getTimeInMillis());
calendarIntent.putExtra(Events.TITLE, "Ninja class");
calendarIntent.putExtra(Events.EVENT_LOCATION, "Secret dojo");
**Note:** This intent for a calendar event is supported only with API level 14 and higher.</pre>

## Verify There is an App to Receive the Intent

* * *

<pre>PackageManager packageManager = `[getPackageManager()](http://developer.android.com/reference/android/content/Context.html#getPackageManager%28%29)`;
List<ResolveInfo> activities = packageManager.queryIntentActivities(intent, 0);
boolean isIntentSafe = activities.size() > 0;</pre>
**complete example:**
<pre>// Build the intent
Uri location = Uri.parse("geo:0,0?q=1600+Amphitheatre+Parkway,+Mountain+View,+California");
Intent mapIntent = new Intent(Intent.ACTION_VIEW, location);

// Verify it resolves
PackageManager packageManager = `[getPackageManager()](http://developer.android.com/reference/android/content/Context.html#getPackageManager%28%29)`;
List<ResolveInfo> activities = packageManager.queryIntentActivities(mapIntent, 0);
boolean isIntentSafe = activities.size() > 0;

// Start an activity if it's safe
if (isIntentSafe) {
    startActivity(mapIntent);
}</pre>

## Show an App Chooser

* * *

if the action to be performed could be handled by multiple apps and the user might prefer a different app each time—such as a "share" action, for which users might have several apps through which they might share an item—you should explicitly show a chooser dialog, which forces the user to select which app to use for the action every time (the user cannot select a default app for the action).
<pre>Intent intent = new Intent(Intent.ACTION_SEND);
...

// Always use string resources for UI text. This says something like "Share this photo with"
String title = getResources().getText(R.string.chooser_title);
// Create and start the chooser
Intent chooser = Intent.createChooser(intent, title);
startActivity(chooser);</pre>
<div>
<div>

# 2.Getting a Result from an Activity

## Start the Activity

* * *

<pre>static final int PICK_CONTACT_REQUEST = 1;  // The request code
...
private void pickContact() {
    Intent pickContactIntent = new Intent(Intent.ACTION_PICK, Uri.parse("content://contacts"));
    pickContactIntent.setType(Phone.CONTENT_TYPE); // Show user only contacts w/ phone numbers
    startActivityForResult(pickContactIntent, PICK_CONTACT_REQUEST);
}</pre>

## Receive the Result

* * *

*   The request code you passed to `[startActivityForResult()](http://developer.android.com/reference/android/app/Activity.html#startActivityForResult%28android.content.Intent,%20int%29)`.
*   A result code specified by the second activity. This is either `[RESULT_OK](http://developer.android.com/reference/android/app/Activity.html#RESULT_OK)` if the operation was successful or `[RESULT_CANCELED](http://developer.android.com/reference/android/app/Activity.html#RESULT_CANCELED)` if the user backed out or the operation failed for some reason.
*   An `[Intent](http://developer.android.com/reference/android/content/Intent.html)` that carries the result data.
<pre>@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // Check which request it is that we're responding to
    if (requestCode == PICK_CONTACT_REQUEST) {
        // Make sure the request was successful
        if (resultCode == RESULT_OK) {
            // Get the URI that points to the selected contact
            Uri contactUri = data.getData();
            // We only need the NUMBER column, because there will be only one row in the result
            String[] projection = {Phone.NUMBER};

            // Perform the query on the contact to get the NUMBER column
            // We don't need a selection or sort order (there's only one result for the given URI)
            // CAUTION: The query() method should be called from a separate thread to avoid blocking
            // your app's UI thread. (For simplicity of the sample, this code doesn't do that.)
            // Consider using `[CursorLoader](http://developer.android.com/reference/android/content/CursorLoader.html)` to perform the query.
            Cursor cursor = getContentResolver()
                    .query(contactUri, projection, null, null, null);
            cursor.moveToFirst();

            // Retrieve the phone number from the NUMBER column
            int column = cursor.getColumnIndex(Phone.NUMBER);
            String number = cursor.getString(column);

            // Do something with the phone number...
        }
    }
}</pre>

# 3.Allowing Other Apps to Start Your Activity

## Add an Intent Filter

* * *

<dl><dt>Action</dt><dd>A string naming the action to perform. Usually one of the platform-defined values such as `[ACTION_SEND](http://developer.android.com/reference/android/content/Intent.html#ACTION_SEND)` or `[ACTION_VIEW](http://developer.android.com/reference/android/content/Intent.html#ACTION_VIEW)`.</dd><dt>Data</dt><dd>A description of the data associated with the intent.</dd></dl>For example, here's an activity with an intent filter that handles the `[ACTION_SEND](http://developer.android.com/reference/android/content/Intent.html#ACTION_SEND)` intent when the data type is either text or an image:
<pre><activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
        <data android:mimeType="image/*"/>
    </intent-filter>
</activity></pre>
suppose your activity handles both text and images for both the `[ACTION_SEND](http://developer.android.com/reference/android/content/Intent.html#ACTION_SEND)` and `[ACTION_SENDTO](http://developer.android.com/reference/android/content/Intent.html#ACTION_SENDTO)` intents. In this case, you must define two separate intent filters for the two actions because a `[ACTION_SENDTO](http://developer.android.com/reference/android/content/Intent.html#ACTION_SENDTO)` intent must use the data `[Uri](http://developer.android.com/reference/android/net/Uri.html)` to specify the recipient's address using the `send` or `sendto` URI scheme. For example:
<pre><activity android:name="ShareActivity">
    <!-- filter for sending text; accepts SENDTO action with sms URI schemes -->
    <intent-filter>
        <action android:name="android.intent.action.SENDTO"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:scheme="sms" />
        <data android:scheme="smsto" />
    </intent-filter>
    <!-- filter for sending text or images; accepts SEND action and text or image data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="image/*"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity></pre>

## Handle the Intent in Your Activity

* * *

<pre>@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.main);

    // Get the intent that started this activity
    Intent intent = getIntent();
    Uri data = intent.getData();

    // Figure out what to do based on the intent type
    if (intent.getType().indexOf("image/") != -1) {
        // Handle intents with image data ...
    } else if (intent.getType().equals("text/plain")) {
        // Handle intents with text ...
    }
}</pre>

## Return a Result

* * *

If you want to return a result to the activity that invoked yours, simply call `[setResult()](http://developer.android.com/reference/android/app/Activity.html#setResult%28int,%20android.content.Intent%29)` to specify the result code and result `[Intent](http://developer.android.com/reference/android/content/Intent.html)`.
<pre>// Create intent to deliver some kind of result data
Intent result = new Intent("com.example.RESULT_ACTION", Uri.parse("content://result_uri");
setResult(Activity.RESULT_OK, result);
finish();</pre>
If you simply need to return an integer that indicates one of several result options, you can set the result code to any value higher than 0\. If you use the result code to deliver an integer and you have no need to include the `[Intent](http://developer.android.com/reference/android/content/Intent.html)`, you can call `[setResult()](http://developer.android.com/reference/android/app/Activity.html#setResult%28int%29)` and pass only a result code. For example:
<pre>setResult(RESULT_COLOR_RED);
finish();</pre>
</div>
</div>
