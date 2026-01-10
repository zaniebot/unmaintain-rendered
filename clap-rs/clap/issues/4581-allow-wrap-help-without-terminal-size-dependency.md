---
number: 4581
title: Allow wrap_help without terminal_size dependency
type: issue
state: open
author: mitsuhiko
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2022-12-27T22:13:50Z
updated_at: 2025-06-28T12:58:23Z
url: https://github.com/clap-rs/clap/issues/4581
synced_at: 2026-01-10T01:27:58Z
---

# Allow wrap_help without terminal_size dependency

---

_Issue opened by @mitsuhiko on 2022-12-27 22:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.30

### Describe your use case

I would like to use clap for terminal wrapping but I would like to avoid using `terminal_size` as dependency. I do not need to make it dependent on the terminal size / I have my own way to read the terminal size. As such I wish there was a way to not pull in this rather heavy dependency while still having text wrapping available.

### Describe the solution you'd like

* Make `terminal_size` something you need to opt in.
* Alternatively add a `wrap_help_core` feature and have `wrap_help` depend on it and `terminal_size`

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @mitsuhiko on 2022-12-27 22:13_

---

_Comment by @epage on 2022-12-27 22:20_

Could you clarify what is the problem with depending on `terminal_size`? You mention its "heavy" but what metric is being used and how heavy is it? 

---

_Comment by @mitsuhiko on 2022-12-27 22:25_

`terminal_size` in the version used by `clap` depends on the following crates:

* `rustix`
* `bitflags` (duplicate, can't eliminate)
* `errno`
* `libc`
* `io-lifetimes`

This is what this looks like compile time wise for clap without default features but with `wrap_help` and `std` only:

<img width="297" alt="image" src="https://user-images.githubusercontent.com/7396/209727776-dc7f494f-dee5-4788-8a4b-e6f1de4aa382.png">

Out of the 2.9 seconds total compile time, more than 1 second is spent on just `terminal_size` and it's dependencies.

---

_Comment by @epage on 2022-12-27 22:31_

Thanks for further quantifying.

Of note, if the `color` feature is enabled, most or all of these transitive dependencies become shared.  Most of that is for `is-terminal` which we hope will be upstreamed into Rust an will then go away.  Unsure what that means for these dependencies.

> I have my own way to read the terminal size

Could you expand on what you do for this?

---

_Comment by @mitsuhiko on 2022-12-27 22:40_

I'm generally always disabling the color feature. For already reading the terminal size I usually build apps with my `console` library which already provides a way to read the terminal size. This however also depends on `libc` so once you use it you don't win that much, but still a bit.

The current case of mine however is just that I want to configure a fixed size for the help output without any sort of terminal width adjustments but still take advantage of text rewrapping.

---

_Comment by @moschroe on 2024-01-26 23:53_

to throw in my niche 2¢: I recently ported a command line binary ([dev tool I wrote out of necessity](https://moschroe.github.io/markup-convert-r/)) to wasm as an experiment, wanted to have help wrapped and there obviously is no way to do any of the low-level `sys` stuff, not to mention to compile the code.

Without restricting the help width, though, the lines are too long and wrap

---

_Label `A-help` added by @epage on 2024-01-29 16:08_

---

_Comment by @hanna-kruppe on 2025-06-28 12:53_

> The current case of mine however is just that I want to configure a fixed size for the help output without any sort of terminal width adjustments but still take advantage of text rewrapping.

This is exactly my use case as well.


> Of note, if the `color` feature is enabled, most or all of these transitive dependencies become shared. Most of that is for `is-terminal` which we hope will be upstreamed into Rust an will then go away. Unsure what that means for these dependencies.

In 2025, `is-terminal` is gone and most of those dependencies aren't shared. Clap's color feature enables anstyle/anstream , which may sometimes share windows-sys with terminal_size when they agree on a semver-compatible version (currently they don't: 0.60 vs. 0.59). rustix has dropped io-lifetimes but still depends on bitflags, errno, libc, and (new since 2022) linux-raw-sys. None of those are shared under clap's default features, nor with most opt-in features. The only other sharing I could find is `clap/debug -> backtrace -> libc`, but that's not very interesting because the feature is rarely enabled and libc is by far the most likely to be shared with another dependency unrelated to clap.

---
