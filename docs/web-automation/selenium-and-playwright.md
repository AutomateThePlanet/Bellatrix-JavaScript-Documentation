---
layout: default
title:  "Selenium and Playwright"
excerpt: "Learn how to switch between Selenium and Playwright engines."
date:   2024-04-07 06:50:17 +0200
parent: web-automation
permalink: /web-automation/selenium-and-playwright/
anchors:
---
Overview
--------
Node.js BELLATRIX framework supports both Selenium and Playwright for web browser automation testing. To switch from one to the other, you simply have to type whichever engine you want to use in the bellatrix.config file, **browserAutomationTool** setting.
```typescript
executionSettings: {
    browserAutomationTool: 'playwright', // playwright, selenium
    browser: 'chrome', // chrome, firefox, safari, edge
    viewport: { width: 1920, height: 1080 },
    headless: false,
    executionType: 'local', // remote
},
```