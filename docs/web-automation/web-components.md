---
layout: default
title:  "Web Components"
excerpt: "Learn how to use BELLATRIX web components."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/web-components/
anchors:
  example: Example
  explanations: Explanations
  full-list-of-all-supported-web-components: List of All Web Components
---
Example
-------
```typescript
@Test
async purchaseRocket() {
  await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

  const sortDropDown = this.app.create(Select).byNameEndingWith('orderby');
  await sortDropDown.selectByText('Sort by price: low to high');

  const protonMReadMoreButton = this.app.create(Anchor).byInnerTextContaining('Read more');
  await protonMReadMoreButton.hover();

  const addToCartFalcon9 = this.app.create(Anchor).byAttributeContaining('data-product_id', '28');
  await addToCartFalcon9.click();

  const viewCartButton = this.app.create(Anchor).byClassContaining('added_to_cart wc-forward');
  await viewCartButton.click();

  const couponCodeTextField = this.app.create(TextField).byId('coupon_code');
  await couponCodeTextField.setText('happybirthday');

  const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
  await applyCouponButton.click();

  const messageAlert = this.app.create(Button).byClassContaining('woocommerce_message');
  messageAlert.wait.toBeVisible();

  await messageAlert.validate('innerText').is('Coupon code applied successfully.');

  const quantityBox = this.app.create(NumberInput).byClassContaining('input-text qty text');
  await quantityBox.setNumber(0);
  await quantityBox.setNumber(2);

  const updateCart = this.app.create(Button).byValueContaining('Update cart');
  await updateCart.click();

  const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
  await totalSpan.validate('innerText').is('95.00€');

  const proceedToCheckout = this.app.create(Anchor).byClassContaining('checkout-button button alt wc-forward');
  await proceedToCheckout.click();

  const billingDetailsHeading = this.app.create(Heading).byInnerTextContaining('Billing details');
  billingDetailsHeading.wait.toBeVisible();

  const showLogin = this.app.create(Anchor).byInnerTextContaining('Click here to login');
  await showLogin.validate('href').is('https://demos.bellatrix.solutions/checkout/#');
  
  const orderCommentsTextArea = this.app.create(TextArea).byId('order_comments');
  await orderCommentsTextArea.scrollToVisible();
  await orderCommentsTextArea.setText('Please send the rocket to my door step!');

  const billingFirstName = this.app.create(TextField).byId("billing_first_name");
  billingFirstName.setText('In');

  const billingLastName = this.app.create(TextField).byId("billing_last_name");
  billingLastName.setText('DeepThought');

  const billingCompany = this.app.create(TextField).byId("billing_company");
  billingCompany.setText('Automate The Planet Ltd.');

  const billingCountry = this.app.create(Select).byId('billing_country');
  billingCountry.selectByText('Bulgaria');

  const billingAddress1 = this.app.create(TextField).byId("billing_address_1");
  Assert.areEqual(await billingAddress1.getPlaceholder(), 'House number and street name');
  billingAddress1.setText('bul. Yerusalim 5');

  const billingAddress2 = this.app.create(TextField).byId("billing_address_2");
  billingAddress2.setText('bul. Yerusalim 6');

  const billingCity = this.app.create(TextField).byId('billing_city');
  billingCity.setText('Sofia');

  const billingState = this.app.create(Select).byId('billing_state');
  billingState.selectByText('Sofia-Grad');

  const billingZip = this.app.create(TextField).byId('billing_postcode');
  billingZip.setText('1000');

  const billingPhone = this.app.create(PhoneField).byId('billing_phone');
  billingPhone.setPhone('+00359894646464');

  const billingEmail = this.app.create(EmailField).byId('billing_email');
  billingEmail.setEmail('info@bellatrix.solutions');

  const createAccountCheckBox = this.app.create(CheckBox).byId('createaccount');
  await createAccountCheckBox.check();

  const checkPaymentsRadioButton = this.app.create(RadioButton).byAttributeContaining('for', 'payment_method_cheque');
  checkPaymentsRadioButton.click();
}
```

Explanations
------------
As mentioned before BELLATRIX exposes 30+ web components. All of them implement Proxy design pattern which means that they are not located immediately when they are created. Another benefit is that each of them includes only the actions that you should be able to do with the specific control and nothing more.
For example, you cannot type into a button. Moreover, this way all of the actions has meaningful names – **setText** not **sendKeys** as in vanilla **WebDriver**, or **fill** as in vanilla **Playwright**
```typescript
const sortDropDown = this.app.create(Select).byNameEndingWith('orderby');
```
Create methods accept a generic parameter the type of the web control. Then only the methods for this specific control are accessible. Here we tell BELLATRIX to find your component by name attribute ending with 'orderby'.

