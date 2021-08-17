# AndroidCI

---
* [Introduction](#Introduction)
* [Setup](#1.-Add-github-folder)
    * [Actions](2.-Add-Actions)

## Introduction

GitHub Actions is a service provided by Github, which can help automate your workflow. More specifically, it allows you to add/edit/delete workflows, which are files stored in .github/workflows as part of the GitHub Actions feature.

##Setup GitHub Actions

### 1. Add github folder

First need to add a .github inside of root folder, example: AndroidCI/.github/<filename>.yml in this case the android.yml file on this proyect

```bash

|-- AndroidCI
|   |-- .github
|   |-- .gradle
|   |-- .idea
|   |-- app

```

### 2. Add Actions