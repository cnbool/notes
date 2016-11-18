title: android开发学习笔记(二)
id: 898
categories:
  - Android
date: 2013-05-28 08:09:17
tags:
---

# Building a Simple User Interface

<!--more-->

## Create a Linear Layout

Open the `activity_main.xml` file from the `res/layout/` directory.
<pre><?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal" >
</LinearLayout></pre>
`[LinearLayout](http://developer.android.com/reference/android/widget/LinearLayout.html)` is a view group (a subclass of `[ViewGroup](http://developer.android.com/reference/android/view/ViewGroup.html)`) that lays out child views in either a vertical or horizontal orientation, as specified by the [`android:orientation`](http://developer.android.com/reference/android/widget/LinearLayout.html#attr_android:orientation) attribute. Each child of a `[LinearLayout](http://developer.android.com/reference/android/widget/LinearLayout.html)` appears on the screen in the order in which it appears in the XML.

The other two attributes, [`android:layout_width`](http://developer.android.com/reference/android/view/View.html#attr_android:layout_width) and [`android:layout_height`](http://developer.android.com/reference/android/view/View.html#attr_android:layout_height), are required for all views in order to specify their size.

Because the `[LinearLayout](http://developer.android.com/reference/android/widget/LinearLayout.html)` is the root view in the layout, it should fill the entire screen area that's available to the app by setting the width and height to `"match_parent"`. This value declares that the view should expand its width or height to _match_ the width or height of the parent view.

## Add a Text Field

To create a user-editable text field, add an `[<EditText>](http://developer.android.com/reference/android/widget/EditText.html)` element inside the `[<LinearLayout>](http://developer.android.com/reference/android/widget/LinearLayout.html)`.
<pre><EditText android:id="@+id/edit_message"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="@string/edit_message" /></pre>
[`android:id`](http://developer.android.com/reference/android/view/View.html#attr_android:id)
This provides a unique identifier for the view, which you can use to reference the object from your app code, such as to read and manipulate the object.
The plus sign (`+`) before the resource type is needed only when you're defining a resource ID for the first time.

[`android:layout_width`](http://developer.android.com/reference/android/view/View.html#attr_android:layout_width) and [`android:layout_height`](http://developer.android.com/reference/android/view/View.html#attr_android:layout_height)
Instead of using specific sizes for the width and height, the`"wrap_content"` value specifies that the view should be only as big as needed to fit the contents of the view.

[`android:hint`](http://developer.android.com/reference/android/widget/TextView.html#attr_android:hint)
This is a default string to display when the text field is empty. Instead of using a hard-coded string as the value, the `"@string/edit_message"` value refers to a string resource defined in a separate file. Because this refers to a concrete resource (not just an identifier), it does not need the plus sign.

## Add String Resources

When you need to add text in the user interface, you should always specify each string as a resource. String resources allow you to manage all UI text in a single location, which makes it easier to find and update text.

**res/values/strings.xml**
<pre><?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">My First App</string>
    <string name="edit_message">Enter a message</string>
    <string name="button_send">Send</string>
    <string name="action_settings">Settings</string>
    <string name="title_activity_main">MainActivity</string>
</resources></pre>

## Add a Button

Now add a `[<Button>](http://developer.android.com/reference/android/widget/Button.html)` to the layout, immediately following the `[<EditText>](http://developer.android.com/reference/android/widget/EditText.html)` element:
<pre><Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_send" /></pre>
This button doesn't need the [`android:id`](http://developer.android.com/reference/android/view/View.html#attr_android:id) attribute, because it won't be referenced from the activity code.
<pre><EditText android:id="@+id/edit_message"
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/edit_message" /></pre>
**The weight** value is a number that specifies the amount of remaining space each view should consume, relative to the amount consumed by sibling views.
if you give one view a weight of 2 and another one a weight of 1, the sum is 3, so the first view fills 2/3 of the remaining space and the second view fills the rest.
In order to improve the layout efficiency when you specify the weight, you should change the width of the`[EditText](http://developer.android.com/reference/android/widget/EditText.html)` to be zero (0dp).
Here’s how your complete layout file should now look:
<pre><?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <EditText android:id="@+id/edit_message"
        android:layout_weight="1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="@string/edit_message" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_send" />
</LinearLayout></pre>

# Starting Another Activity

## Respond to the Send Button

open the`activity_main.xml` layout file and add the [`android:onClick`](http://developer.android.com/reference/android/view/View.html#attr_android:onClick)attribute to the `[<Button>](http://developer.android.com/reference/android/widget/Button.html)` element:
<pre><Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/button_send"
    android:onClick="sendMessage" /></pre>
The [`android:onClick`](http://developer.android.com/reference/android/view/View.html#attr_android:onClick) attribute’s value, `"sendMessage"`, is the name of a method in your activity that the system calls when the user clicks the button.
Open the `MainActivity` class (located in the project's `src/` directory) and add the corresponding method:
<pre>/** Called when the user clicks the Send button */
public void sendMessage(View view) {
    // Do something in response to button
}</pre>
the method must:

*   Be public
*   Have a void return value
*   Have a `[View](http://developer.android.com/reference/android/view/View.html)` as the only parameter (this will be the `[View](http://developer.android.com/reference/android/view/View.html)` that was clicked)
<div>

## Build an Intent

An `[Intent](http://developer.android.com/reference/android/content/Intent.html)` is an object that provides runtime binding between separate components (such as two activities).
Inside the `sendMessage()` method, create an `[Intent](http://developer.android.com/reference/android/content/Intent.html)` to start an activity called `DisplayMessageActivity`:
<pre>Intent intent = new Intent(this, DisplayMessageActivity.class);</pre>
The constructor used here takes two parameters:

*   A `[Context](http://developer.android.com/reference/android/content/Context.html)` as its first parameter (`this` is used because the `[Activity](http://developer.android.com/reference/android/app/Activity.html)` class is a subclass of `[Context](http://developer.android.com/reference/android/content/Context.html)`)
*   The `[Class](http://developer.android.com/reference/java/lang/Class.html)` of the app component to which the system should deliver the `[Intent](http://developer.android.com/reference/android/content/Intent.html)` (in this case, the activity that should be started)
An intent not only allows you to start another activity, but it can carry a bundle of data to the activity as well.
<pre>public class MainActivity extends Activity {
    public final static String EXTRA_MESSAGE = "com.example.myfirstapp.MESSAGE";
    ...
}

Intent intent = new Intent(this, DisplayMessageActivity.class);
EditText editText = (EditText) findViewById(R.id.edit_message);
String message = editText.getText().toString();
intent.putExtra(EXTRA_MESSAGE, message);</pre>

## Start the Second Activity

To start an activity, call `[startActivity()](http://developer.android.com/reference/android/app/Activity.html#startActivity(android.content.Intent))` and pass it your `[Intent](http://developer.android.com/reference/android/content/Intent.html)`. The system receives this call and starts an instance of the `[Activity](http://developer.android.com/reference/android/app/Activity.html)` specified by the `[Intent](http://developer.android.com/reference/android/content/Intent.html)`.
<pre>/** Called when the user clicks the Send button */
public void sendMessage(View view) {
    Intent intent = new Intent(this, DisplayMessageActivity.class);
    EditText editText = (EditText) findViewById(R.id.edit_message);
    String message = editText.getText().toString();
    intent.putExtra(EXTRA_MESSAGE, message);
    startActivity(intent);
}</pre>

## Create the Second Activity

Click **New**
select **Android Activity**. Click **Next**.
Select **BlankActivity** and click **Next**.
Fill in the activity details:

*   **Project**: MyFirstApp
*   **Activity Name**: DisplayMessageActivity
*   **Layout Name**: activity_display_message
*   **Title**: My Message
*   **Hierarchial Parent**: com.example.myfirstapp.MainActivity
*   **Navigation Type**: None
Click **Finish**.

### Add the title string

add the new activity's title to the `strings.xml` file:
<pre><resources>
    ...
    <string name="title_activity_display_message">My Message</string>
</resources></pre>

### Add it to the manifest

All activities must be declared in your manifest file, `AndroidManifest.xml`, using an [`<activity>`](http://developer.android.com/guide/topics/manifest/activity-element.html) element.
<pre><application ... >
    ...
    <activity
        android:name="com.example.myfirstapp.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
</application></pre>

## Receive the Intent

You can get the `[Intent](http://developer.android.com/reference/android/content/Intent.html)` that started your activity by calling `[getIntent()](http://developer.android.com/reference/android/app/Activity.html#getIntent())` and retrieve the data contained within it.

## Display the Message

create a `[TextView](http://developer.android.com/reference/android/widget/TextView.html)` widget and set the text using `[setText()](http://developer.android.com/reference/android/widget/TextView.html#setText(char[], int, int))`. Then add the`[TextView](http://developer.android.com/reference/android/widget/TextView.html)` as the root view of the activity’s layout by passing it to `[setContentView()](http://developer.android.com/reference/android/app/Activity.html#setContentView(android.view.View))`.
The complete `[onCreate()](http://developer.android.com/reference/android/app/Activity.html#onCreate(android.os.Bundle))` method for `DisplayMessageActivity` now looks like this:
<pre>@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Get the message from the intent
    Intent intent = getIntent();
    String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE);

    // Create the text view
    TextView textView = new TextView(this);
    textView.setTextSize(40);
    textView.setText(message);

    // Set the text view as the activity layout
    setContentView(textView);
}
</pre>
You can now run the app. When it opens, type a message in the text field, click Send, and the message appears on the second activity.
</div>
