---
layout: default
title:  "Wait for Components"
excerpt: "Learn how to wait for web components with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/wait-for-components/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@Test
async blogPageOpened_When_PromotionsButtonClicked() {
    await this.app.navigation.navigate('https://demos.bellatrix.solutions/');
    const blogLink = this.app.create(Anchor).byLinkText('Blog');
    await blogLink.wait.toBeClickable();
    await blogLink.wait.toBeVisible();
    await blogLink.click();
}
```

Explanations
------------
```typescript
await blogLink.wait.toBeClickable();
await blogLink.wait.toBeVisible();
```
If there is a condtion you want to wait to be fulfilled before proceeding with the test, you can use the WaitService of the components. 

All Available toBe Methods
--------------------------
### toExist ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toExist();
```
Waits for the component to exist on the page. BELLATRIX always does it by default, before each method against the element.
### toBeVisible ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toBeVisible();
```
Waits for the component to be visible.
### toBeClickable ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toBeClickable();
```
Waits for the component to be clickable (may be disabled at first).
### toBeDisabled ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toBeDisabled();
```
Waits for the component to be disabled (may be clickable at first).
### toNotBeVisible ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toNotBeVisible();
```
Waits for the component to be invisible.
### toNotExist ###
```typescript
await this.app.create(Anchor).byLinkText("Blog").wait.toNotExist();
```
Waits for the component to disappear. Usually, we use in assertion methods.