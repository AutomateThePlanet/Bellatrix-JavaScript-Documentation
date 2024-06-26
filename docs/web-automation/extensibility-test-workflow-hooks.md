---
layout: default
title:  "Extensibility – Plugin Hooks"
excerpt: "Learn how to extend the BELLATRIX plugins using hooks."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-test-workflow-hooks/
# anchors:
#   example: Example
#   explanations: Explanations
---
Coming Soon
-------

<!-- Example
-------
```java
public class TestWorkflowHooksTests extends WebTest {
    private static Select sortDropDown;
    private static Anchor protonRocketAnchor;

    @Override
    public void beforeClass() {
        // Executes a logic once before all tests in the test class.
    }

    @Override
    public void beforeMethod() {
        sortDropDown =
                app().create().byXPath(Select.class, "//*[@id='main']/div[1]/form/select");
        protonRocketAnchor =
                app().create().byXPath(Anchor.class, "//*[@id='main']/div[2]/ul/li[1]/a[1]");

        app().navigate().to("http://demos.bellatrix.solutions/");

        sortDropDown.selectByText("Sort by price: low to high");
    }

    @Override
    public void afterMethod() {
        // Executes a logic after each test in the test class.
    }

    @Override
    public void afterClass() {
        // Executes a logic once after all tests in the test class.
    }

    @Test
    public void sortDropDownIsAboveOfProtonRocketAnchor() {
        sortDropDown.above(protonRocketAnchor).validate();
    }

    @Test
    public void sortDropDownIsAboveOfProtonRocketAnchor_41px() {
        sortDropDown.above(protonRocketAnchor).equal(41).validate();
    }

    @Test
    public void sortDropDownIsAboveOfProtonRocketAnchor_GreaterThan40px() {
        sortDropDown.above(protonRocketAnchor).greaterThan(40).validate();
    }

    @Test
    public void sortDropDownIsAboveOfProtonRocketAnchor_GreaterThanOrEqual41px() {
        sortDropDown.above(protonRocketAnchor).greaterThanOrEqual(41).validate();
    }

    @Test
    public void sortDropDownIsNearTopOfProtonRocketAnchor_GreaterThan40px() {
        sortDropDown.topInside(protonRocketAnchor).greaterThan(40).validate();
    }
}
```

Explanations
------------
One of the greatest features of BELLATRIX is test workflow hooks. It gives you the possibility to execute your logic in every part of the test workflow. Also, as you can read in the next chapter write plugins that execute code in different places of the workflow every time. This is happening no matter what test framework you use – TestNG or JUnit.

**BELLATRIX Default Test Workflow.**

The following methods are called once for test class:


1. Current class **configure** logic executes, registering all plugins.
2. All plugins **preBeforeClass** logic executes.
3. Current class **beforeClass** method executes. By default it is empty, but you can override it in each class and execute your logic. This is the place where you can set up data for your tests, call internal API services, SQL scripts and so on.
4. All plugins **postBeforeClass** logic executes.

In case there is an exception thrown in one of the above phases **beforeTestFailed** logic of all plugins is run.

5. All plugins **preAfterClass** logic executes.
6. Current class **afterClass** method executes. By default it is empty, but you can override it in each class and execute your logic. This is the place where you can execute cleanup scripts after all tests have finished executing.
7. All plugins **postAfterClass** logic executes.

In case there is an exception thrown in one of the above phases **afterClassFailed** logic of all plugins is run.

The following methods are called once for each test in the class:

1. All plugins **preBeforeTest** logic executes.
2. Current class **beforeMethod** method executes. By default it is empty, but you can override it in each class and execute your logic. You can add some logic that is executed for each test instead of copy pasting it for each test. For example – navigating to a specific Android activity.
3. All plugins **postBeforeTest** logic executes.

In case there is an exception thrown in one of the above phases **beforeTestFailed** logic of all plugins is run.

4. All plugins **preAfterTest** logic executes.
5. Current class **afterMethod** method executes. By default it is empty, but you can override it in each class and execute your logic.
You can add some logic that is executed after each test instead of copy pasting it. For example – deleting some entity from DB.
6. All plugins **postAfterTest** logic executes.

In case there is an exception thrown in one of the above phases **afterTestFailed** logic of all plugins is run.

```java
@Override
public void beforeMethod() {
    sortDropDown =
            app().create().byXPath(Select.class, "//*[@id='main']/div[1]/form/select");
    protonRocketAnchor =
            app().create().byXPath(Anchor.class, "//*[@id='main']/div[2]/ul/li[1]/a[1]");

    app().navigate().to("http://demos.bellatrix.solutions/");

    sortDropDown.selectByText("Sort by price: low to high");
}
```
This is one of the ways you can use **beforeMethod**. You can find create all elements in the **TestsArrange**, create all necessary data for the tests and execute the actual tests logic but without asserting anything. Then in each separate test execute single assert or validate method. Following the best testing practices – having a single assertion in a test. If you execute multiple assertions and if one of them fails, the next ones are not executed which may lead to missing some major clue about a bug in your product. Anyhow, BELLATRIX allows you to write your tests the standard way of executing the primary logic in the tests or reuse some of it through the usage of **beforeMethod** and **afterMethod** methods.
```java
@Override
public void beforeMethod() {
    // ...
}
```
Executes a logic before each test in the test class.
```java
@Override
public void afterMethod() {
    // ...
}
```
Executes a logic after each test in the test class. -->