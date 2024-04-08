---
layout: default
title:  "Locate Components"
excerpt: "Learn how to locate web components with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/locate-components/
anchors:
  example: Example
  explanations: Explanations
  available-create-methods: Available create Methods
  find-multiple-components: Find Multiple Components
  available-createall-methods: Available createAll Methods
  find-nested-components: Find Nested Components
  available-create-methods-for-finding-nested-components: Available Nested create Methods
  available-createall-methods-for-finding-nested-components: Available Nested createAll Methods
---
Example
-------
```typescript
    @Test
    async promotionsPageOpened_When_PromotionsButtonClicked() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

        const promotionsLink = this.app.create(Anchor).byLinkText('Promotions');

        await promotionsLink.click();

        console.log(promotionsLink.getFindStrategy().getValue());

        console.log(promotionsLink.getWrappedElement().getTagName());
    }
```

Explanations
-------
```typescript
const promotionsLink = this.app.create(Anchor).byLinkText('Promotions');
```
There are different ways to locate components on the page. To do it you use the component create service.
You need to know that BELLATRIX has a built-in complex mechanism for waiting for components, so you do not need to worry about this anymore. Keep in mind that when you use the **create** methods, the component is not searched on the page. All components use lazy loading. Which means that they are searched once you perform an action or assertion on them. By default on each new action, the component is searched again and is refreshed.
```typescript
console.log(promotionsLink.getFindStrategy().getValue());
```
Because of the proxy component mechanism (we have a separate type of component instead of single **WebDriver/Playwright** **WebElement/Locator** interface) we have several benefits. Each control (component type – **ComboBox**, **TextField** and so on) contains only the actions you can do with it, and the methods are named properly.
Also, we have some additional properties in the proxy web control such as – **getFindStrategy**. Now you can get the locator with which you component was found.
```typescript
console.log(promotionsLink.getWrappedElement().getTagName());
```
You can access the native wrapped element through **getWrappedElement**.

Available create Methods
------------------------
BELLATRIX extends the vanilla Selenium and Playwright selectors and gives you additional ones.
### create().byIdEndingWith ###
```typescript
this.app.create(Anchor).byIdEndingWith('myIdSuffix');
```
Searches the component by ID ending with the locator.
### create().byTag ###
```typescript
this.app.create(Anchor).byTag('a');
```
Searches the component by its tag.
### create().byId ###
```typescript
this.app.create(Button).byId('myId');
```
Searches the component by its ID.
### create().byIdContaining ###
```typescript
this.app.create(Button).byIdContaining('myIdMiddle');
```
Searches the component by ID containing the specified text.
### create().byValueContaining ###
```typescript
this.app.create(Button).byValueContaining('pay');
```
Searches the component by value attribute containing the specified text.
### create().byXpath ###
```typescript
this.app.create(Button).byXpath('//*title="Add to cart"');
```
Searches the component by XPath locator.
### create().byCss ###
```typescript
this.app.create(Button).byCss('[data-product_id*="28"]');
```
Searches the component by CSS locator.
### create().byLinkText ###
```typescript
this.app.create(Anchor).byLinkText('blog');
```
Searches the component by its link (href).
### create().byLinkTextContaining ###
```typescript
this.app.create(Anchor).byLinkTextContaining('account');
```
Searches the component by its link (href) if it contains specified value.
### create().byClass ###
```typescript
this.app.create(Anchor).byClass('product selected');
```
Searches the component by its CSS classes.
### create().byClassContaining ###
```typescript
this.app.create(Anchor).byClassContaining('selected');
```
Searches the component by its CSS classes containing the specified values.
### create().byInnerTextContaining ###
```typescript
this.app.create(Div).byInnerTextContaining('Showing all');
```
Searches the component by its inner text content, including all child HTML components.
### create().byNameEndingWith ###
```typescript
this.app.create(SearchField).byNameEndingWith('a');
```
Searches the component by its name containing the specified text.
### create().byAttributeContaining ###
```typescript
this.app.create(Anchor).byAttributeContaining('data-product_id', '31');
```
Searches the component by some of its attribute containing the specified value.

