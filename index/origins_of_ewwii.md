---
title: "Origins of Ewwii"
description: "Exploring early days of ewwii, why it was created, and early design choices."
date: 2026-05-26
---

During mid July, 2025 I was deep into working on a desktop environment called **Theom**. It was a simple and lightweight desktop environment that I created as a contribution to [AxOS](https://axos-project.com). Theom's architecture revolved around each component being its own independant software instead of most of the components being powered by a single tool.

Almost all of the GUI based components (like the dashboard) used [flutter](https://flutter.dev/) as the framework. With flutter, it is extremely easy to make good looking applications thanks to its focus on [M3E](https://m3.material.io/blog/building-with-m3-expressive). But, it had a downside--it was created for Android. On Linux, flutter has a noticeably slow on startup. Often showing a blank window for *~1-2* seconds before showing the contents. For a desktop environment with focus on lightweightness, this was unacceptable. I needed to find a new tool that I can use to build most of the core GUI applications like the dashboard and the settings.

At that time, two of the status bars that Theom provided used [polybar](https://polybar.github.io) and [eww](https://elkowar.github.io/eww). Since eww was a more flexible widget system, I decided to use it to build the dashboard. Unfortunately, I didn't get the hang of yuck, the configuration language of eww. I spend days on the dashboard and I kept hitting the same problems:

1. Yuck was hard to read as I've never used a lisp-like language before.
2. It was hard to create complex and dynamic widgets with lots of moving pieces.

And during my misery, I got the bold idea of forking eww and fixing the problems myself. But there were two problems:

1. I don't know rust.
2. I'm not familiar with the codebase.

After thinking for a while, I decided to go through with the idea to at least see how far I'll go. And thus began my ultimate misery (you'll see why).

## Journey to Compilation

I went into the codebase with nothing but the goal of replacing yuck with something else. Something else that I can understand. And so I removed two crates from the codebase: `yuck` and `simplexpr`. Little did I know, this unleashed final boss of Rust.

I tried compiling the project to see how much work I need to put into it. And was immediately bombarded by **350+ errors**. I was flabbergasted seeing that and I immediately quit working on the project. But after debating to myself, I ended up on the conclusion that I need to continue working on it. I really needed eww without yuck because eww was a really lightweight widget system and a perfect match to my lightweight desktop environment. Surely I only needed to solve 350+ errors, right?

And I began work. I pulled up the [Rust Book](https://doc.rust-lang.org/stable/book/) and ChatGPT and solving each error one by one. It wasn't the best experience, but the rust compiler was very helpful. It told me what to do when I was hit with simple errors.

The errors were extremely annoying though. I remember myself getting mad at how deeply yuck is coupled into eww. What do you mean the Window Definition and Widget Definition is inside yuck and not eww? What do you mean the properties of a window are evaluated by something called "Simplexpr". Why is yuck more than just a parser but a core component of eww? I was annoyed a lot, but I kept going.

Slowly but surely, in a week, I got the number of errors down to like 30, I believe? And then I solved that one error. And the second wave of errors hit me. The error count which was just at 30 was now at 120+. I was mad and wanted to quit, but I just couldn't because I already spend a week of my life on this project. And quitting was like accepting defeat.

And then I slowly but surely got the number of errors down to like 8. You see where its going? *I solved that one error and was back at 120+ errors*. But again, like the previous I managed to get it to a very low number. With each compilation, I expected to be bombarded with a lot of errors again. But nothing happened. And in no time, I was one error away from freedom. One error away from actual **feedback loop**. I fixed the erorr and braced for the worst. But it compiled. The misery was over and the project actually worked. Sure, it was buggy but it could show a label (the only widget that ewwii supported at that time) without crashing.

I was thrilled. I finally got what I wanted after two weeks of work. And the best part? This transformed me completely. I started appreciating rust and enjoyed programming in it. Everything since this point was a breeze. I iterated fast and added all the features I wanted.

## Design Choices

During the journey to the first compilation, I made many design choices--include the configuration language. At first, I didn't even know rhai existed. Instead, I was planning to embed lua. But during that time, I believe I used a modern `rustc` version which the lua crate was not compatible with. It gave some weird errors and I just stopped trying to embed lua because at that time, I wanted nothing but to get this project over with.

Then I found rhai, and it looked easier to embed than lua. And so I made up my mind to embed that and get it over with. I worked on the ideal configuration style that can be achieved for like two hours and came up with this:

```js
enter([
  // Add all defwindow inside enter. Enter is the root of the config.
  defwindow(
    "example",
    #{
      monitor: 0,
      windowtype: "dock",
      stacking: "fg",
      wm_ignore: false,
      geometry: #{
        x: "0%",
        y: "2px",
        width: "90%",
        height: "30px",
        anchor: "top center",
      },
      exclusive: true,
      reserve: #{ distance: "40px", side: "top" },
    },
    label(#{ text: "example content" })
  ),
]);
```

Sure, it was not the prettiest, but it did the job, and also looked like the best configuration style I could achieve with rhai.

And another design choice I made regarding rhai was including the dyanamic configuration evaluation feature, which in theory is great and allows dynamic widget creation/destruction, but hurts the performance very badly and is extremely hard to build correctly (I had to build a diffing system to save performance but they made things complicated and there is still a fundamental performance cost to this design).

## Conclusion

Ewwii didn't have the best start. And maybe that is a skill issue. But it surely did make me a programmer that I never was before. And honestly, this is, if not the most painful, yet rewarding thing I've ever done. And the only other thing that ever comes close to this experience was the migration to gtk4, which introduced **200+ errors** and was a toolkit which I was not faimilar with.

Regarding the early design choices, I ain't too pround of them because they weren't the greatest and I had to change them later on and infact required a lot more effort to change because the eww codebase was once rewritten by a Rust newbie.

Anyhow, ewwii is doing best that ever and I hope it can grow well and fast. Peace!