```html
<select name="orderby" class="orderby">
   <option value="popularity" selected="selected">Sort by popularity</option>
   <option value="rating">Sort by average rating</option>
   <option value="date">Sort by newness</option>
   <option value="price">Sort by price: low to high</option>
   <option value="price-desc">Sort by price: high to low</option>
</select>
```
```typescript
await sortDropDown.selectByText('Sort by price: low to high');
```
You can select from select inputs by text (**selectByText**), by index (**selectByIndex**), or by value (**selectByValue**). Also, you can get the selected option through **getSelected** method.
```html
<a href='http://demos.bellatrix.solutions/product/proton-m/'>Read more</a>
```
```typescript
const protonMReadMoreButton = this.app.create(Anchor).byInnerTextContaining('Read more');
```
Here BELLATRIX finds the first anchor component which has inner text containing the 'Read more' text.
```typescript
await protonMReadMoreButton.hover();
```
You can Hover and Focus on most web components. Also, can invoke Click on anchors.
```html
<a href="/?add-to-cart=28" data-product_id="28">Add to cart</a>
```
```typescript
const addToCartFalcon9 = this.app.create(Anchor).byAttributeContaining('data-product_id', '28');
await addToCartFalcon9.click();
```
Locate components by custom attribute. BELLATRIX waits until the element is actionable before doing any actions, for Selenium. Playwright has this logic built-in.
```html
<a href="http://demos.bellatrix.solutions/cart/" class="added_to_cart wc-forward" title="View cart">View cart</a>
```
```typescript
const viewCartButton = this.app.create(Anchor).byClassContaining('added_to_cart wc-forward');
await viewCartButton.click();
```
Find the anchor by class 'added_to_cart wc-forward' and click it, after waiting for it to be clickable.
```typescript
const couponCodeTextField = this.app.create(TextField).byId('coupon_code');
```
Find a regular text input component by id = 'coupon_code'.
```typescript
await couponCodeTextField.setText('happybirthday');
```
Instead of using vanilla WebDriver sendKeys or vanilla Playwright fill to set the text, use the setText method.
```html
<input type="submit" class="button" name="apply_coupon" value="Apply coupon">
```
```typescript
const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
await applyCouponButton.click();
```
Create a button control by value attribute containing the text 'Apply coupon'. Button can be any of the following web components – input button, input submit or button.
```html
<div class="woocommerce-message" role="alert">Coupon code applied successfully.</div>
```
```typescript
const messageAlert = this.app.create(Button).byClassContaining('woocommerce_message');
messageAlert.wait.toBeVisible();
```
Wait for the message DIV to show up.
```typescript
// Assert.areEqual(await billingAddress1.getPlaceholder(), 'House number and street name');
```
Sometimes you need to verify the content of some component. However, since the asynchronous nature of websites, the text or event may not happen immediately. Playwright has Assert methods with built-in waiting logic for which one can set the timeout, but our Selenium doesn't; This makes the simple **Assert** methods + vanilla **WebDriver** useless. 
The commented code fails 1 from 5 times.
To handle these situations, BELLATRIX has hundreds of Ensure methods that wait for some condition to happen before asserting.
Bellow the statement waits for the specific text to appear and assert it.
```typescript
await messageAlert.validate('innerText').is('Coupon code applied successfully.');
```
**Note**: *There are much more details about these methods in the next chapters.*
```html
<input type="number" id="quantity_5ad35e76b34a2" step="1" min="0" max="" value="1" size="4" pattern="[0-9]*" inputmode="numeric">
```
```typescript
const quantityBox = this.app.create(NumberInput).byClassContaining('input-text qty text');
```
Find the number input component by class 'input-text qty text'.
```typescript
await quantityBox.setNumber(0);
await quantityBox.setNumber(2);
```
For number input components, you can set the number and get most of the properties of these components.
```typescript
const billingDetailsHeading = this.app.create(Heading).byInnerTextContaining('Billing details');
billingDetailsHeading.wait.toBeVisible();
```
As mentioned before, BELLATRIX has special synchronization mechanism for locating components, so usually, there is no need to wait for specific components to appear on the page. However, there may be some rare cases when you need to do it. The statement finds the heading by its inner text containing the text 'Billing details'.
```typescript
billingDetailsHeading.wait.toBeVisible();
```
Wait for the heading with the above text to be visible. This means that the correct page is loaded.
```typescript
// Assert.assertEqual(await showLogin.getHref(), 'http://demos.bellatrix.solutions/checkout/#');
await showLogin.validate('href').is('https://demos.bellatrix.solutions/checkout/#');
```
All web components have multiple properties for their most important attributes and ensure methods for their verification.
```typescript
const orderCommentsTextArea = this.app.create(TextArea).byId('order_comments');
await orderCommentsTextArea.scrollToVisible();
```
Here we find the order comments text area and since it is below the visible area we scroll down so that it gets visible on the video recordings. Then the text is set. One can easily set in the configuration file to enable automatic scroll before executing actions on elements that are not currently visible.
```typescript
const billingAddress1 = this.app.create(TextField).byId("billing_address_1");
Assert.areEqual(await billingAddress1.getPlaceholder(), 'House number and street name');
```
Through the Placeholder, you can get the default text of the control.
```typescript
const billingPhone = this.app.create(PhoneField).byId('billing_phone');
billingPhone.setPhone('+00359894646464');
```
Create the special text field control Phone it contains some additional properties unique for this web component.
```typescript
const billingEmail = this.app.create(EmailField).byId('billing_email');
billingEmail.setEmail('info@bellatrix.solutions');
```
Here we create the special text field control Email it contains some additional properties unique for this web component.
```typescript
const createAccountCheckBox = this.app.create(CheckBox).byId('createaccount');
await createAccountCheckBox.check();
```
You can check and uncheck checkboxes.
```typescript
const checkPaymentsRadioButton = this.app.create(RadioButton).byAttributeContaining('for', 'payment_method_cheque');
checkPaymentsRadioButton.click();
```
BELLATRIX finds the first RadioButton with attribute 'for' containing the value 'payment_method_cheque'. The radio buttons compared to checkboxes cannot be unchecked/unselected.

