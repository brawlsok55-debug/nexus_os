name: Build APK
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.9
      - run: unzip -q nexus-os-stage1.zip
      - run: gradle assembleDebug --stacktrace
        working-directory: nexus-os
      - uses: actions/upload-artifact@v4
        with:
          name: NexusOS-debug-apk
          path: nexus-os/app/build/outputs/apk/debug/*.apk
