---
title: "Ewwii v0.3.0: Introduction of GTK4"
description: "A full explanation of what changes have occoured."
date: 2025-10-04
---

Ewwii v0.3.0 marks a new beginning where ewwii is faster, more performant, and more animatable. But everything comes with its own pros and cons. This article goes into detail on this release, on new issues, and on recommended things that you can do to mitigate these new issues.

## Release Overview

First of all, let's get into the release. Not all the changes are listed here, but the important are.

## New Features

### `can_target` property

A new property called `can_target` has been introduced to all widgets. This property which makes it unable to receive pointer / targetable events.

### `icon` widget

A new widget which maintains 1:1 ratio no matter what. It is particularly useful when the user has to show icons that doesn't automatically resize to fit the full container.

## Changed Features

### Dynamic updates

The dynamic runtime engine's widget creation / removal logic has been tweaked to match the new GTK4 expectations. As a widget cannot be downcasted to a container, the type of the widget is now matched against a list of container widgets and the supposed function is executed.

### Window positioning logic

GTK4 removed the ability to move a window for cross-platform compatibilty but it is a kick to the guts in ewwii as it relies on these logic.

No changes were needed for the wayland backend as the window positioning logic is handled by the wlr-layer-shell protocol. But X11 requires a message to the X11 server for it to position a window. Hence, the window positioning logic has moved to a function that manually sends a request to the X11 server.

### EventBox widget

The eventbox widget has been fully rewritten to match GTK4 expectations. In GTK4, there is actually no eventbox widget as there are controllers which can be attached to any widget. However, adding an option to add controllers to every single widget will be hard to maintain and hence ewwii retains an eventbox widget that is just a box with controllers attached.

### Initialization logic of windows

The intialization logic of windows changed from asking GTK to create the window and position it to an x, y coordinate to just asking GTK to create the window. This change is done as we are handling the positioning logic after the initialization of the windows.

## Removed Features

### CSS class on the windows

Previously, ewwii attached the name of the window as the window's and its root child's css class. This has been done as GTK4 seems to be causing unexpected behaviour with applying CSS that changes layout on X11.

Now only the root child inherits a css class that is the name of its parent window as it ensures compatibility across wayland and x11.

This change doesn't break anything as styling the root child works exactly as styling the root window.

### `icon_name` property from `image` widget

The `icon_name` property has been removed from the `image` widget i nfavour of the `icon` widget. This property is accessable on the `icon` widget.

### `same_size` property from stack widget

GTK4 removed the underlying API that is called by this property and ewwii had no option other than removing it. The `box` widget already allows doing the same thing and this widget can be used as a replacement.

### `icon_size` property from image widget

GTK4 also removed the underlying API that this property relies on. CSS can be used instead of setting the size of the image widget instead.

### Centerbox widget

The `centerbox` widget has also been removed as a part of the migration to GTK4 as a `box` can be used to implement the same functionality.

## The New Issues

Migrating a full GTK3 based application to GTK4 comes with a lot of issues and of course, Ewwii now has new problems.

### Resource Usage

GTK4 is GPU accelerated and thus needs more resources to set up. Moreover, the rendering backends also use a lot of resources due to the new changes. In testings, RAM usage has been found to spike to around 200MB very easily on certain hardware.

This is a massive trade off for the new performance and hot startup boosts ewwii previous had and posses a great challenge towards the lightweight nature of ewwii.

#### The solution

Luckly, there is a solution to this problem. GTK4 allows switching the GSK backends. So, this means that we can use the old cario based backend (which now also benifits from GPU acceleration) to get a stable resource usage.

```bash
GSK_RENDERER=cairo ewwii open foo
```

This command can be used to set the rendering backend to cario and get a stable resource usage with reasonable RAM usage until ewwii moves to its own custom renderer which is planned to be developed in the near future.

### X11 Compatibility

Another critical issue that may make ewwii unreliable on X11 in the future. GTK4 is built with wayland in mind and it is clearly reflected by the removal of X11 related functions.

One of the major problems while porting ewwii to GTK4 was GTK4 didn't have those convinient GTK3 features that send requests to the X11 server to perform actions like making a window sticky.

However, ewwii aims to keep its compatible nature by manually implementing each one of those convinient features that GTK3 had. Most of the time of porting to GTK4 was spend on this as the window creation architecture had to be changed completely.

## Conclusion

The migration to GTK4 is a major architectural change that breaks some compatibility with the previous versions. Even though this migration introduced increased resource usage, ewwii is actively being developed to be as light as possible without scarificing any of its features.
