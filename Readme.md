MEGA Android Client
================

A fully-featured client to access your Cloud Storage provided by MEGA.

This document will guide you to build the application on a Linux machine with Android Studio.

> **Note:** This fork intentionally disables WebRTC (chat and audio/video calling features) via
> `DISABLE_WEBRTC = true` in `app/src/main/jni/Application.mk`. The original MEGA-hosted WebRTC
> prebuilt library link (step 6 below) now returns **"File is no longer available"** on mega.nz —
> MEGA's own takedown notice states this happens when the file was removed after a Terms of
> Service report, or the uploading account was suspended for repeated ToS breaches. Since no
> working replacement link is available, WebRTC is disabled instead. This is not needed for core
> file storage/sync/sharing functionality.

### Setup development environment

* [Android Studio](https://developer.android.com/studio)

* [Android SDK Tools](https://developer.android.com/studio#Other)

* [Android NDK](https://developer.android.com/ndk/downloads)

### Build & Run the application

1. Get the source code.

```
git clone --recursive https://github.com/meganz/android.git
```

2. Install in your system the [Android NDK 21](https://dl.google.com/android/repository/android-ndk-r21d-linux-x86_64.zip) (latest version tested: NDK r21d).

3. Export `NDK_ROOT` variable or create a symbolic link at `${HOME}/android-ndk` to point to your Android NDK installation path.

```
export NDK_ROOT=/path/to/ndk
```
```
ln -s /path/to/ndk ${HOME}/android-ndk
```

4. Export `ANDROID_HOME` variable or create a symbolic link at `${HOME}/android-sdk` to point your Android SDK installation path.

```
export ANDROID_HOME=/path/to/sdk
```
```
ln -s /path/to/sdk ${HOME}/android-sdk
```

5. Export `JAVA_HOME` variable or create a symbolic link at `${HOME}/android-java` to point your Java installation path.

```
export JAVA_HOME=/path/to/jdk
```
```
ln -s /path/to/jdk ${HOME}/android-java
```

6. ~~Download the link https://mega.nz/file/RsMEgZqA#s0P754Ua7AqvWwamCeyrvNcyhmPjHTQQIxtqziSU4HI, uncompress it and put the folder `webrtc` in the path `app/src/main/jni/megachat/`.~~
   **Skipped in this fork.** WebRTC is intentionally disabled (`DISABLE_WEBRTC = true` in
   `app/src/main/jni/Application.mk`), so this step is not required. Chat and calling features
   will be unavailable, but core storage/sync functionality is unaffected.

7. Before running the building script, install the required packages. For example for Ubuntu or other Debian-based distro:

```
sudo apt install build-essential swig automake libtool autoconf cmake
```

8. Build SDK by running `./build.sh all` at `app/src/main/jni/`. You could also run `./build.sh clean` to clean the previous configuration. **IMPORTANT:** check that the build process finished successfully, it should finish with the **Task finished OK** message. Otherwise, modify `LOG_FILE` variable in `build.sh` from `/dev/null` to a certain text file and run `./build.sh all` again for viewing the build errors.

9. Open the project with Android Studio, let it build the project and hit _*Run*_.

> **Note:** The original step 9 here (a MEGA-hosted link providing `debug`/`release` folders for
> `app/src/`) now also returns **"File is no longer available"** on mega.nz, for the same
> takedown reasons as the WebRTC link above. The exact contents of that archive are unknown, so
> it has been removed from these instructions rather than guessed at. If the build fails due to
> missing files under `app/src/debug/` or `app/src/release/`, that archive is the likely cause —
> check the repository's git history or open an issue upstream for guidance on what belongs there.

#### macOS setup

To build jni libs on macOS, you need install these dependencies via brew:

    `brew install bash gnu-sed gnu-tar autoconf automake cmake coreutils libtool swig wget xz`

Then reboot MacOS to ensure newly installed latest bash(v5.x) overrides default v3.x in PATH

Then edit PATH env (Please make sure the gnu paths are setup in front of $PATH):

    `export PATH="/usr/local/opt/gnu-tar/libexec/gnubin:$PATH"`
    `export PATH="/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"`

Then download and setup NDK follow guides above, then run this command to build:

    `bash ./build.sh all`


##### If the build script fails to detect cmake when building ffmpeg extension on a mac

1. In Android studio, open the SDK manager (Or through Settings>Appearance & Behaviour>System Settings>Android SDK)
2. Go to the SDK Tools tab
3. Check the "Show package details" box
4. Expand the CMake section in the list
5. Select 3.10.2.4988404
6. Click "OK"
7. Add the following to your PATH:
    `export PATH="/Users/{USERNAME}/Library/Android/sdk/cmake/3.10.2.4988404/bin:$PATH"`
8. Retry the build

### Notice

To use the *geolocation feature* you need a *Google Maps API key*:

1. To get one, follow the directions here: https://developers.google.com/maps/documentation/android/signup.

2. Once you have your key, replace the "google_maps_key" string in these files: `app/src/debug/res/values/google_maps_api.xml` and `app/src/release/res/values/google_maps_api.xml`.