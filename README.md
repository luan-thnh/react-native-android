# React Native with Expo Setup Guide

## 1. Prerequisites
- **Node.js** (LTS version recommended)
- A text editor like **Visual Studio Code**

---

## 2. Create a New Expo Project
1. Open your terminal and run:
   ```bash
   npx create-expo-app my-app
   ```
2. Follow the prompts to select a project template (e.g., "Blank").
3. Navigate to your project folder:
   ```bash
   cd my-app
   ```

### Project Structure Overview
After initialization, your project will look like this:
```
my-app/
├── app/
    ├── _layout.tsx
```
Expo uses an **automatic routing system** for navigation, making it easier to manage screens (placed under `app/`).

---

## 3. Add WebView
To embed a website in your app:

1. Install the WebView package:
   ```bash
   npm install react-native-webview
   ```

2. Add a `_layout.tsx` file in the `app/` directory:
   ```javascript
   import React from 'react';
   import { View, StyleSheet } from 'react-native';
   import { WebView } from 'react-native-webview';

   export default function HomeScreen() {
     return (
       <View style={styles.container}>
         <WebView source={{ uri: 'https://yourwebsite.com' }} />
       </View>
     );
   }

   const styles = StyleSheet.create({
     container: {
       flex: 1,
     },
   });
   ```

3. Ensure routing works:
   Expo's automatic routing will detect your `HomeScreen` component if placed under `app/`. Start the app to verify it works:
   ```bash
   npm start
   ```

---

## 4. Build for Android
1. Log in to your Expo account:
   ```bash
   npx expo login
   ```

2. Build the app:
   ```bash
   npx expo build:android
   ```
   Follow the prompts to build either an **APK** or an **AAB** file.

3. The generated file will be available on Expo's build service, and a download link will be provided.

---

## 5. Convert AAB to APK
If you need an APK file:

1. Ensure you have **Java Development Kit (JDK)** installed.
2. Download `bundletool`:
   ```bash
   wget https://github.com/google/bundletool/releases/download/1.17.2/bundletool-all-1.17.2.jar
   ```
3. Run this command to convert the AAB:
   ```bash
   java -jar bundletool-all-1.17.2.jar build-apks \
     --bundle=path/to/your-app.aab \
     --output=path/to/output.apks \
     --mode=universal \
     --ks=path/to/keystore.jks \
     --ks-key-alias=your-key-alias \
     --ks-pass=pass:your-password
   ```
4. Extract the APK:
   ```bash
   unzip path/to/output.apks -d output-folder
   ```

---

## 6. Creating and Using Keystore for Signing Android Apps
A keystore is required to sign your app for production builds.

1. **Generate a Keystore**:
   Run the following command in your terminal to generate the keystore:
   ```bash
   keytool -genkeypair -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-key-alias
   ```
   Follow the prompts to set the keystore password and other details.

2. **Store Keystore Information**:
   Make sure to save the following information securely:
   - Keystore location
   - Keystore password
   - Key alias
   - Key password

---
## 7. Deploy APK on Android Devices
Transfer the APK file to your Android phone and install it manually. Enable "Install unknown apps" in your device settings if needed.
