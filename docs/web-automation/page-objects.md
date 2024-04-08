---
layout: default
title:  "Page Objects"
excerpt: "Learn how to use the BELLATRIX page objects."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/page-objects/
anchors:
  introduction: Introduction
  non-page-object-test-example: Non-page-object Test Example
  how-to-create-bellatrix-page-object: How to Create BELLATRIX Page Object
  page-object-example: Page Object Example
  page-object-example-explanations: Page Object Example Explanations
  page-object-test-example: Page Object Test Example
  page-object-test-example-explanations: Page Object Test Example Explanations
---

Introduction
------------
As you most probably noticed this is like the 4th time we use almost the same elements and logic inside our tests. Similar test writing approach leads to unreadable and hard to maintain tests.
Because of that people use the so-called Page Object design pattern to reuse their elements and pages' logic. BELLATRIX comes with powerful built-in page objects which are much more readable and maintainable than regular vanilla ones.

Non-page-object Test Example
----------------------------
```typescript
@Test
async purchaseRocketWithoutPageObjects() {
    await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

    // Home page elements
    const sortDropDown = this.app.create(Select).byNameEndingWith('orderby');
    const protonMReadMoreButton = this.app.create(Anchor).byInnerTextContaining('Read more');
    const addToCartFalcon9 = this.app.create(Anchor).byAttributeContaining('data-product_id', '28');
    const viewCartButton = this.app.create(Anchor).byClassContaining('added_to_cart wc-forward');

    // Home page actions
    await sortDropDown.selectByText('Sort by price: low to high');
    await protonMReadMoreButton.hover();
    await addToCartFalcon9.click();
    await viewCartButton.click();

    // Cart page elements
    const couponCodeTextField = this.app.create(TextField).byId('coupon_code');
    const applyCouponButton = this.app.create(Button).byValueContaining('Apply coupon');
    const messageAlert = this.app.create(Div).byClassContaining('woocommerce-message');
    const quantityBox = this.app.create(NumberInput).byClassContaining('input-te4xt qty text');
    const updateCart = this.app.create(Button).byValueContaining('Update cart');
    const totalSpan = this.app.create(Span).byXpath(`//*[@class='order-total']//span`);
    const proceedToCheckout = this.app.create(Anchor).byClassContaining('checkout-button alt wc-forward');

    // Cart page actions
    await couponCodeTextField.setText('happybirthday');
    await applyCouponButton.click();
    await messageAlert.wait.toBeVisible();
    await messageAlert.validate('innerText').is('Coupon code applied successfully.');

    await quantityBox.setNumber(0);
    await quantityBox.setNumber(2);
    await updateCart.click();
    await totalSpan.validate('innerText').is('95.00€');
    await proceedToCheckout.click();

    // Checkout page elements
    const billingDetailsHeading = this.app.create(Heading).byInnerTextContaining('Billing details');
    const showLogin = this.app.create(Anchor).byInnerTextContaining('Click here to login');
    const orderCommentsTextArea = this.app.create(TextArea).byId('order_comments');
    const billingFirstName = this.app.create(TextField).byId('billing_first_name');
    const billingLastName = this.app.create(TextField).byId('billing_last_name');
    const billingCompany = this.app.create(TextField).byId('billing_company');
    const billingCountry = this.app.create(Select).byId('billing_country');
    const billingAddress1 = this.app.create(TextField).byId('billing_address_1');
    const billingAddress2 = this.app.create(TextField).byId('billing_address_2');
    const billingCity = this.app.create(TextField).byId('billing_city');
    const billingState = this.app.create(Select).byId('billing_state');
    const billingZip = this.app.create(TextField).byId('billing_postcode');
    const billingPhone = this.app.create(PhoneField).byId('billing_phone');
    const billingEmail = this.app.create(EmailField).byId('billing_email');
    const createAccountCheckBox = this.app.create(CheckBox).byId('createaccount');
    const checkPaymentsRadioButton = this.app.create(RadioButton).byAttributeContaining('for', 'payment_method_cheque');

    // Checkout page actions
    await billingDetailsHeading.wait.toBeVisible();
    await showLogin.validate('href').is('https://demos.bellatrix.solutions/checkout/#');
    await orderCommentsTextArea.scrollToVisible();
    await orderCommentsTextArea.setText('Please send the rocket to my door step!');
    await billingFirstName.setText('In');
    await billingLastName.setText('DeepThought');
    await billingCompany.setText('Automate The Planet Ltd.');
    await billingCountry.selectByText('Bulgaria');
    await billingAddress1.setText('bul. Yerusalim 5');
    await billingAddress2.setText('bul. Yerusalim 6');
    await billingCity.setText('Sofia');
    await billingState.selectByText('Sofia-Grad');
    await billingZip.setText('1000');
    await billingPhone.setPhone('+00359894646464');
    await billingEmail.setEmail('info@bellatrix.solutions');
    await createAccountCheckBox.check();
    await checkPaymentsRadioButton.click();
}
```

How to Create BELLATRIX Page Object
-----------------------------------
- On most pages, you need to define elements. Placing them in a single place makes the changing of the locators easy. It is a matter of choice whether to have action methods or not. If you use the same combination of same actions against a group of elements then it may be a good idea to wrap them in a page object action method. In our example, we can wrap the filling the billing info in such a method. 
- In the assertions file, we may place some predefined **validate** methods. For example, if you always check the same email or title of a page, there is no need to hardcode the string in each test. Later if the title is changed, you can do it in a single place. The same is true about most of the things you can assert in your tests.

Page Object Example
-------------------
### Page Methods File ###
```typescript
@Page(CartPageMap, CartPageAsserts)
export class CartPage extends WebPage<CartPageMap, CartPageAsserts> {
    protected override get url() {
        return 'https://demos.bellatrix.solutions/cart/';
    }

