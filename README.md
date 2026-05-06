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
- a transparent `showWhenLocked` Activity,
- `KeyguardManager.requestDismissKeyguard()` when the target is already trusted by Extend Unlock.

It does not inject input, capture the screen, use ADB, require root, use Accessibility, or depend on Shizuku.

## Suggested Target Modes

- **No lockscreen**: may work for some users, but some apps are hostile toward disabled lockscreen or insecure-device configurations.
- **Keyguard on with Extend Unlock**: recommended stock-Android mode. Install JetKVM Companion on the target phone and enable launch-on-boot if desired.
- **Keyguard on without Extend Unlock**: no stock/public JetKVM-only solution. Hard-locked devices require the user credential or third-party automation/privilege such as Tasker, Shizuku, Accessibility automation, root, or device-owner/OEM privileges.

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
