# android-sdk

## Installation

Installation of the android SDK is very straightforward.

- Download the SDK and unzip it.
- Add the eyesquare-android-sdk.aar file to your project.

The sdk requires that you enable Java 8 in your builds. You can learn more about how to enable this at https://developer.android.com/studio/write/java8-support. Essentially adding the following to your application's `build.gradle`:

```
android {
  ...
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
```

### Supported versions

Android 4.4 or higher

## Usage

A task flow is started launching a `TaskActivity` instance through [startActivityForResult](https://developer.android.com/reference/android/app/Activity.html#startActivityForResult(android.content.Intent,%20int)). The parameters ProjectID, SubjectGroupID and UserHash are used to identify the subject and define the experimental variation. These should be passed to the `TaskActivity` intent using [putExtra](https://developer.android.com/reference/android/content/Intent.html#putExtra(java.lang.String,%20android.os.Parcelable[])) before calling [startActivityForResult](https://developer.android.com/reference/android/app/Activity.html#startActivityForResult(android.content.Intent,%20int)):

### possible success values:

success is an integer and represents the result of the task.

* -1: a technical error occured, the subject couldn't start/get through the survey. This is often caused by wrongly passed ProjectIds and or SubjectGroupIds
* 0: the success criteria of the ad/subjectGroup wasn't met
* 1: the success criteria of the ad/subjectGroup was met
* 2: the user canceled the run early

```java
import com.eyesquare.sdk.TaskActivity;
...
static final int TASK_REQUEST = 1;
...
Intent intent = new Intent(MainActivity.this, TaskActivity.class);

intent.putExtra("ProjectID",      "<projectId>");
intent.putExtra("SubjectGroupID", "<groupId>");
intent.putExtra("UserHash",       "<userHash>");

MainActivity.this.startActivityForResult(intent, TASK_REQUEST);
```

### Get the task results

Once the flow is completed, you can be notified and get the flow result  by overriding the `onActivityResult` method on the calling activity.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {

	if (requestCode == TASK_REQUEST && resultCode == RESULT_OK) {
		// use the return intent to get information about the flow
		int result = data.getIntExtra("success", 0);
	}
}
```