    protected override async waitForPageLoad() {
        await this.map.couponCodeTextField.wait.toExist();
    }

    async applyCoupon(coupon: string) {
        await this.map.couponCodeTextField.setText(coupon);
        await this.map.applyCouponButton.click();
        await this.app.browser.waitForAjax();
    }

    async increaseProductQuantity(productNumber: number, newQuantity: number) {
        if (productNumber > await this.map.quantityBoxes.count()) {
            throw new Error('There are less added items in the cart. Please specify smaller product number.');
        }
        
        const quantityBox = await this.map.quantityBoxes.get(productNumber - 1);
        
        await quantityBox.setText(newQuantity.toString());
        await this.map.updateCart.click();
        await this.app.browser.waitForAjax();
    }

    async clickProceedToCheckout() {
        await this.map.proceedToCheckout.click();
        await this.app.browser.waitUntilPageLoadsCompletely();
    }

    async getTotal() {
        return await this.map.totalSpan.getInnerText();
    }

    async getMessageNotification() {
        return await this.map.messageAlert.getInnerText();
    }
}
```
### Page Element Map File ###
```typescript
export class CartPageMap extends WebPageMap {
    get couponCodeTextField() {
        return this.create(TextField).byId('coupon_code');
    }

    get applyCouponButton() {
        return this.create(Button).byCss('[value*="Apply coupon"]');
    }

    get quantityBoxes() {
        return this.create(TextField).allByClassContaining('input-text qty text');
    }

    get updateCart() {
        return this.create(Button).byCss('[value*="Update cart"]');
    }

    get messageAlert() {
        return this.create(Div).byCss('[class*="woocommerce-message"]');
    }

    get totalSpan() {
        return this.create(Span).byXpath('//*[@class="order-total"]//span');
    }

    get proceedToCheckout() {
        return this.create(Button).byCss('[class*="checkout-button button alt wc-forward"]');
    }
}
```
### Page Asserts File ###
```typescript
export class CartPageAsserts extends WebPageAsserts<CartPageMap> {
    async couponAppliedSuccessfully() {
        await this.map.messageAlert.validate('innerText').is('Coupon code applied successfully.');
    }

