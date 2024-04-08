---
layout: default
title:  "General Information"
excerpt: "Learn how to install and activate BELLATRIX. Read the system requirements."
date:   2022-08-12 06:50:17 +0200
parent: general-information
permalink: /general-information/
anchors:
  overview: Overview
  simple-installation: Simple Installation
  running-tests-through-cli: Running Tests through CLI
  supported-code-editors: Supported Code Editors
  # sdks-and-frameworks-prerequisites: SDKs and Frameworks Prerequisites
---
Overview
--------
Customize and extend our cross-platform Node.js framework to perfectly fit your needs. Start on top of hundreds of best practice features and integrations.

Contains the full source code of BELLATRIX Test Automation Framework and example project for faster usage

BELLATRIX is not a single thing it contains multiple framework libraries, extensions and tools.

Simple Installation
------------------
1. Download the BELLATRIX projects as a zip file from the Code green button in the right corner.
2. Unzip it. Open BELLATRIX-JavaScript in Visual Studio Code.
3. Open terminal and type `npm ci` in the root of the project.

4. In order to run the sample tests, navigate to the example folder with terminal and type `npm run test`.
5. You can try to write a simple test yourself.
6. For an in-depth revision of all framework features you can check the [**official documentation**](http://docs.javascript.bellatrix.solutions/web-automation/)

Running Tests through Bellatrix CLI
--------------------------
To execute your tests via command line in Continues Integration (CI), you can use the Bellatrix CLI through an npm script.
1. Navigate to the folder of your test project.
2. Open the CMD there.
3. Execute the following command:

```
bellatrix <path>
```
where `<path>` is an optional argument for a path to the tests folder, either relative or absolute.

Filters and other more advanced configurations will be supported in a future version.

Supported Code Editors
----------------------
The recommended code editor for writing BELLATRIX tests is Visual Studio Code.

<!-- SDKs and Frameworks Prerequisites
-------------------------------- 
We recommend to install the latest stable version of JDK.

For BELLATRIX desktop modules you need to download [**WinAppDriver**](https://github.com/Microsoft/WinAppDriver/releases). You need to make sure it is started before running any BELLATRIX desktop tests.

For BELLATRIX mobile modules you need to download and install [**Appium**](http://appium.io/). You need to make sure it is started before running any BELLATRIX mobile tests. -->
