---
layout: default
title:  "Execute Tests in a Grid"
excerpt: "Learn to use BELLATRIX to execute tests in a grid."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/execute-tests-grid/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Explanations
------------
To execute BELLATRIX tests within a grid, you should configure that in the **bellatrix.config.ts** file.

Configuration
-------------
There's an option about execution type in the **bellatrix.config.ts** file under the **webSettings** section.
```typescript
executionType: 'local', // remote
```
You can set the grid URL and set some additional arguments under the **remoteExecutionSettings** section.
```typescript
executionSettings: {
    browserAutomationTool: 'playwright', // playwright, selenium
    browser: 'chrome', // chrome, firefox, safari, edge
    viewport: { width: 1920, height: 1080 },
    headless: false,
    executionType: 'local', // remote
    baseUrl: 'https://demos.bellatrix.solutions/'
},
remoteExecutionSettings: {
    remoteUrl: 'localhost:4444',
    user: '',
    key: ''
}
```
This is the structure for RemoteExecutionSettings:
```typescript
type RemoteExecutionSettings = {
    provider: 'Selenium Grid' | 'Selenoid',
    remoteUrl: string,
    capabilities: Capabilities,
    headers?: object,
} | {
    provider: 'LambdaTest' | 'BrowserStack',
    capabilities: Capabilities,
    username: string,
    accessKey: string,
}
```