    async totalPrice(expectedPrice: string) {
        await this.map.totalSpan.validate('innerText').is(`${expectedPrice}€`);
    }
}
```

Page Object Example Explanations
--------------------------------
```typescript
@Page(CartPageMap, CartPageAsserts)
export class CartPage extends WebPage<CartPageMap, CartPageAsserts>
```
All BELLATRIX page objects are implemented as a package with three different classes which means that you have separate files for different parts of it – actions, elements and assertions. This makes the maintainability and readability of these classes much better. Also, you can easily locate what you need. You can always create BELLATRIX page objects yourself by extending the WebPage class.
```typescript
protected override get url() {
    return 'https://demos.bellatrix.solutions/cart/';
}
```
Overriding the **url** getter that comes from the base page object you can later use the **open** method to go to the page.
```typescript
async applyCoupon(coupon: string) {
    await this.map.couponCodeTextField.setText(coupon);
    await this.map.applyCouponButton.click();
    await this.app.browser.waitForAjax();
}
```
Elements are accessed through the **map** property. These elements are always used together when coupon is applied. There are many test cases where you need to apply different coupons and so on. This way you reuse the code instead of copy-paste it. If there is a change in the way how the coupon is applied, change the workflow only here. Even single line of code is changed in your tests.
```typescript
async increaseProductQuantity(productNumber: number, newQuantity: number) {
    if (productNumber > await this.map.quantityBoxes.count()) {
        throw new Error('There are less added items in the cart. Please specify smaller product number.');
    }
    
    const quantityBox = await this.map.quantityBoxes.get(productNumber - 1);
    
    await quantityBox.setText(newQuantity.toString());
    await this.map.updateCart.click();
    await this.app.browser.waitForAjax();
}
```
Another method that we can add here is the one for updating the quantity of a product. This is an excellent place to put validations in your code. Here we make sure that the specified number of products that we want to update exists.
**create().allBy** method returns a ComponentsList<TComponent> in this case ComponentsList<TextField>. We can use all the default ComponentsList methods here – in our case, we use get() and count() methods.
```typescript
for (const quantityBox of await this.map.quantityBoxes.get()){
    quantityBox.setText(newQuantity.toString());
}
```
Since ComponentsList<TComponent>'s get() method returns an array of TComponent, we can use it directly in foreach statements.
```typescript
get couponCodeTextField() {
    return this.create(TextField).byId('coupon_code');
}
```
All elements are placed inside the file **Map** so that the declarations of your elements to be in a single place. It is convenient since if there is a change in some of the locators or elements' types you can apply the fix only here. All elements are implemented as properties.
```typescript
get quantityBoxes() {
    return this.create(TextField).allByClassContaining('input-text qty text');
}
```
If you want to find multiple elements, you can use ComponentsList<TComponent>.
```typescript
async totalPrice(expectedPrice: string) {
    await this.map.totalSpan.validate('innerText').is(`${expectedPrice}€`);
}
```
In the **Asserts** file, we have a method called **totalPrice**. We can access it through the **asserts** property, just like we used the **map** property above. With this validation, reuse the formatting of the currency. Also, since the method is called from the page, it makes your tests a little bit more readable.

Page Object Test Example
------------------------
```typescript
@Test
async purchaseRocketWithPageObjects() {
    const mainPage = await this.app.goTo(MainPage);
    await mainPage.addRocketToShoppingCart("Falcon 9");

    const cartPage = this.app.createPage(CartPage);
    await cartPage.applyCoupon("happybirthday");
    await cartPage.asserts.couponAppliedSuccessfully();
    await cartPage.increaseProductQuantity(1, 2);
    await cartPage.asserts.totalPrice("95.00");
    await cartPage.proceedToCheckout();

    const purchaseInfo: PurchaseInfo = {
        email: "info@berlinspaceflowers.com",
        firstName: "Anton",
        lastName: "Angelov",
        company: "Space Flowers",
        country: "Germany",
        address1: "1 Willi Brandt Avenue Tiergarten",
        address2: "Lützowplatz 17",
        city: "Berlin",
        zip: "10115",
        phone: "+498888999281",
    };

    const checkoutPage = this.app.createPage(CheckoutPage);
    await checkoutPage.fillBillingInfo(purchaseInfo);
    await checkoutPage.asserts.orderReceived();
}
```

Page Object Test Example Explanations
-------------------------------------
```typescript
const mainPage = await this.app.goTo(MainPage);
```
You can use the **app.goTo** method to navigate to the page and get a single instance of it.
```typescript
await mainPage.addRocketToShoppingCart("Falcon 9");
```
After you have the instance, you can directly start using the action methods of the page. As you can see the test became much shorter and more readable. The additional code pays off in future when changes are made to the page, or when you need to reuse some of the methods.
```typescript
const cartPage = this.app.createPage(CartPage);
```
Navigate to the shopping cart page by clicking the view cart button, so we do not have to call the GoTo method. But we still need an instance. We can get only an instance of the page through the **app.createPage** method.
```typescript
await cartPage.applyCoupon("happybirthday");
await cartPage.asserts.couponAppliedSuccessfully();
await cartPage.increaseProductQuantity(1, 2);
await cartPage.asserts.totalPrice("95.00");
await cartPage.proceedToCheckout();
```
Removing all elements and some implementation details from the test made it much more clear and readable. This is one of the strategies to follow for long-term successful automated testing.
```typescript
const purchaseInfo: PurchaseInfo = {
    email: "info@berlinspaceflowers.com",
    firstName: "Anton",
    lastName: "Angelov",
    company: "Space Flowers",
    country: "Germany",
    address1: "1 Willi Brandt Avenue Tiergarten",
    address2: "Lützowplatz 17",
    city: "Berlin",
    zip: "10115",
    phone: "+498888999281",
};

const checkoutPage = this.app.createPage(CheckoutPage);
await checkoutPage.fillBillingInfo(purchaseInfo);
```
You can move the creation of the data objects in a separate factory method or class.