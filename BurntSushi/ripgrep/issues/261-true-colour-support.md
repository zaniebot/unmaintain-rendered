```yaml
number: 261
title: true colour support
type: issue
state: closed
author: daxim
labels:
  - help wanted
  - icebox
assignees: []
created_at: 2016-12-01T11:02:56Z
updated_at: 2018-01-31T02:39:52Z
url: https://github.com/BurntSushi/ripgrep/issues/261
synced_at: 2026-01-12T16:13:21Z
```

# true colour support

---

_@daxim_

Let us pick [any RGB colour](https://gist.github.com/XVilka/8346728) instead of just eight.

---

_Comment by @BurntSushi on 2016-12-01 11:59_

I think what we have now is sufficient for the vast majority of use cases. I expended a lot of energy of getting what we have now, and I'm not likely to revisit it myself for quite some time (if ever). If someone else wants to work on this, then I'd be fine with that. But, I would expect to see a specification here before submitting a PR, including the behavior in environments where true color isn't supported and how it will be exposed in the [`termcolor`](https://docs.rs/termcolor/0.1.1/termcolor/) library.

---

_Label `help wanted` added by @BurntSushi on 2016-12-01 11:59_

---

_Label `icebox` added by @BurntSushi on 2016-12-01 11:59_

---

_Comment by @daxim on 2016-12-02 03:54_

The `termcolor` type `Color` is extended to accept RGB triples, e.g. `255;100;0`. The same format can be used in user-facing applications like rg in addition to the traditional colour names.

From the library, emit e.g. `␛[38;2;255;100;0m` for foreground and `␛[48;2;255;100;0m` for background.

Terminals that do not interpret the code ignore it.

---

_Comment by @scottchiefbaker on 2017-01-12 21:45_

I agree that we need access to more colors. The base eight ANSI colors are pretty limiting.

I like the way `ag` handles this where you specify your colors in raw ANSI codes. This allows the user to set both the color and the boldness of the text (and anything else ANSI supports). For example my `ag` alias is this:

```
alias ag="ag --color-match '1;38;5;208'"
```

This sets the matching text to: bold, orange (256 color pallet). 

This would require no more logic from `rg`, as it would just emit whatever is in the color match BEFORE the actual output.

---

_Comment by @BurntSushi on 2017-01-12 22:43_

I'm unlikely to work on this any time soon. I think this is strictly "nice to have."

(For the most part, I'm just in general sick of dealing with terminal/color issues.)

---

_Comment by @BurntSushi on 2017-01-12 22:44_

If someone else wanted to take a crack at this, I'd be happy to mentor it.

---

_Comment by @jdhorwitz on 2017-03-15 16:42_

@BurntSushi I could take a crack at this if you are still willing to mentor it.  Thanks!

---

_Comment by @BurntSushi on 2017-03-15 16:46_

@jdhorwitz Cool. I think I'd start with this:

1. Write up a specification for how this works. If you're not sure where to start, here's my suggestion: write the documentation you would expect to see, *as an end user*, once this feature is implemented.
2. Sketch a rough implementation plan.

No need to spend too much time on this, but I think it's useful to think about this a little before writing code so that we can poke at it a bit!

---

_Comment by @BurntSushi on 2017-03-15 16:46_

(Putting the write up in this issue is cool.)

---

_Comment by @BurntSushi on 2017-03-15 16:46_

And of course, if you need help with the writeup, then ask questions!

---

_Comment by @scottchiefbaker on 2017-03-15 18:44_

I don't know anything about rust, but I'd be interested in providing feedback on how the feature should look. If you're soliciting feedback, this would be a good place to start a discussion.


---

_Comment by @tiehuis on 2017-04-14 01:11_

### Summary
 - Provide 24-bit rgb and 256 color modes as Ansi color extensions.
 - Fallback to white for the moment on Windows if an rgb/256 color is specified
 - Up to the user to ensure that their terminal supports whatever color codes they are providing it

#### Questions
 - Provide extended attributes? As @scottchiefbaker mentions. Only issue I see with this is it exposes the underlying implementation for all to see. This ties us explicitly to ANSI codes. Specifying a list of attributes in human readable form may be better. For example `--colors 'match:fg:bold+underline,red'`.
Wouldn't want to overdo here and make the format too arcane. For now just deal with extended colors.

If this all sounds good I can clean up the implementation a bit and we can go from there.

----

I've had a quick look at this and here are my thoughts. I've pushed a sample implementation (see above) which accepts 256-color codes and full rgb color codes too. 

Example invocations right now are as follows.

```
# 256-color mode
rg --colors 'match:fg:25'
# truecolor mode
rg --colors 'match:fg:128,0,3'
```

Format is just the first thing that came to mind, nothing special or set in stone. For the moment, we just fall back to white on windows if we encounter an rgb or 256-color code that we cannot parse. This is likely sufficient behavior for the moment.

## Terminal Support?

The main problems I had with this are that different terminal emulators are fickle things and don't parse the 24-bit color codes in a nice way. I've done some tests myself on 4 different terminals to check the difference.

I've tested the following terminals:
 - st 0.7
 - urxvt 9.22
 - XTerm(327)
 - Sakura 3.3.4

These are all the latest versions currently on Arch.

## Truecolor Support

![Truecolor Support (st+urxvt)](https://i.imgur.com/FCoskIX.png)
![Truecolor Support (xterm+sakura)](https://i.imgur.com/wMxUBCA.png)

All except for urxvt display this in a reasonable manner. urxvt supposedly approximates but it looks this change hasn't yet made it into the stable version available in package managers.

### Buggy urxvt Parsing

The main problem however is not that urxvt does not display these colors. It is that it will parse rgb color codes incorrectly, treating each rgb component instead as an attribute it does know about.

For example, the following command `--colors 'match:fg:31,3,5` should display a very dark red. However urxvt will instead parse this as red (31), underline (3), then blink (5) attributes. This provides some interesting results.

![urxvt-buggy](https://i.imgur.com/JbZI6rT.gif)

## 256-color Support

![256-color Support (st+urxvt)](https://i.imgur.com/e44yJRq.png)
![256-color Support (xterm+sakura)](https://i.imgur.com/qGfX6z8.png)

All support 256-color mode reasonably.

## Should we still want these colors?

Even though this can produce incorrect colors, I still think it is worthwhile adding with the existing functionality (or similar). These colors could be considered an extension and the assurance that the terminal being printed to supports them should be placed on the user instead. Only potential issue I see is passing an rgb color code and not seeing the expected color (windows or bad terminal) could be considered a defect by an end-user. We would need to clearly indicate this next to the format specification.


---

_Comment by @scottchiefbaker on 2017-04-14 21:16_

> Xterm,[14] KDE's Konsole,[18] as well as all libvte based terminals[19] (including GNOME Terminal) support ISO-8613-3 24-bit foreground and background color setting[better source needed] Quoting one of the text-files in its source-tree:[20]

This is from [Wikipedia](https://en.wikipedia.org/wiki/ANSI_escape_code). It's 2017, I think true color is pretty well supported in most modern terminals. It's an accepted ANSI standard now, I don't think we need to worry about XYZ terminal not supporting it. The popular main terminals already support it.

---

_Comment by @scottchiefbaker on 2017-04-14 21:21_

For reference here is a good discussion on terminal support for true color: 

https://gist.github.com/XVilka/8346728

---

_Comment by @scottchiefbaker on 2017-04-14 21:22_

Not to totally spam this bug but... I really like your implementation details:

```
# 256-color mode
rg --colors 'match:fg:25'
# truecolor mode
rg --colors 'match:fg:128,0,3'
```

I think it's intuitive, simple, and backwards compatible with what's already in place. This gets a 👍 from me. I'll test this build and report back.

---

_Comment by @scottchiefbaker on 2017-04-17 20:43_

I was able to build your branch and everything works exactly as expected. It should completely address this issue. You'd get my +1 to merge if you submitted a PR to @BurntSushi 

---

_Comment by @parkovski on 2017-11-11 17:30_

To test and enable this on Windows if it's supported, you'd use the equivalent of this C code:
```c
// There are a couple ways to get this handle, just use the one you have.
HANDLE stdout = GetStdHandle(STD_OUTPUT_HANDLE);
DWORD out_mode;
if (!GetConsoleMode(stdin, &out_mode)) {
    // Couldn't get the current mode, assume it's not supported.
}
if (!SetConsoleMode(stdin, out_mode | ENABLE_VIRTUAL_TERMINAL_PROCESSING /* 0x4 */)) {
    // The mode couldn't be set or wasn't recognized, use the console API coloring fallback
}
// If both those calls succeeded, the console will process
// ANSI escaped output and support true color.
```

The console mode functions in Rust would be declared as (`BOOL` and `DWORD` are always 32 bits):
```rust
extern "system" {
    fn GetConsoleMode(handle: std::os::windows::io::RawHandle, mode: *mut u32) -> i32;
    fn SetConsoleMode(handle: std::os::windows::io::RawHandle, mode: u32) -> i32;
}
```

---

_Comment by @BurntSushi on 2018-01-31 02:39_

This was done by @tiehuis in #766.

---

_Closed by @BurntSushi on 2018-01-31 02:39_

---
