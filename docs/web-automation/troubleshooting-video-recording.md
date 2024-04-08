---
layout: default
title:  "Troubleshooting – Video Recording"
excerpt: "Learn how to use BELLATRIX cross-platform video recording."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/troubleshooting-video-recording/
# anchors:
#   example: Example
#   explanations: Explanations
#   configuration: Configuration
---
Coming Soon
-------

<!-- Example
-------
```java
public class FullPageScreenshotsOnFailTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");
        var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");
        promotionsLink.click();
    }
}
```

Explanations
------------
If it is turned on the engine will capture video for every test and keep them in case the test failed.

Configuration
-------------
If you open the **testFrameworkSettings.\<env\>.json** file, you find the screenshot properties under **webSettings** section that controls this behaviour.
```json
"webSettings": {
    "videosOnFailEnabled": "true",
    "videosSaveLocation": "user.home/BELLATRIX/Videos"
}
```
You can turn off the making of videos for all tests and specify where the videos to be saved. In the extensibility chapters read more about how you can create custom video recorder or change the saving strategy. -->