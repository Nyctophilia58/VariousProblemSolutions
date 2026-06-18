# Flutter Development Environment Setup

This guide explains how to install and configure Flutter with Android Studio and VS Code on both **Windows** and **Arch Linux**.

---

# Requirements

* Flutter SDK
* Android Studio
* Android SDK
* VS Code
* Dart Extension
* Flutter Extension
* JDK 17+

---

# Windows Setup

## 1. Install Flutter

Download Flutter SDK:

https://docs.flutter.dev/get-started/install/windows

Extract it to:

```
C:\src\flutter
```

---

## 2. Add Flutter to PATH

Open:

```
System Properties
→ Environment Variables
→ Path
```

Add:

```
C:\src\flutter\bin
```

Verify:

```powershell
flutter --version
```

---

## 3. Install Android Studio

Download:

https://developer.android.com/studio

During installation, install:

* Android SDK
* Android SDK Platform
* Android SDK Build Tools
* Android Emulator
* Android SDK Platform Tools

---

## 4. Find Android SDK Location

Open:

```
Android Studio
→ More Actions
→ SDK Manager
```

Usually:

```
C:\Users\<username>\AppData\Local\Android\Sdk
```

---

## 5. Configure Environment Variables

Add:

```
ANDROID_HOME
```

Value:

```
C:\Users\<username>\AppData\Local\Android\Sdk
```

Add these to PATH:

```
%ANDROID_HOME%\platform-tools
%ANDROID_HOME%\cmdline-tools\latest\bin
```

---

## 6. Accept Android Licenses

```powershell
flutter doctor --android-licenses
```

Accept everything.

---

## 7. Install VS Code

Download:

https://code.visualstudio.com/

Install extensions:

* Flutter
* Dart

Verify:

```powershell
flutter doctor
```

Expected:

```
[✓] Flutter
[✓] Android toolchain
[✓] Android Studio
[✓] VS Code
```

---

# Arch Linux Setup

## 1. Update System

```bash
sudo pacman -Syu
```

---

## 2. Install Dependencies

```bash
sudo pacman -S git curl unzip zip xz jdk17-openjdk
sudo pacman -Sy dart
```

Verify:

```bash
java --version
```

### Optional

```bash
sudo pacman -Sy android-tools
```

## 3. Install Flutter

Download:

```bash
git clone https://github.com/flutter/flutter.git -b stable ~/flutter
```

---

## 4. Add Flutter to PATH

```bash
export PATH="$HOME/flutter/bin:$PATH"
```

Reload:

```bash
source ~/.bashrc
```

---

## 5. Configure Flutter

```bash
sudo usermod -a -G flutterusers <your-user>
```

---

## 6. Install Android Studio

```bash
sudo pacman -S android-studio
```

Launch:

```bash
android-studio
```

---

## 7. Install Android SDK

Inside Android Studio:

```
More Actions
→ SDK Manager
```

Install:

* Android SDK
* Android SDK Platform
* Android SDK Platform Tools
* Android SDK Build Tools
* Android Emulator

Typical location:

```
/home/<username>/Android/Sdk
```

---

## 8. Configure Environment Variables

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
export ANDROID_HOME=$HOME/Android/Sdk
export ANDROID_SDK_ROOT=$ANDROID_HOME
```

Reload:

```bash
source ~/.bashrc
```

Verify:

```bash
echo $ANDROID_HOME
```

---

## 9. Accept Android Licenses

```bash
flutter doctor --android-licenses
```

Accept everything.

---

## 10. CommandLine Tools

```bash
mkdir -p ~/Android/Sdk/cmdline-tools
wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O /tmp/cmdline-tools.zip
unzip /tmp/cmdline-tools.zip -d ~/Android/Sdk/cmdline-tools
mv ~/Android/Sdk/cmdline-tools/cmdline-tools ~/Android/Sdk/cmdline-tools/latest
```

---

## 11. Install VS Code

```bash
sudo pacman -S code
```

Launch:

```bash
code
```

Install extensions:

* Flutter
* Dart

---

# Creating a Flutter Project

Create:

```bash
flutter create my_app
```

Enter:

```bash
cd my_app
```

Open:

```bash
code .
```

---

# Create an Android Emulator

Open:

```
Android Studio
→ More Actions
→ Virtual Device Manager
```

Create:

* Pixel Device
* Latest Android Version

Start emulator.

Verify:

```bash
flutter devices
```

---

# Running the App

```bash
flutter run
```

or press:

```
F5
```

inside VS Code.

---

# Useful Commands

Upgrade Flutter:

```bash
flutter upgrade
```

Check installation:

```bash
flutter doctor
```

Detailed information:

```bash
flutter doctor -v
```

List devices:

```bash
flutter devices
```

Clean project:

```bash
flutter clean
```

Get dependencies:

```bash
flutter pub get
```

Run app:

```bash
flutter run
```

Build APK:

```bash
flutter build apk
```

Build Release APK:

```bash
flutter build apk --release
```

Build App Bundle:

```bash
flutter build appbundle
```

---

# Troubleshooting

## Android SDK not found

Windows:

```
C:\Users\<username>\AppData\Local\Android\Sdk
```

Linux:

```
/home/<username>/Android/Sdk
```

**Linux paths are case-sensitive**

```
Sdk ≠ sdk
```

---

## Android licenses not accepted

Run:

```bash
flutter doctor --android-licenses
```

---

## Emulator not detected

Restart ADB:

```bash
adb kill-server
adb start-server
```

Verify:

```bash
flutter devices
```

---

## Verify Entire Setup

```bash
flutter doctor -v
```
