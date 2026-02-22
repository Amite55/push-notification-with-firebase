# Push Notification Setup (Expo + Firebase)

This document explains how to set up Firebase Push Notifications in an Expo app and send notifications using the Expo push token.

---

## 1. Firebase App Configuration

1. Create a new app in the Firebase Console (iOS/Android).
2. Download the configuration file:
   - Android: `google-services.json`
   - iOS: `GoogleService-Info.plist`
3. Go to the **Credentials/Configuration** section in the Expo Dashboard and upload the JSON/PLIST file.
4. Get the **Server Key** (SHA key) from Firebase and add it to the Expo Dashboard.

---

## 2. Expo App Configuration

1. Add the following notification configuration in `app.json` or `app.config.js`:

```json
{
  "expo": {
    "name": "YourAppName",
    "slug": "your-app-slug",
    "version": "1.0.0",
    "platforms": ["ios", "android"],
    "notification": {
      "icon": "./assets/notification-icon.png",
      "color": "#6523E7"
    }
  }
}


3. EAS Build & Credentials

Upload Firebase JSON/PLIST file and Server Key to EAS Build credentials.

Run the build:

eas build
eas build --platform android
eas build --platform ios


##Push Token Verification

Use the Expo SDK to get the push token:

import * as Notifications from 'expo-notifications';
import * as Device from 'expo-device';

async function registerForPushNotificationsAsync() {
  let token;
  if (Device.isDevice) {
    const { status: existingStatus } = await Notifications.getPermissionsAsync();
    let finalStatus = existingStatus;
    if (existingStatus !== 'granted') {
      const { status } = await Notifications.requestPermissionsAsync();
      finalStatus = status;
    }
    if (finalStatus !== 'granted') {
      alert('Failed to get push token for push notifications!');
      return;
    }
    token = (await Notifications.getExpoPushTokenAsync()).data;
    console.log(token);
  } else {
    alert('Push notifications require a physical device!');
  }
  return token;
}
```



Doc link expo : https://docs.expo.dev/push-notifications/push-notifications-setup/
