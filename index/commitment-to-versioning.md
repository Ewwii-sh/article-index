---
title: "Committing to interface stability with v1.0.0"
description: "I believe its about time for ewwii to reach the 'v1.0.0' milestone."
date: 2026-09-13
---

I'm writing this article to announce that ewwii will be commiting to interface stability by going v1.0.0 next release. 
It's a big milestone and I'm very happy we reached this far despite being a small community. Ewwii has reached a level
of *stability* that I never thought it would reach. "Stability" in terms of architecture rather than eradication of all bugs.

I'd like to believe that ewwii has been architecturally stable since the v0.9.0 release, with efforts towards stability starting
mainly from v0.5.0 onwards. Despite ewwii being stable since v0.9.0, the commitment to stability was delayed in hopes
of eleminating all bugs before doing so. But every feature is a new potential bug introduced. Unless ewwii slowed down the number
of new features added and fixed more bugs, this would be a chase without an end. Moreover, ewwii needs to be thoroughly tested
to discover new bugs, which is a demand I cannot keep up with.

## New Features in v1.0.0

That said, I would like to show some features that would be introduced in v1.0.0.

### Better Hot Reloading

Like eww, ewwii too relied on automatically closing and reopening windows in the development workflow. However, ewwii does not 
have to do that. Since the *rhai era* of ewwii, the foundation for keeping track of widgets based on unique identifiers already 
existed. And an 'hot reloading' mechanism already used to exist within ewwii, which was later discarded in the v0.5.0 as part of 
the simplification of rhai, and eventually the switch to nbcl.

Ewwii brings this mechanism back and gives it a clear purpose of dynamically diffing the changes and only updating what actually
changed, which suits ewwii's philosophy of being lightweight and efficient (probably should write an article on that).

### NBCL Callbacks

An `nbcl` function for automatically generating a bash command that will execute the provided NBCL lambda function.
This can be attached to a widget's *event property* (those onclick, onchange, etc.) to have it run an NBCL code when triggered.

Here is an example of how it would look like:

```nbcl
Button {
    onclick = nbcl(|| {
        print("Hello, World!")
    })
}
```

### Adwaita Powered Animations

I am working on brining libadwaita to ewwii to use the animation capabilities of it. The current `Animation` widget 
prototype with the adwaita backend already looks really good with a lot less code. So I'll try to use adwaita 
wherever it can optimize something.

If that goes well, then I will probably add adwaita widgets to ewwii. I know most of those have good animations.

### Probability of New Data Source

I've always thought that `Poll`, and `Listen` were too clunky to work with ever since I started working on ewwii. 
Would be nice if I could find a nice little middle ground data source that is highly convenient to work with. I really
did like the direction I was going to with JsCore but implementing the similar functionality into ewwii would break existing
configuration (which I hate to do!!!).

Most likely `Poll` and `Listen` will stay and no new data source will be introduced. But I'm still looking for new options.
You could pretty much say that the 'NBCL Callback' idea was introduced because of the same reason, but I'm still to see
what people think about it. `Script` is... just a failed experiment at this point. I don't care about it anymore. It exists
because it exists.

## Bug Fixes in v1.0.0

Ewwii's X11 backend is not as mature as the Wayland one, which resulted in many bugs originating there. The v1.0.0 release
will mainly focus on solving these X11 related bugs. Beyond that, I also aim to optimize the engine (if I can) for better 
performance, however that isn't really a "bug fix", and most likely will not be a big improvement.

## Probability for Detailed Examples

There is a probability that I will work on more detailed examples for ewwii. Like a full desktop shell powered by ewwii. 
I did do one of those before, but I don't have time to maintain it anymore. And it's pretty much also a fact that I suck at 
designing UI. Someone did ask me regarding this, I forgot who it was, but that is the main reason I am thinking of doing this.

## Conclusion

I'm too lazy to write a conclusion. "Commiting to interface stability with v1.0.0" thats pretty much it. Bye. Oh, did you know
that I am redesigning the ewwii website? Probably still looks trash... Bye again.
