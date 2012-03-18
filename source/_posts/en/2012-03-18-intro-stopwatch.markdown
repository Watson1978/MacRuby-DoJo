---
layout: post
title: "Creates a Stopwatch Application"
date: 2012-03-18 00:01
comments: true
sharing: true
categories: MacRuby
---

In this content, how to create a simple application. This content may have an essence to create an application using MacRuby.

An application's User Interface will be designed in like a below. We will call this application "StopWatch".

![image](/images/en/intro-stopwatch/stopwatch.png)

"StopWatch" has below behaviors.

1. Run a timer if start button is clicked.
2. Stop a timer if stop button is clicked.
3. A timer value is displayed in Text Field.


## Design User Interface
To start the design, need to some operations in Xcode.

Choose a *MainMenu.xib*.

![image](/images/en/intro-stopwatch/mainmenu_xib.png)

Choose a [Window - StopWatch] to display Window for design.

![image](/images/en/intro-stopwatch/window_stopwatch.png)

Click an icon in toolbar in like a below. Then, Object Library is displayed.

![image](/images/en/intro-stopwatch/show_object_library.png)

You have been completed to prepare to design. Then, place the User Interface parts from Object Library on Window.

![image](/images/en/intro-stopwatch/ui_design.png)


## Connect the Outlets
"StopWatch" application sets a timer value into Text Field. However, if Text Field is just placed on Window, cannot set a value into there. We should use the Outlets to set a value, or to get a status of User Interface parts.

To use the Outlets, need to write a program code. Choose a *AppDelegate.rb*, then add a `attr_accessor :textField` to AppDelegate class.

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # Outlet
```

Return to *MainMenu.xib* screen, connect the Outlet. Press <kbd>control</kbd> key, and drag from App Delegate to Text Field.

![image](/images/en/intro-stopwatch/connect_outlet.png)

A list of Outlets will be displayed, then choose a `textField` to connect it.

![image](/images/en/intro-stopwatch/outlets.png)

You are able to set/get the value of the Text Field through the `textField` accessor.


## Connect the Actions
When you clicked start/stop button, it happen nothing yet. You need to set the behaviors when the buttons are clicked.

Choose an *AppDelegate.rb*, then add `startTimer` and `stopTimer` methods as following.

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # Outlet
  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end

  def startTimer(sender)
    # This method is called when clicked start button.
  end

  def stopTimer(sender)
    # This method is called when clicked stop button.
  end

end
```

Return to *MainMenu.xib* screen, connect the Actions. Press control key, and drag from start button to App Delegate.

![image](/images/en/intro-stopwatch/connect_action.png)

A list of Actions will be displayed, then choose a `startTimer` to connect it.

![image](/images/en/intro-stopwatch/actions.png)

Connect stop button to `stopTimer` in the same way. Each methods will be called when the button is clicked.

`stopTimer`/`startTimer` are also known as the action method.

<div class="note">
When you write an action method, you <strong>must</strong> provide a <code>sender</code> argument. If methods does not have a <code>sender</code> argument, it is not recognized as an action method.
</div>


## Use the timer
If you do something at a constant interval, use the [NSTimer](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/Foundation/Classes/nstimer_Class/Reference/NSTimer.html).

When use a timer as shown below, you will be able to handle at a constant interval.

```ruby
@timer = NSTimer
           .scheduledTimerWithTimeInterval(0.1,
                                           target: self,
                                           selector: "timerHandler:",
                                           userInfo: nil,
                                           repeats: true)

def timerHandler(userInfo)
  # Handler
end
```

Generate a timer at startTimer and stop a timer at stopTimer.

```ruby
class AppDelegate
  attr_accessor :window
  attr_accessor :textField # Outlet
  def applicationDidFinishLaunching(a_notification)
    # Insert code here to initialize your application
  end

  def startTimer(sender)
    if @timer.nil?
      @time = 0.0
      @timer = NSTimer
                .scheduledTimerWithTimeInterval(0.1,
                                                target: self,
                                                selector: "timerHandler:",
                                                userInfo: nil,
                                                repeats: true)
    end
  end

  def stopTimer(sender)
    if @timer
      @timer.invalidate
      @timer = nil
    end
  end

  def timerHandler(userInfo)
    @time += 0.1
    string = sprintf("%.1f", @time)
    textField.setStringValue(string)
  end
end
```

Invoke `@timer.invalidate` to stop a timer. Invoke     `textField.setStringValue(string)` to set string to Text Field.


"Stopwatch" application is complete!

Change to "StopWatch" from "Deployment" in Scheme. Then, Click [Run]. "Stopwatch" application will be running!!

![image](/images/en/intro-stopwatch/scheme.png)