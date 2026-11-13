# Studio Video - Aplikasi Android Editor Video & Foto
Repository: https://github.com/riakitty17-oss/editor-video

## ⚡ Cara Build APK Otomatis di GitHub (1 Menit, Gratis)
1. Buka repository Anda:
   👉 https://github.com/riakitty17-oss/editor-video
2. Klik **Add file** > **Create new file**, ketik nama:
   `.github/workflows/build.yml`
3. Tempelkan script workflow di bawah ini lalu klik **Commit changes**:

```yaml
name: Build Studio Video APK

on:
  push:
    branches: [ "main", "master" ]
  workflow_dispatch:

jobs:
  build:
    name: Build Android APK
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./StudioVideo

    steps:
      - name: Checkout Source Code
        uses: actions/checkout@v4

      - name: Set up Java JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: gradle

      - name: Accept Android SDK Licenses
        run: |
          yes | $ANDROID_HOME/cmdline-tools/latest/bin/sdkmanager --licenses || true

      - name: Setup Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.11.1'
          build-root-directory: ./StudioVideo

      - name: Build Debug APK
        run: gradle assembleDebug --stacktrace

      - name: Upload Studio Video APK Artifact
        uses: actions/upload-artifact@v4
        with:
          name: StudioVideo-debug-apk
          path: StudioVideo/app/build/outputs/apk/debug/app-debug.apk
          if-no-files-found: error
          retention-days: 30

```

4. Buka tab **Actions**, tunggu ~2 menit hingga centang hijau, lalu unduh **`StudioVideo-debug-apk`** di bagian **Artifacts**.
