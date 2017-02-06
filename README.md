# android-sdk

## Installation

Installation of the android SDK is very straightforward.

- Download the SDK and unzip it.
- Add the android-sdk-0.x.x.aar file to your project.

### Supported versions

Android 4.4 or higher

## Usage

### Start a task

A task flow is started launching a `TaskActivity` instance through [startActivityForResult](https://developer.android.com/reference/android/app/Activity.html#startActivityForResult(android.content.Intent, int)). The parameters ProjectID, SubjectGroupID and UserHash are used to identify the subject and define the experimental variation. These should be passed to the `TaskActivity` intent using [putExtra](https://developer.android.com/reference/android/content/Intent.html#putExtra(java.lang.String, android.os.Parcelable[])) before calling [startActivityForResult](https://developer.android.com/reference/android/app/Activity.html#startActivityForResult(android.content.Intent, int)):


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
		Boolean result = data.getBooleanExtra("success", false);
	}
}
```
