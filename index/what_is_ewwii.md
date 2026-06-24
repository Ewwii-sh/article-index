---
title: "What is ewwii?"
description: "An explanation on what ewwii is and is not."
date: 2025-09-08
---

Ewwii is a fork of [eww (Elkowars Wacky Widgets)](https://github.com/elkowar/eww) rewritten to be more powerful, flexible, and extensible.

### What seperates them?

"Configuration Language and Architecture"

Ewwii uses [Rhai](https://rhai.rs), an imperative language, while eww uses Yuck, a declarative language. Rhai is way powerful than yuck due to its imperative language but may be hard for users who are familiar with yucks lisp-like syntax.

That said, let's move on to the main difference--their architecture. By nature, ewwii is fully dynamic. It supports creating, destroying, and updating the widgets on the fly while eww only supports updating the widgets. This enables ewwii to do a lot of things that eww couldnt before.

### What ewwii is

Ewwii is a Widget System built on GTK3. It is meant to be fast, dynamic, powerful, and extensible.

You can use ewwii to do many complex things!

- Build an application? Sure
- Integrate with any programming language? Yes, via [stdout](https://en.wikipedia.org/wiki/Standard_streams).
- Build a Wifi Manager? Sure. You can even checkout [ewifi_gui_template](https://github.com/Ewwii-sh/ewifi_gui_template).

### What ewwii is not

Ewwii **is not a shell toolkit**--it's a widget system.

Ewwii can't **yet** be used to build lockscreens, complex UI elements, etc. So, ewwii will be able to do a range of things that eww couldn't. But it is not yet the level of a full shell toolkit which it aspire to be.

### Should you use ewwii?

This is a question that is **not** meant to be included here, but is something that concerns people who are approach ewwii.

And, to be honest, there is no answer to this question. It is **you** that should answer this question.

Ewwii might be better/worse in comparison with other widget systems. But it is **you the user** who decides which **tool** you will use.

### Conclusion

In short, we can say that ewwii is significantly different from eww that has its own set of limitations which it aims to overcome.

> Ewwii is not a rebellion against eww. It is something that stands on it, aiming for the skies
> -- me
