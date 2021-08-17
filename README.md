# AndroidCI

---
* [Introduction](#Introduction)
* [Setup Git Workflow](#Setup-GitHub-Actions)
    * [Add Github Folder](#1.-Add-github-folder)
    * [Add Github Actions](#2.-Add-Actions)

## Introduction

GitHub Actions is a service provided by Github, which can help automate your workflow. More specifically, it allows you to add/edit/delete workflows, which are files stored in .github/workflows as part of the GitHub Actions feature.

In this project just want to upload .apk to github artifacts and send it to Firebase app distribution, add release notes and send it to the testers

## Setup GitHub Actions

### 1. Add github folder

First need to add a .github inside of root folder, example: AndroidCI/.github/<filename>.yml in this case the android.yml file on this proyect AndroidCI

```bash

|-- AndroidCI
|   |-- .github
|   |   |-- <filename>.yml
|   |-- .gradle
|   |-- .idea
|   |-- app

```

### 2. Add Actions

GitHub Actions help you automate tasks within your software development life cycle. GitHub Actions are event-driven, meaning that you can run a series of commands after a specified event has occurred. For example, every time someone creates a pull request for a repository, you can automatically run a command that executes a software testing script.

```yml
# Name of the Action
name: Android CI
# When the action is triggered
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'
          distribution: 'adopt'

      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
        
      - name: Build with Gradle
        run: ./gradlew build
        
      - name: Upload Apk
        uses: actions/upload-artifact@v2.2.4
        with:
          name: app
          path: app/build/outputs/apk/debug/app-debug.apk

      - name: upload artifact to Firebase App Distribution
        uses: wzieba/Firebase-Distribution-Github-Action@v1
        with:
          appId: ${{secrets.FIREBASE_APP_ID}}
          token: ${{secrets.FIREBASE_TOKEN}}
          groups: testers
          releaseNotesFile: app/release-notes.txt
          file: app/build/outputs/apk/release/app-release-unsigned.apk
```