Full List of All Supported Web Components
---------------------------------------
### WebComponent ###
- create
- wait
- getAttribute
- scrollToVisible
- isVisible
- hover

**Note**: *All other components have access to the above methods and properties*

Component | Available properties
------------ | -------------
Anchor | click, getInnerHtml, getInnerText, getHref, getTarget, getRel
Button | click, getValue, isDisabled, getInnerText
CheckBox | check, uncheck, isChecked, isDisabled, getValue
Div | getInnerHtml, getInnerText
FileInput | upload, isRequired, isMultiple, getAccept
Frame | getName, getHref
Heading | getInnerText
Image | getSrc, getAlt, getHeight, getWidth, getSrcSet, getSizes, getLongDesc
Label | getInnerText, getInnerHtml, getFor, getRelatedElement
Option | isSelected, isDisabled, select, getInnerText, getValue
RadioButton | click, isChecked, isDisabled, getValue
Reset | click, isDisabled, getText, getValue
Select | getSelected, getAllOptions, selectByText, selectByIndex, selectByValue, isDisabled, isReadonly, isRequired
Span | getInnerText, getInnerHtml
TextArea | getInnerText, setText, isDisabled, isAutoComplete, isSpellCheck, isReadonly, isRequired, getPlaceholder, getMaxLength, getMinLength, getRows, getCols, getWrap
TextField | getInnerText, setText isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLength, getMinLength


### HTML5 Components: ### 

Component | Available properties
------------ | -------------
ColorInput | getColor, setColor, isAutoComplete, isDisabled, isRequired, getList, getValue
DateInput | getDate, setDate, isAutoComplete, isDisabled, isReadonly, isRequired, getMin, getMax, getStep, getValue
DateTimeInput | getTime, setTime, isAutoComplete, isDisabled, isReadonly, isRequired, getMin, getMax, getStep, getValue
EmailField | getEmail, setEmail, isAutoComplete, isDisabled, isReadonly, isRequired, getMinLength, getMaxLength, getPlaceholder, getSizeAttribute, getValue, getPattern
MonthInput | getMonth, setMonth, isDisabled, isAutoComplete, isReadonly, isRequired, getMax, getMin, getStep, getValue
NumberInput | getNumber, setNumber, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMax, getMin, getStep, getValue
Output | getText, getHtml, getFor
PasswordField | getPassword, setPassword, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
PhoneField | getPhone, setPhone, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue, getPattern
Progress | getMax, getText, getValue
RangeInput | getRange, setRange, isDisabled, isAutoComplete, isRequired, getList, getMax, getMin, getStep, getValue
SearchField | getSearch, setSearch, IsDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
TimeInput | getTime, setTime, getHover, getFocus, isDisabled, isAutoComplete, isReadonly, getMax, getMin, getStep, getValue
UrlField | getUrl, setUrl, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
WeekInput | getWeek, setWeek, isDisabled, isAutoComplete, isReadonly, getMax, getMin, getStep, getValue

