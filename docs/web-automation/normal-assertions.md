---
layout: default
title:  "Normal Assertions"
excerpt: "Learn how to use normal assertion methods in BELLATRIX tests."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/normal-assertions/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@Test
async assertCartPageFields() {
  await this.app.navigation.navigate('https://demos.bellatrix.solutions/?add-to-cart=26');
  await this.app.navigation.navigate('https://demos.bellatrix.solutions/cart/');

  const couponCodeTextField = this.app.create(TextField).byId('coupon_code');
  Assert.areEqual(await couponCodeTextField.getPlaceholder(), 'Coupon code');

  const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
  Assert.isTrue(await applyCouponButton.isVisible());

  const messageAlert = this.app.create(Div).byClassContaining('woocommerce-message');
  Assert.isFalse(await messageAlert.isVisible());

  const updateCart = this.app.create(Button).byValueContaining('Update cart');
  Assert.isTrue(await updateCart.isVisible());

  const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
  Assert.areEqual(await totalSpan.getInnerText(), '120.00€');

  Assert.multiple(
      async () => Assert.areEqual(await totalSpan.getInnerText(), '120.00€'),
      async () => Assert.isTrue(await updateCart.isDisabled())
  );
}
```

Explanations
------------
```typescript
Assert.areEqual(await couponCodeTextField.getPlaceholder(), 'Coupon code');
```
```typescript
async () => Assert.isTrue(await updateCart.isDisabled())
```
Here we assert that the apply coupon button is visible on the page. On fail the following message is displayed:
```
Expected :true
Actual   :false
```
Cannot learn much about what happened.
```typescript
const messageAlert = this.app.create(Div).byClassContaining('woocommerce-message');
Assert.isFalse(await messageAlert.isVisible());
```
Since there are no validation errors, verify that the message div is not visible.
```typescript
const updateCart = this.app.create(Button).byValueContaining('Update cart');
Assert.isTrue(await updateCart.isVisible());
```
We have not made any changes to the added products so the update cart button should be disabled.
```typescript
const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
Assert.areEqual(await totalSpan.getInnerText(), '120.00€');
```
We check the total price contained in the order-total span HTML element.

```typescript
Assert.multiple(
    async () => Assert.areEqual(await totalSpan.getInnerText(), '120.00€'),
    async () => Assert.isTrue(await updateCart.isDisabled())
);
```
You can execute multiple assertions failing only once viewing all results.

<!-- One more thing you need to keep in mind is that normal assertion methods do not include BDD logging and any available hooks. BELLATRIX provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places. -->