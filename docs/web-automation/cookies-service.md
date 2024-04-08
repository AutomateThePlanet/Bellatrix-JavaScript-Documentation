---
layout: default
title:  "Cookies Service"
excerpt: "Learn how to use BELLATRIX cookies service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/cookies-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@TestClass
class CookiesServiceTests extends WebTest {
    @Test
    async getAllCookies() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        await this.app.cookies.addCookie('woocomerce_items_in_cart1', '3');
        await this.app.cookies.addCookie('woocomerce_items_in_cart2', '3');
        await this.app.cookies.addCookie('woocomerce_items_in_cart3', '3');

        const cookies = await this.app.cookies.getAllCookies();

        Assert.areEqual(cookies.length, 3);
    }

    @Test
    async getSpecificCookie() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        await this.app.cookies.addCookie('woocomerce_items_in_cart', '3');

        const itemsInCartCookie = await this.app.cookies.getCookie('woocomerce_items_in_cart');

        Assert.areEqual(itemsInCartCookie?.value, '3');
    }

    @Test
    async deleteAllCookies() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        const protonRocketAddToCartBtn = this.app.create(Anchor).allByInnerTextContaining('Add to cart').get(0);
        await protonRocketAddToCartBtn.click();

        await this.app.cookies.clearCookies();
    }

    @Test
    async deleteSpecificCookie() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        const protonRocketAddToCartBtn = this.app.create(Anchor).allByInnerTextContaining('Add to cart').get(0);
        await protonRocketAddToCartBtn.click();

        await this.app.cookies.deleteCookie('woocomerce_items_in_cart');
    }

    @Test
    async addNewCookie() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        await this.app.cookies.addCookie('woocomerce_items_in_cart1', '3');
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with cookies using the **cookies** property. You need to make sure that you have navigated to the desired web page.
```typescript
await this.app.cookies.getAllCookies();
```
Get all cookies.
```typescript
await this.app.cookies.getCookie('woocomerce_items_in_cart');
```
Get a specific cookie by name.
```typescript
await this.app.cookies.clearCookies();
```
Delete all cookies.
```typescript
await this.app.cookies.deleteCookie('woocomerce_items_in_cart');
```
Delete a specific cookie by name.
```typescript
await this.app.cookies.addCookie('woocomerce_items_in_cart1', '3');
```
Add a new cookie.