# JetKVM Companion

Obtainium-friendly release repository for the JetKVM Companion APK.

This repository contains release assets only. Source code lives in the Android support fork:

- Source directory: https://github.com/Batestinha/kvm/tree/jetkvm-android-native-login-pr/jetkvm-companion
- Upstream PR: https://github.com/jetkvm/kvm/pull/1441

## What This App Does

JetKVM Companion is a small target-side Android helper for JetKVM Android target setups.

It exists because Android's multi-display keyguard policy limits unlocking from secondary displays. AOSP documents that secondary-display lockscreen UI does not support unlocking from secondary screens, and its multi-display FAQ says the default secondary-display lockscreen is not interactive and does not allow unlocking.

References:

- https://source.android.com/docs/core/display/multi_display/lock-screen
- https://source.android.com/docs/core/display/multi_display/faq

The companion uses only public Android APIs:

- a foreground service,
- Android's overlay permission for a tiny non-touchable background launch-assist
  overlay,
- Android `InputDevice` metadata to detect JetKVM-identifiable local
  peripherals,
- a transparent `showWhenLocked` Activity,
- `KeyguardManager.requestDismissKeyguard()` when the target is already trusted by Extend Unlock.

It does not inject input, capture the screen, use ADB, require root, use
Accessibility, or depend on Shizuku.

## Suggested Target Modes

- **No lockscreen**: may work for some users, but some apps are hostile toward disabled lockscreen or insecure-device configurations.
- **Keyguard on with Extend Unlock**: recommended stock-Android mode. Install
  JetKVM Companion on the target phone, grant background launch assist and
  unrestricted battery usage, and enable launch-on-boot if desired.
- **Keyguard on without Extend Unlock**: no stock/public JetKVM-only solution. Hard-locked devices require the user credential or third-party automation/privilege such as Tasker, Shizuku, Accessibility automation, root, or device-owner/OEM privileges.

## Watchdog Model

JetKVM Companion uses a two-stage watchdog:

- A boot-started primary watchdog detects JetKVM-identifiable input devices
  exposed to Android. In the current JetKVM firmware this is the `JetKVM USB
  Emulation Device` keyboard plus touchscreen or pointer.
- The current Linux gadget VID/PID `1d6b:0104` is treated as supporting
  metadata, but the required identity signal is the JetKVM-specific input
  device name. This keeps detection stable if JetKVM later moves to a dedicated
  VID/PID.
- Only while those peripherals are present does a secondary screen-on watchdog
  perform trusted keyguard dismissal.
- One screen-on event produces one bounded transparent Activity launch and one
  `requestDismissKeyguard()` call. There is no retry loop.

## Obtainium

Add this repository URL to Obtainium:

```text
https://github.com/Batestinha/jetkvm-companion
```

Package ID:

```text
com.jetkvm.companion
```

APK asset pattern:

```text
JetKVM-Companion-.*\.apk
```
