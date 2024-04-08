---
layout: default
title:  "Navigate to Pages"
excerpt: "Learn how to navigate to web pages with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/navigate-to-pages/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@TestClass
class ProductPurchaseTests extends WebTest {
    @Test
    async promotionsPageOpened_When_PromotionsButtonClicked() {
        await this.app.navigation.navigate('http://demos.bellatrix.solutions/');

        const promotionsLink = this.app.create(Anchor).byLinkText('Promotions');

        await promotionsLink.click();

        await this.app.browser.waitUntilPageLoadsCompletely();
    }

    @Test
    public void blogPageOpened_When_PromotionsButtonClicked() {
        await this.app.navigation.navigate('http://demos.bellatrix.solutions/');

        const blogLink = this.app.create(Anchor).byLinkText('Blog');

        await blogLink.click();
    }
}
```

Explanations
------------

```typescript
await this.app.navigation.navigate('http://demos.bellatrix.solutions/');
```
You can always navigate in each separate tests, but if all of them go to the same page, you can use the techniques shown below for code reuse.
```typescript
await this.app.browser.waitUntilPageLoadsCompletely();
```
Sometimes, some AJAX async calls are not caught natively by WebDriver. So you can use the BELLATRIX browser service's method. **waitUntilPageLoadsCompletely** which waits for these calls automatically to finish. Keep in mind that usually this is not necessary since BELLATRIX has a complex built-in mechanism for handling element waits.

Depending on the types of tests you want to write there are a couple of ways to navigate to specific pages.
In later chapters, there are more details about the different test workflow hooks. Find here two of them.
```typescript
override async beforeAll() {
    await this.app.navigation.navigate('http://demos.bellatrix.solutions/');
}
```
If you reuse your browser and want to navigate once to a specific page. You can use the **beforeAll** method.
It executes once for all tests in the class.
```typescript
override async beforeEach() {
    await this.app.navigation.navigate('http://demos.bellatrix.solutions/');
}
```
If you need each test to navigate each time to the same page, you can use the **beforeEach** method.
```typescript
await this.app.navigation.waitForPartialUrl('/blog/');
```
Sometimes before proceeding with searching and making actions on the next page, we need to wait for something.
It is useful in some cases to wait for a partial URL instead hard-coding the whole URL since it can change depending on the environment. Keep in mind that usually this is not necessary since BELLATRIX has a complex built-in mechanism for handling element waits.
```typescript
await this.app.navigation.navigateToLocalPage('testPage.html');
```
Sometimes you may need to navigate to a local HTML file. Make sure to copy the file to the resources folder of the module with your tests files.