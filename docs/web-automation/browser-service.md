---
layout: default
title:  "Browser Service"
excerpt: "Learn how to use BELLATRIX browser service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/browser-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@TestClass
class BrowserServiceTests extends WebTest {
    @Test
    async getCurrentUrl() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

        console.log(await this.app.browser.getUrl());
    }

    @Test
    async controlBrowser() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

        await this.app.browser.back();

        await this.app.browser.forward();

        await this.app.browser.refresh();
    }

    @Test
    async getTabTitle() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

        Assert.isTrue((await this.app.browser.getTitle()).includes("Bellatrix Demos"));
    }

    @Test
    async printCurrentPageHtml() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

        console.log(await this.app.browser.getPageSource());
    }
}
```

Explanations
------------
BELLATRIX gives you an interface to most common operations for controlling the started browser through the **browser** property.

```typescript
await this.app.browser.getUrl();
```
Get the current tab URL.
```typescript
await this.app.browser.back();
```
Simulates clicking the browser's back button.
```typescript
await this.app.browser.forward();
```
Simulates clicking the browser's forward button.
```typescript
await this.app.browser.refresh();
```
Simulates clicking the browser's refresh button.
```typescript
await this.app.browser.getTitle();
```
Get the current tab title.
```typescript
await this.app.browser.getPageSource();
```
Get the current page's HTML source.