Find Multiple Components
----------------------
Sometimes we need to find more than one component. For example, in this test we want to locate all Add to Cart buttons.
To do it you can use the component create service **createAll** method.

```typescript
@Test
async checkAllAddToCartButtons() {
    await this.app.navigation.navigate('https://demos.bellatrix.solutions/');
    
    const addButtons = this.app.create(Anchor).allByXpath('//*[@normalize-space()="Add to cart"]');
}
```

Available createAll Methods
------------------------
### create().allByIdEndingWith ###
```typescript
this.app.createAnchorallByIdEndingWith('myIdSuffix');
```
Searches the components by ID ending with.
### create().allByTag ###
```typescript
this.app.create(Anchor).allByTag('a');
```
Searches the components by their tags.
### create().allById ###
```typescript
this.app.create(Button).allById('myId');
```
Searches the components by their IDs.
### create().allByIdContaining ###
```typescript
this.app.create(Button).allByIdContaining('myIdMiddle');
```
Searches the components by ID containing the specified text.
### create().allByValueContaining ###
```typescript
this.app.create(Button).allByValueContaining('pay');
```
Searches the components by value attribute containing the specified text.
### create().allByXpath ###
```typescript
this.app.create(Button).allByXpath('//*title="Add to cart"');
```
Searches the components by XPath locator.
### create().allByCss ###
```typescript
this.app.create(Button).allByCss('[data-product_id*="28"]');
```
Searches the components by CSS locator.
### create().allByLinkText ###
```typescript
this.app.create(Anchor).allByLinkText('blog');
```
Searches the components by their link (href).
### create().allByLinkTextContaining ###
```typescript
this.app.create(Anchor).allByLinkTextContaining('account');
```
Searches the components by their link (href) if it contains specified value.
### create().allByClass ###
```typescript
this.app.create(Anchor).allByClass('product selected');
```
Searches the components by their CSS classes.
### create().allByClassContaining ###
```typescript
this.app.create(Anchor).allByClassContaining('selected');
```
Searches the components by their CSS classes containing the specified values.
### create().allByInnerTextContaining ###
```typescript
this.app.create(Div).allByInnerTextContaining('Showing all');
```
Searches the components by their inner text content, including all child HTML components.
### create().allByNameEndingWith ###
```typescript
this.app.create(SearchField).allByNameEndingWith('a');
```
Searches the components by their name containing the specified text.
### create().allByAttributeContaining ###
```typescript
this.app.create(Anchor).allByAttributeContaining('data-product_id', '31');
```
Searches the components by some of their attributes containing the specified value.

Find Nested Components
----------------------
Sometimes it is easier to locate one component and then find the next one that you need, inside it. For example in this test we want to locate the Sale! button inside the product's description. To do it you can use the component's create methods.

```typescript
@Test
async openSalesPage_When_locatedSaleButtonInsideProductImage() {
    await this.app.navigation.navigate('https://demos.bellatrix.solutions/');

    const productColumn = this.app.create(Option).byClass('products columns-4');

    const saleButton = productsColumn.create(Anchor).byXpath('//div[class*="woocommerce-LoopProduct-link woocommerce-loop-product__link"]').create(Button).byInnerTextContaining('Sale!');

    await saleButton.click();
}
```
The first products row is located. Then search inside it for the first product image, inside it search for the Sale! Span element.

**Note**: *It is entirely legal to create a Button instead of Span. BELLATRIX library does not care about the real type of the HTML elements. The proxy types are convenience wrappers so to say. Meaning they give you a better interface of predefined properties and methods to make your tests more readable.*

