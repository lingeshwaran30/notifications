# 8. Develop an Android Application Implementing Notifications Using Notification Manager

[← Back to Index](README.md)

## Subject: Services, Notifications and Broadcast Receivers

## Project Name
`NotificationDemo`

## Aim
To develop an Android application that displays notifications using `NotificationManager` and `NotificationCompat`.

## Introduction

Notifications are messages displayed by Android outside an application's normal user interface. They can inform users about events, updates, or other important information.

In this application, a button is used to display a notification. The application creates a notification channel for Android 8.0 and later and requests notification permission on Android 13 and later.

## 1. File Name: `activity_main.xml`

**File Path:** `app/src/main/res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <Button
        android:id="@+id/btnNotify"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show Notification" />

</LinearLayout>
```

## 2. File Name: `AndroidManifest.xml`

**File Path:** `app/src/main/AndroidManifest.xml`

Add the notification permission **above the `<application>` element**:

```xml
<uses-permission
    android:name="android.permission.POST_NOTIFICATIONS" />
```

Keep your existing `<application>` and activity declarations unchanged.

## 3. File Name: `MainActivity.java`

**File Path:** `app/src/main/java/com/example/notificationdemo/MainActivity.java`

```java
package com.example.notificationdemo;

import android.Manifest;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.content.pm.PackageManager;
import android.os.Build;
import android.os.Bundle;
import android.widget.Button;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.NotificationCompat;

public class MainActivity extends AppCompatActivity {

    private static final String CHANNEL_ID = "MyChannel";
    private static final int NOTIFICATION_PERMISSION_CODE = 100;

    private Button btnNotify;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Find the button from activity_main.xml
        btnNotify = findViewById(R.id.btnNotify);

        // Create notification channel
        createNotificationChannel();

        // Request notification permission for Android 13+
        requestNotificationPermission();

        // Button click
        btnNotify.setOnClickListener(v -> {
            showNotification();
        });
    }

    // Request notification permission
    private void requestNotificationPermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {

            if (checkSelfPermission(
                    Manifest.permission.POST_NOTIFICATIONS)
                    != PackageManager.PERMISSION_GRANTED) {

                requestPermissions(
                        new String[]{
                                Manifest.permission.POST_NOTIFICATIONS
                        },
                        NOTIFICATION_PERMISSION_CODE
                );
            }
        }
    }

    // Create notification channel
    private void createNotificationChannel() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {

            NotificationChannel channel =
                    new NotificationChannel(
                            CHANNEL_ID,
                            "My Channel",
                            NotificationManager.IMPORTANCE_DEFAULT
                    );

            channel.setDescription(
                    "Notification Channel for Demo"
            );

            NotificationManager manager =
                    getSystemService(NotificationManager.class);

            if (manager != null) {
                manager.createNotificationChannel(channel);
            }
        }
    }

    // Show notification
    private void showNotification() {

        // Check permission for Android 13+
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {

            if (checkSelfPermission(
                    Manifest.permission.POST_NOTIFICATIONS)
                    != PackageManager.PERMISSION_GRANTED) {

                requestNotificationPermission();
                return;
            }
        }

        NotificationCompat.Builder builder =
                new NotificationCompat.Builder(this, CHANNEL_ID)
                        .setSmallIcon(
                                android.R.drawable.ic_dialog_info
                        )
                        .setContentTitle("Notification Example")
                        .setContentText(
                                "Hello from Android Notification"
                        )
                        .setPriority(
                                NotificationCompat.PRIORITY_DEFAULT
                        )
                        .setAutoCancel(true);

        NotificationManager manager =
                (NotificationManager) getSystemService(
                        NOTIFICATION_SERVICE
                );

        if (manager != null) {
            manager.notify(1, builder.build());
        }
    }
}
```

**Important:** Ensure that the package name `com.example.notificationdemo` matches your Android Studio project's package name.

## 4. Explanation

### NotificationManager
- `NotificationManager` is responsible for posting and managing notifications.
- `manager.notify(1, builder.build())` displays the notification using ID `1`.

### Notification Channel
- Android 8.0 (API 26) and later require notifications to use a notification channel.
- `createNotificationChannel()` creates a channel named **My Channel**.
- The channel description is `Notification Channel for Demo`.

### Notification Permission
- Android 13 (API 33) and later require runtime permission for notifications in typical app installations.
- The application requests `POST_NOTIFICATIONS` if permission has not already been granted.
- If permission is denied, the notification will not be displayed.

### NotificationCompat.Builder
The builder configures the notification:

- `setSmallIcon()` sets the notification icon.
- `setContentTitle()` sets the title.
- `setContentText()` sets the message.
- `setPriority()` sets the priority for compatible Android versions.
- `setAutoCancel(true)` allows the notification to be dismissed when tapped.

## 5. Expected Output

1. The application displays a **Show Notification** button.
2. On Android 13 and later, the application requests notification permission if necessary.
3. When the user clicks the button after granting permission, a notification appears.
4. The notification displays the title **Notification Example**.
5. The notification message is **Hello from Android Notification**.
6. The user can dismiss the notification by tapping it.

## 6. Result

Thus, an Android application implementing notifications using `NotificationManager` and `NotificationCompat` was developed successfully.

## 7. Important Notes

- Ensure that `POST_NOTIFICATIONS` is declared above the `<application>` element in `AndroidManifest.xml`.
- Notification permission must be granted on Android 13 and later.
- Notifications may also be disabled by the user in the device's system settings.
- If your project does not already include AndroidX Core, add the appropriate `androidx.core:core` dependency to your app-level Gradle file.
- The sample uses `android.R.drawable.ic_dialog_info` as a basic notification icon.

## 8. Upload the Project to GitHub

1. Open your Android Studio project folder.
2. Create or update the `README.md` file with this content.
3. Save the file.
4. Open your GitHub repository.
5. Commit and push your changes.

### Git Commands

```bash
git add README.md
git commit -m "Add Notification Manager practical"
git push
```
