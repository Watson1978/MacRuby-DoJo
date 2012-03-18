---
layout: post
title: "Embeds the MacRuby into Application"
date: 2012-03-19 00:00
comments: true
sharing: true
categories: MacRuby
---

Application that you created has been worked environments only which MacRuby is installed. When you distribute your application, MacRuby may not have been installed in users environments.

Thefore, MacRuby is able to embed MacRuby into your application. The application which embeded MacRuby will work everywhere.

You choose "Deployment" and click [Run] as a following figure, then MacRuby is embedded into your application.

![image](/images/en/intro-deployment/deployment.png)

You can specify several options with "Deployment" in "Arguments" as a following figure.

![image](/images/en/intro-deployment/deployment_option.png)

Run a `macruby_deploy` command in Terminal.app, you could confirm the options.

```
$ macruby_deploy 
Usage: macruby_deploy [options] application-bundle
        --compile                    Compile the bundle source code
        --embed                      Embed MacRuby inside the bundle
        --no-stdlib                  Do not embed the standard library
        --stdlib [LIB]               Embed only LIB from the standard library
        --gem [GEM]                  Embed GEM and its dependencies
        --bs                         Embed the system BridgeSupport files
        --verbose                    Log all commands to standard out
        --codesign [CERT]            Sign the files with the specified certificate
    -v, --version                    Display the version
```