# Firebase setup

1. Create a Firebase project.
2. Enable **Authentication → Sign-in method → Google**.
3. Create a **Web app** and copy its configuration into `firebase-config.js`.
4. Enable **Cloud Firestore** in production mode.
5. Publish [`firestore.rules`](./firestore.rules).
6. Add the APK/WebView domain to **Authentication → Settings → Authorized domains** if you use hosted web testing.
7. For GitHub Actions, add a repository secret named `FIREBASE_CONFIG_JS` containing the complete contents of `firebase-config.js`.
8. Add an Android app in Firebase with package name `com.evc.treasurer`, download `google-services.json`, base64-encode it, and save it as the GitHub secret `GOOGLE_SERVICES_JSON_B64`. The workflow injects it into the APK build; never commit this file.

The Firebase web configuration is an identifier, not a service-account private key. Never add service-account JSON or private keys to this repository.

The app uses Firebase Auth's local persistence and Firestore's offline cache. A user must sign in once while online; after that, the cached Google session can unlock the app offline and Firestore queues local changes until internet returns.

On Android, the app uses `@capacitor-firebase/authentication` and Credential Manager for the Google sign-in flow. In a normal browser/VSCode test, it uses Firebase's web popup flow instead.
