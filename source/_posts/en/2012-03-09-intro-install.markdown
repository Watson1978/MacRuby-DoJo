---
layout: post
title: "Constructs the MacRuby environment"
date: 2012-03-09 21:58
comments: true
sharing: true
categories: MacRuby
---

## Requirements

When use the MacRuby, you would need to install the following softwares in advance.

- Mac OS X 10.6.8 later
- Xcode 4.2 or Xcode 4.3
- MacRuby
- BridgeSupport Preview 3 (If you use MacRuby on Mac OS X 10.6.8)

## Install Xcode
Install Xcode from the Mac App Store. Launch Mac App Store, and then, search by keywords of "Xcode" at search field on top of right corner.

![image](/images/en/intro-install/search_xcode.png)

You could install Xcode easily from the search results.

![image](/images/en/intro-install/xcode.png)

If you use Mac OS X 10.6.8, Xcode 4.2.x will be installed. And if you use Mac OS X 10.7.x, Xcode 4.3.x will be installed.

<div class="note">
<p>
If you have previously installed an older version (as Xcode 4.2.x) and installed Xcode 4.3.x at this time,
recommend to be running the following command on the Terminal.
</p>

```
$ sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer/
```
</div>


## Install Command Line Tools
Since Xcode 4.3, Command Line Tools (compiler, etc) became to additional package. Also would install Command Line Tools.

1. Select [Xcode]->[Preferences…] form Xcode's menu to display Preferences.
2. Select Downloads tab as shown in the figure below, and install Command Line Tools.

![image](/images/en/intro-install/command_line_tools.png)


# Install MacRuby
Next step, install MacRuby.

![image](/images/en/intro-install/macruby_org.png)

MacRuby 0.10 has been released now. However, unfortunately, it does not support for Xcode 4.3. Therefore, you should install the nightly build and could download it from [http://www.macruby.org/files/nightlies/](http://www.macruby.org/files/nightlies/). Nightly build which has latest changes has packaged every night.

![image](/images/en/intro-install/nightly_build.png)


# Install BridgeSupport Preview 3
**For Mac OS X 10.6.8 users only**, need to install BridgeSupport Preview 3. BridgeSupport is used to get information about the Framework provided by the Mac OS X. In Mac OS X 10.7 environment, it has been pre-installed. You could get the BridgeSupport Preview 3 from [http://www.macruby.org/files/](http://www.macruby.org/files/) as *BridgeSupport Preview 3.zip*.
