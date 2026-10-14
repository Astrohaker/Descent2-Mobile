Descent II Mobile (Android)
===========================

This is a source port of the original DOS/Windows/Mac game "Descent II" for Android. You will need the game files from a licensed copy in order to play it.

**NOTE: This is an Android-only fork. The iOS port has been removed; see the upstream repository (devint1/Descent2-Mobile) for the version that still contains it.**

Licence
-------
Please see "COPYING."

Required Game Data
------------------
The app does not contain any game data. Only the PC data files are supported. The following files are needed:

- alien1.pig
- alien2.pig
- descent2.ham
- descent2.hog
- descent2.s22 -or- descent2.s11
- fire.pig
- groupa.pig
- ice.pig
- intro-h.mvl -or- intro-l.mvl
- other-h.mvl -or- other-l.mvl
- robots-h.mvl -or- robots-l.mvl
- water.pig

The movies (`*.mvl`) are optional.

**Release builds** ask for the data on first launch. The player picks the folder with their copy of Descent II (subfolders are searched too), and the app copies the data files into its private storage, lowercasing the names. On Android 4.4 and older, where there is no folder picker, the files have to be copied by hand into the folder the setup screen shows (`Android/data/astrohacker.descent2/files`).

**Debug builds** can bundle the data instead: copy the files to `Descent2/src/debug/assets` and the setup screen is skipped. This folder is ignored by git and is not included in release builds.

Redbook Music
-------------
Redbook music can be used instead of MIDI music, if desired. Put the tracks in a folder named `music` inside the game folder before picking it; the app copies them along with the data. Debug builds can also bundle them in `Descent2/src/debug/assets/music`. The tracks are found with the pattern `<tracknum>.*`, where `<tracknum>` is the track number as it would appear on the original Descent II CD. Descent2-Mobile uses the same scheme as the original version of Descent II, where track 02 is the title track, track 03 is the credits track, and the remaining tracks are used for the levels. The files can be in any format supported by `MediaPlayer`.

Building
--------
Open the project in Android Studio (for debug builds, copy the data files to `Descent2/src/debug/assets` first). From there, simply build/run the project. The Android version only supports the OpenGL ES renderer. Tested on an NVIDIA Shield Tablet K1 running Android 6.0.1 (Marshmallow) and a Nexus 6P running Android 6.0.1.

### Toolchain

The build uses Gradle 9.8.0 with Android Gradle plugin 9.4.1 and runs on the JBR bundled with current Android Studio (JDK 17 or newer). From the command line, point `JAVA_HOME` at that JDK, e.g. `JAVA_HOME=/path/to/android-studio/jbr ./gradlew :Descent2:assembleDebug`.

The required SDK components are pinned in `Descent2/build.gradle` and can be installed with `sdkmanager`:

    sdkmanager "platforms;android-36" "build-tools;36.0.0" "ndk;21.4.7075529" "cmake;3.18.1"

`assembleDebug` produces one APK per ABI (`armeabi-v7a`, `arm64-v8a`, `x86`, `x86_64`) in `Descent2/build/outputs/apk/debug` — there is no universal APK. Release builds are signed only when the signing properties below are set.

### Signing release builds

The release key is not part of the repository. Add its location and passwords to `~/.gradle/gradle.properties`:

    DESCENT2_STORE_FILE=/path/to/release.jks
    DESCENT2_STORE_PASSWORD=...
    DESCENT2_KEY_ALIAS=...
    DESCENT2_KEY_PASSWORD=...

`./gradlew :Descent2:assembleRelease` then writes signed APKs to `Descent2/build/outputs/apk/release`. Without these properties the release APKs are left unsigned.

Issues and Limitations
----------------------
Please see the “Issues” section in GitHub.

Special Thanks
--------------
- HMP to MIDI conversion code - Hans de Goede, d1x project
- LIBMVE, d2x project contributors