# Weekly Ring

A small offline Android app: assign a different ringtone to each day of the
week. When a call comes in, it checks today's day and swaps the system
ringtone to match, right as the phone starts ringing.

**This app is 100% offline.** Check `app/src/main/AndroidManifest.xml` —
there is no `INTERNET` permission anywhere, so it is physically incapable of
making a network request. All data lives in local `SharedPreferences` on
the device.

## How to build the APK

1. Install [Android Studio](https://developer.android.com/studio) (free).
2. Open Android Studio → **Open** → select this `WeeklyRing` folder.
3. Let Gradle sync (first sync needs internet to download build tools —
   the *build process* needs internet, the *finished app* does not).
4. **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
5. Find the APK at `app/build/outputs/apk/debug/app-debug.apk` and install
   it on your phone (`adb install app-debug.apk`, or copy it to the phone
   and tap it, with "install unknown apps" allowed for that source).

## Using it

1. Open the app, tap **"Allow ringtone changes"** — this grants the special
   `WRITE_SETTINGS` permission Android requires for any app that changes
   the system ringtone.
2. Tap **"Choose ringtone"** under each day and pick a tone from your
   phone's built-in ringtone list.
3. Leave the app installed (it doesn't need to stay open). It reacts to the
   phone's `PHONE_STATE` broadcast in the background.

## Honest caveats

- This works by changing the **system default ringtone** at the moment the
  phone starts ringing — a well-known technique, but not an officially
  guaranteed API. It behaves reliably on stock Android; some manufacturers
  (Samsung, Xiaomi, etc.) aggressively kill background receivers unless you
  disable battery optimization for the app.
- If Do Not Disturb or a silent profile is active, no ringtone will play
  regardless of this app's settings.
- No `.apk` file is included in this project — Android app builds must be
  compiled and signed by an actual Android build toolchain (Android Studio
  or `gradlew assembleDebug` on a machine with the Android SDK installed),
  which isn't available in the environment that generated this code.
