---
date: "2020-02-13"
title: Easily Mute and Unmute in Google Meet with Custom Shortcuts using Hammerspoon
tags:
- hacks
---

### Introduction

As a frequent Google Meet user, I often find myself in situations where I need to quickly mute and unmute my microphone or toggle my video on and off during meetings. In my quest to make these tasks more efficient, I discovered Hammerspoon, a powerful automation tool for macOS. In this blog post, I'll share a handy script I created using Hammerspoon to set up custom shortcuts for these common actions in Google Meet.

### Hammerspoon: A Brief Overview

Before diving into the script, let me tell you a bit about Hammerspoon. It's an open-source automation tool for macOS that allows you to write Lua scripts to control various aspects of your system. I've found Hammerspoon to be incredibly useful for customizing my workflow, automating repetitive tasks, and creating shortcuts. I highly recommend checking it out if you're a macOS user looking to enhance your productivity.

### My Google Meet Script

```lua
local function muteGoogleMeet(event)
    local keyCode = event:getKeyCode()
    local eventType = event:getType()

    if eventType == hs.eventtap.event.types.keyDown then
        local toggleFeature, shortcut

        if keyCode == hs.keycodes.map["F1"] then
            toggleFeature = "mute"
            shortcut = "d"
        elseif keyCode == hs.keycodes.map["F2"] then
            toggleFeature = "video"
            shortcut = "e"
        else
            return false
        end

        print("Toggling " .. toggleFeature .. " on Google Meet")
        hs.alert.show("Toggling " .. toggleFeature .. " on Google Meet")
        hs.application.launchOrFocus("Google Meet")
        hs.timer.doAfter(0.01, function()
            hs.eventtap.keyStroke({"cmd"}, shortcut)
        end)
        return true
    end

    return false
end

keyLogger = hs.eventtap.new({hs.eventtap.event.types.keyDown}, muteGoogleMeet)
keyLogger:start()
```

### Script Explanation

I developed this Hammerspoon script to create custom shortcuts for muting and unmuting my microphone, as well as toggling my video on and off during a Google Meet session. The script is simple but effective, allowing me to stay focused on my meetings instead of fumbling with the Google Meet interface.

The script starts with a function called 'muteGoogleMeet', which takes an 'event' parameter. This function retrieves the 'keyCode' and 'eventType' of the event. If a key has been pressed, the script initializes two variables: 'toggleFeature' and 'shortcut'. Depending on whether the 'F1' or 'F2' key is pressed, the script sets these variables to control the microphone or video, respectively.

After determining the desired action, the script displays a notification on the screen and brings Google Meet into focus. It then waits for a brief moment before sending a keyStroke event with the appropriate shortcut to perform the action in Google Meet.

The script concludes by creating an event tap named 'keyLogger', which listens for keyDown events and calls the 'muteGoogleMeet' function when a relevant key is pressed.

### Conclusion

By using Hammerspoon and my custom Google Meet script, I've been able to significantly improve my efficiency during virtual meetings. With just a couple of key presses, I can now quickly mute and unmute my microphone or toggle my video on and off, without having to navigate the Google Meet interface. If you're a macOS user who wants to streamline your Google Meet experience, I highly recommend giving Hammerspoon a try!

{{% notice info %}}
This blog post was written by chatgpt, and approved by me
{{% /notice %}}
