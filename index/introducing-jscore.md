---
title: "Introducing Jscore"
description: "Introducing Jscore, a JavaScript plugin for ewwii."
date: 2026-06-24
---

> https://ewwii-sh.github.io/jscore.ewwii

I am excited to introduce Jscore, a plugin that adds JavaScript support to ewwii. Not just "JavaScript", no its more than that.
Jscore redefines the flow of data within ewwii and replaces it with a stronger, and flexible architecture where your configuration itself 
produces the data instead of relying on external scripts and polling/listening them. Alongside this, jscore also comes with a lot
more features which this article will discuss.

## After Render System

Starting with the biggest highlight of jscore, the after render system. This is what allows configs to be self contained
within a jscore config without relying on external scripts.

```js 
export function after_render(api) {}
```

The `after_render` function is called immedeately after ewwii renders the window and it is within this function you 
update the widgets. The `api` contains all the functions that you would need to mutate the state of an ewwii widget.

Here is an example:

```js 

export function after_render(api) {
    // suppose we have a widget with name 'cool-box'
    const widget = api.find("cool-box");

    widget.set_property("orientation", "h");

    widget.add_class("red");
    widget.add_classes("green", "blue", "orange");

    widget.remove_class("green");
    widget.remove_classes("red", "blue", "orange");
}
```

You can use `async` in here too to update multile widgets at the same time. Or you can use the functions in `Tools.stream` like so:

```js 
import * as Tools from "ewwii/tools";

export function after_render(api) {
    const myLabel = api.find("cool-text");

    Tools.stream.time((t) => {
        console.log(`System ticked at: ${t.iso}`);
        myLabel.set_property("text", t.string);
    });
}
```

## Npm Compatibility

Jscore is compatible with `npm`, `pnpm`, `yarn`, etc. Basically anything that outputs the modules inside the `node_modules` file.
But there is a catch, the package you are trying to use should not rely on node exclusive features like `node:fs`.

Jscore needs to load the module as a single file from a readable location. For this it uses esbuild to quickly bundle it on the fly.
Jscore can do it automatically. Just make sure to append `esbuild:` to the name of your packages, otherwise it wont load.

```js 
import lodash from 'esbuild:lodash';

const arr = [1, 2, 3, 4, 5];
console.log(lodash.chunk(arr, 2));
console.log(lodash.sum(arr));
```

## Imperative Nature

Jscore embraces the imperative nature and makes the configuration reolves around it. Instead of defining how the UI should look,
you tell jscore how to show the UI.

```js 
import * as Widgets from "ewwii/widgets";

let myLabel = Widgets.Label("Hello, World!");
let myBox = Widgets.Box();

myBox.add_child(myLabel);

let myWindow = Widgets.Window("my_window");
myWindow.add_child(myBox);

Widgets.register(myWindow);
```

## Built-in Features and Tooling

Although jscore uses V8, a barebones JavaScript evaluator that lacks features like fetch, timers, etc. Jscore 
overcomes this limitation by implementing its own simple alternative to these features. This lets users tap into 
the familiar web JS style.

## Takeaway

Jscore is designed for people who need maximum power. Even if you prefer some other language, maybe give jscore a shot one day!
Its completely opposite to how configuration normally takes place in eww/ewwii.
