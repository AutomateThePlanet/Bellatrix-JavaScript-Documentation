---
layout: default
title:  "Validate Assertions"
excerpt: "Learn how to use BELLATRIX Validate assertion methods."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/validate-assertions/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@Test
async assertValidateCartPageFields() {
  await this.app.navigation.navigate('https://demos.bellatrix.solutions/?add-to-cart=26');
  await this.app.navigation.navigate('https://demos.bellatrix.solutions/cart/');

  const couponCodeTextField = this.app.create(TextField).byId('coupon_code');
  couponCodeTextField.validate('placeholder').is('Coupon code');

  const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
  applyCouponButton.validateIs('visible');

  const messageAlert = this.app.create(Div).byClassContaining('woocommerce-message');
  messageAlert.validateIsNot('visible');

  const updateCart = this.app.create(Button).byValueContaining('Update cart');
  updateCart.validateIs('disabled');

  const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
  totalSpan.validate('innerText').is('120.00€');
}
```

Explanations
------------
```typescript
// Assert.areEqual(await couponCodeTextField.getPlaceholder(), 'Discount code');
couponCodeTextField.validate('placeholder').is('Coupon code');
```
We can assert the default text in the coupon text fiend through the BELLATRIX component **getPlaceholder** method. The different BELLATRIX web component classes contain lots of these methods which are a representation of the most important HTML element attributes. The biggest drawback of using vanilla assertions is that the messages displayed on failure are not meaningful at all. This is so because most unit testing frameworks are generic to suite all use cases. In next chapter, there is information how BELLATRIX solves the problems with the introduction of **validate** methods. If the bellow assertion fails the following message is displayed:
```
Expected : Discount code
Actual   : Coupon code
```
You can guess what happened, but you do not have information which element failed and on which page. If we use the **validate** methods, BELLATRIX waits some time for the condition to pass. After this period if it is not successful, a beautified meaningful exception message is displayed:
```
The component's placeholder should be 'Discount code' but was 'Coupon code'. The test failed on URL: http://demos.bellatrix.solutions/cart/
```

```typescript
const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
// Assert.isTrue(await applyCouponButton.isVisible());
applyCouponButton.validateIs('visible');
```
Assert that the apply coupon button exists and is visible on the page. On fail, the normal assertion prints the following message:
```
Expected :true
Actual   :false
```
Cannot learn much about what happened.
Now if we use the **validateIs('visible')** method and the check does not succeed the following error message is displayed:
```
The control should be visible but wasn't. The test failed on URL: http://demos.bellatrix.solutions/cart/
```
To all exception messages, the current URL is displayed, which improves the troubleshooting.
```typescript
const messageAlert = this.app.create(Div).byClassContaining('woocommerce-message');
messageAlert.validateIsNot('visible');
```
Since there are no validation errors, verify that the message div is not visible.
```typescript
const updateCart = this.app.create(Button).byValueContaining('Update cart');
updateCart.validateIs('disabled');
```
No changes are made to the added products so the update cart button should be disabled.
```typescript
const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
totalSpan.validate('innerText').is('120.00€');
```
Check the total price contained in the order-total span HTML element. By default, all Validate methods have 5 seconds timeout. However, you can specify a custom timeout and sleep interval (period for checking again) for each validate method, or you can change the default timeout in the config file.
<!-- 
BELLATRIX provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places. -->