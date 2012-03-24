---
layout: post
title: "Changes an Application's Icon"
date: 2012-03-24 10:00
comments: true
sharing: true
categories: MacRuby
---

All applications which you created have a system default icon.

![image](/images/en/intro-icon/default_icon.png)

You can change the icon of the application to suit your needs.


## Icon Composer
Prepare the image for the icon to set for your application. Convert an image to icon with Icon Composer. Icon Composer is installed along with Xcode.

Since Xcode 4.3, launch Icon Composer with [Xcode]->[Open Developer Tool] in Xcode's menu.

![image](/images/en/intro-icon/icon_composer_xcode43.png)

Previous Xcode 4.2, Icon Composer exists in */Developer/Applications/Utilities/Icon Composer.app*

![image](/images/en/intro-icon/icon_composer_xcode42.png)

Some frames are shown in main screen of Icon Composer. Drag and drop prepared the image into there. After that, save it as icon.

![image](/images/en/intro-icon/icns.png)


## Change an Application's Icon
Change an Application's Icon with Xcode. Choose a [TARGETS] like the following figure. After that, drag and drop an icon which created with Icon Composer into [App Icon].

![image](/images/en/intro-icon/change_icon.png)