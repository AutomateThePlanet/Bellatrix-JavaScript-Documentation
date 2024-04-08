---
layout: default
title:  "JavaScript Service"
excerpt: "Learn how to use BELLATRIX JavaScript service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/javascript-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```typescript
@TestClass
class JavaScriptServiceTests extends WebTest {
    @Test
    async fillUpAllFields() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/my-account/');

        await this.app.script.execute(`document.getElementById('username').value = 'BELLATRIX';`);

        await this.app.create(PasswordField).byId('password').setPassword('Gorgeous');
        const button = this.app.create(Button)byClassContaining('woocomerce-Button button');

        await button.evaluate('el => el.click();');
    }

    @Test
    async getElementStyle() {
        await this.app.navigation.navigate('https://demos.bellatrix.solutions/my-account/');

        const resultsCount = this.app.create(WebComponent).byClassContaining('woocomerce-result-count');
        const fontSize = await resultsCount.evaluate<string>('el => el.style.font-size;');

        Assert.areEqual(fontSize, '14px');
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier execution of JavaScript code using the **script** property. You need to make sure that you have navigated to the desired web page.
```typescript
await this.app.script.execute(`document.getElementById('username').value = 'BELLATRIX';`);
```
Execute a JavaScript code on the page. Here we find an element with id = 'firstName' and sets its value to 'BELLATRIX'.
```typescript
await button.evaluate('el => el.click();');
```
There is no need to use the script service, as every component has evaluate() method that automatically adds the wrapped element as an argument.
```typescript
const fontSize = await resultsCount.evaluate<string>('el => el.style.font-size;');
Assert.areEqual(fontSize, '14px');
```
Get the results from a script. After that, get the value for a specific style and assert it.