Available create Methods for Finding Nested Components
----------------------------------------------------
### create().byIdEndingWith ###
```typescript
component.create(Anchor).byIdEndingWith('myIdSuffix');
```
Searches the component by ID ending with the locator.
### create().byTag ###
```typescript
component.create(Anchor).byTag('a');
```
Searches the component by its tag.
### create().byId ###
```typescript
component.create(Button).byId('myId');
```
Searches the component by its ID.
### create().byIdContaining ###
```typescript
component.create(Button).byIdContaining('myIdMiddle');
```
Searches the component by ID containing the specified text.
### create().byValueContaining ###
```typescript
component.create(Button).byValueContaining('pay');
```
Searches the component by value attribute containing the specified text.
### create().byXpath ###
```typescript
component.create(Button).byXpath('//*title="Add to cart"');
```
Searches the component by XPath locator.
### create().byCss ###
```typescript
component.create(Button).byCss('[data-product_id*="28"]');
```
Searches the component by CSS locator.
### create().byLinkText ###
```typescript
component.create(Anchor).byLinkText('blog');
```
Searches the component by its link (href).
### create().byLinkTextContaining ###
```typescript
component.create(Anchor).byLinkTextContaining('account');
```
Searches the component by its link (href) if it contains specified value.
### create().byClass ###
```typescript
component.create(Anchor).byClass('product selected');
```
Searches the component by its CSS classes.
### create().byClassContaining ###
```typescript
component.create(Anchor).byClassContaining('selected');
```
Searches the component by its CSS classes containing the specified values.
### create().byInnerTextContaining ###
```typescript
component.create(Div).byInnerTextContaining('Showing all');
```
Searches the component by its inner text content, including all child HTML components.
### create().byNameEndingWith ###
```typescript
component.create(SearchField).byNameEndingWith('a');
```
Searches the component by its name containing the specified text.
### create().byAttributeContaining ###
```typescript
component.create(Anchor).byAttributeContaining('data-product_id', '31');
```
Searches the component by some of its attribute containing the specified value.

Available createAll Methods for Finding Nested Components
----------------------------------------------------
### create().allByIdEndingWith ###
```typescript
component.create(Anchor).allByIdEndingWith('myIdSuffix');
```
Searches the components by ID ending with.
### create().allByTag ###
```typescript
component.create(Anchor).allByTag('a');
```
Searches the components by their tags.
### create().allById ###
```typescript
component.create(Button).allById('myId');
```
Searches the components by their IDs.
### create().allByIdContaining ###
```typescript
component.create(Button).allByIdContaining('myIdMiddle');
```
Searches the components by ID containing the specified text.
### create().allByValueContaining ###
```typescript
component.create(Button).allByValueContaining('pay');
```
Searches the components by value attribute containing the specified text.
### create().allByXpath ###
```typescript
component.create(Button).allByXpath('//*title="Add to cart"');
```
Searches the components by XPath locator.
### create().allByCss ###
```typescript
component.create(Button).allByCss('[data-product_id*="28"]');
```
Searches the components by CSS locator.
### create().allByLinkText ###
```typescript
component.create(Anchor).allByLinkText('blog');
```
Searches the components by their link (href).
### create().allByLinkTextContaining ###
```typescript
component.create(Anchor).allByLinkTextContaining('account');
```
Searches the components by their link (href) if it contains specified value.
### create().allByClass ###
```typescript
component.create(Anchor).allByClass('product selected');
```
Searches the components by their CSS classes.
### create().allByClassContaining ###
```typescript
component.create(Anchor).allByClassContaining('selected');
```
Searches the components by their CSS classes containing the specified values.
### create().allByInnerTextContaining ###
```typescript
component.create(Div).allByInnerTextContaining('Showing all');
```
Searches the components by their inner text content, including all child HTML components.
### create().allByNameEndingWith ###
```typescript
component.create(SearchField).allByNameEndingWith('a');
```
Searches the components by their name containing the specified text.
### create().allByAttributeContaining ###
```typescript
component.create(Anchor).allByAttributeContaining('data-product_id', '31');
```
Searches the components by some of their attributes containing the specified value.