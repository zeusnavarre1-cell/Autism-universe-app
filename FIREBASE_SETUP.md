# Firebase Configuration Guide - El Mundo de Juanito

## 🔧 Step 1: Create Firebase Project

1. Go to https://console.firebase.google.com
2. Click "Create a new project"
3. Project name: `autism-universe`
4. Accept terms and continue
5. Disable Google Analytics (optional)
6. Click "Create project"

## 📱 Step 2: Android Configuration

### 2.1 Register Android App

1. In Firebase Console, click "Add app" → Android
2. **Android package name**: `com.elmundodejuanito.autismuniverse`
3. **App nickname** (optional): `Autism Universe Android`
4. Click "Register app"

### 2.2 Download Configuration

1. Download `google-services.json`
2. Place it in: `android/app/google-services.json`

### 2.3 Add Android dependencies

The `android/build.gradle` and `android/app/build.gradle` are already configured.

## 🍎 Step 3: iOS Configuration

### 3.1 Register iOS App

1. In Firebase Console, click "Add app" → iOS
2. **iOS bundle ID**: `com.elmundodejuanito.autismuniverse`
3. **App nickname** (optional): `Autism Universe iOS`
4. Click "Register app"

### 3.2 Download Configuration

1. Download `GoogleService-Info.plist`
2. Open Xcode: `open ios/Runner.xcworkspace`
3. Drag `GoogleService-Info.plist` into Xcode
4. Make sure it's added to both Target and Runner

## 🔐 Step 4: Enable Authentication

1. In Firebase Console, go to "Authentication"
2. Click "Get Started"
3. Enable **Email/Password**
4. Enable **Google**
5. Add your test emails if needed

## 📊 Step 5: Setup Database

1. Go to "Firestore Database"
2. Click "Create Database"
3. Start in **Test Mode** (development only)
4. Choose location: `us-central1`
5. Click "Enable"

### Create Collections

```
users/
  {userId}/
    - name: string
    - email: string
    - avatarUrl: string
    - age: number
    - createdAt: timestamp
    - preferences: object

rewards/
  {userId}/
    - balance: number
    - totalEarned: number
    - rewards: array

routines/
  {userId}/
    - routines: array
    
emotions/
  {userId}/
    - logs: array
```

## 🔑 Step 6: Security Rules

Go to Firestore → Rules and paste:

```firebase
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
    match /rewards/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
    match /routines/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
    match /emotions/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

## 🚀 Step 7: Update Flutter Code

The `lib/firebase_options.dart` file needs your actual Firebase credentials.

Run:
```bash
flutterfire configure
```

This will automatically update your Firebase configuration.

## ✅ Verification

1. Run the app: `flutter run`
2. Try to login/register
3. Check Firebase Console for new users
4. Verify database entries

## 📖 Useful Links

- [Firebase Console](https://console.firebase.google.com)
- [Flutter Firebase Docs](https://firebase.flutter.dev)
- [Firebase Security Rules](https://firebase.google.com/docs/rules)

## 🛟 Troubleshooting

### Android Build Issues
```bash
flutter clean
flutter pub get
flutter run
```

### iOS Build Issues
```bash
cd ios
rm -rf Pods Podfile.lock
cd ..
flutter pub get
flutter run
```

### Firebase Connection Issues
1. Check internet connection
2. Verify google-services.json/GoogleService-Info.plist
3. Check Firebase project ID matches
4. Verify authentication is enabled

---

**Need help? Check the official Firebase documentation!**
