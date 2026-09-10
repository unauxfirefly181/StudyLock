# Study Lock

A small Android app that pins your phone to a full-screen countdown for a set number
of minutes (default 120 = 2 hours), and only reveals a random unlock code once the
timer finishes.

## What it actually does (read this first)

Android does **not** let a normal app silently change your real system lock-screen
PIN — that API was locked down years ago and is now only usable by phones enrolled
in a company/enterprise MDM. So this app doesn't touch your real lock screen.

Instead it uses Android's built-in **Screen Pinning** (`startLockTask()`), the same
feature banks/kiosk apps use, to pin itself as the only usable app on the phone.
While pinned:
- Home and Recents are disabled.
- Back does nothing until the timer ends.
- A random 6-digit code is generated and stored, but never shown to you.
- A persistent notification shows the time remaining.
- When the timer hits zero, the code is revealed on-screen and you tap "Unlock" to exit.

**Honest limitations:**
- It verifies that *time passed with the phone locked away*, not that you actually
  studied. There's no reliable way for any app to verify real studying.
- Screen pinning can technically be broken by long-pressing Back + Recents together
  (that's a stock Android gesture, not something an app can block) or by holding the
  power button and choosing to force-stop the app from Settings. If you want zero
  temptation, hand the phone to someone else once it's locked.
- Some OEM skins (heavily customized ones) restrict `startLockTask()` slightly
  differently. It works reliably on stock/near-stock Android (Pixel, Android One, etc.).

## Build & run

1. Install [Android Studio](https://developer.android.com/studio).
2. Open this folder (`StudyLock`) as a project — Android Studio will offer to
   generate the Gradle wrapper automatically; accept it and let Gradle sync.
3. Connect your phone via USB with USB debugging enabled (Settings → Developer
   options → USB debugging), or use an emulator.
4. Click Run ▶ to install and launch the app on your device.

## Using it

1. Open the app, enter minutes (default 120), tap **Start Lock Session**.
2. The phone pins itself and starts counting down. Put it down and study.
3. When time's up, the code appears on screen automatically — type it in and tap
   **Unlock**.

## Recommended settings for reliability

- Settings → Battery → Study Lock → set to "Unrestricted" (so Android doesn't kill
  the background service).
- Keep the phone plugged in or with reasonable battery — if it dies, the session
  state is saved and resumes from where it left off next time you power on and open
  the app.

## If something goes wrong

If the app gets stuck or you need out in an emergency, connect the phone to a
computer with `adb` and run:
```
adb shell am force-stop com.studylock.app
```
This is a legitimate escape hatch — keep it in mind rather than relying on it as
your everyday plan, or the lock won't do its job.
