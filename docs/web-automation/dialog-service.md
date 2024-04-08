---
layout: default
title:  "Dialog Service"
excerpt: "Learn how to use BELLATRIX dialog service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/dialog-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@TestClass
class DialogServiceTests extends WebTest {
    @Test
    async acceptDialogAlert() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        const couponButton = this.app.create(Button).byId('couponBtn');
        await couponButton.click();

        await this.app.dialog.accept();
    }

    @Test
    async happyBirthdayCouponDisplayed_When_ClickOnCouponButton() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        const couponButton = this.app.create(Button).byId('couponBtn');
        await couponButton.click();

        await this.app.dialog.handle(async dialog => {
            Assert.areEqual((await dialog.getMessage()), 'Try the coupon- happbirthday');
        });
    }

    @Test
    async dismissDialogAlert() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/welcome/');

        const couponButton = this.app.create(Button).byId('couponBtn');
        await couponButton.click();

        await this.app.dialog.dismiss();
    }
}
```

Explanations
------------
BELLATRIX gives you some methods for handling dialogs.
```typescript
await this.app.dialog.accept();
```
You can accept the alert.
```typescript
await this.app.dialog.handle(async dialog => {
    Assert.areEqual((await dialog.getMessage()), 'Try the coupon- happbirthday');
});
```
You can pass a lambda function and do something with the alert.
```typescript
await this.app.dialog.dismiss();
```
You can tell the dialog service to dismiss it.
```typescript
await this.app.dialog.accept('some prompt text');
```
You can first type in the dialog and then accept it.