```yaml
number: 1467
title: Make it easier to use --multiline-dotall
type: issue
state: open
author: waldyrious
labels:
  - question
assignees: []
created_at: 2020-01-22T14:21:20Z
updated_at: 2020-04-04T11:15:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1467
synced_at: 2026-01-12T16:13:23Z
```

# Make it easier to use --multiline-dotall

---

_@waldyrious_

#### What version of ripgrep are you using?

```
❯ rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

MacOS Catalina (v10.15.2)

#### Describe your question, feature request, or bug.

I often need to search for patterns that span multiple lines, for which the combination `--multiline --multiline-dotall` is very helpful. There are two changes that would make this feature less cumbersome to use, though:

1. Passing `--multiline-dotall` could automatically assume `--multiline`;
2. A short option name could be added for `--multiline-dotall`, just like there's `-U` for `--multiline`.

These two together would make a command like

    rg -U --multiline-dotall 'foo.+?bar'

become, say:

    rg -µ 'foo.+?bar'

(of course, replacing `-µ` with the actual short option name for `--multiline-dotall`).

Alternatively, if only (2) is implemented, it would still be quite more convenient:

    rg -Uµ 'foo.+?bar'

I'd be happy to submit a PR if this is something that would be welcome.

---

_Comment by @BurntSushi on 2020-01-22 14:28_

Thanks for the thoughtful issue. I have a few thoughts.

I think making `--multiline-dotall` enable `--multiline` sounds plausible to me on the surface. However, the original intent was that folks could put `--multiline-dotall` in their config file (or alias), which would be inert without `--multiline` but otherwise enable the dotall behavior by default when `--multiline` is used. This is indeed a documented behavior, so I'm hesitant to change it. This may work for you if `--multiline-dotall` is indeed the most common thing. You could then use `--no-multiline-dotall` or `(?-s)` to disable it in the few cases where you don't want it.

Additionally, `--multiline-dotall` is not the only way to enable this behavior. For example, instead of

```
$ rg -U --multiline-dotall 'foo.+?bar'
```

you could write

```
$ rg -U '(?s)foo.+?bar'
```

Finally, the problem with using a short option is that we have very few choices left and I'm not quite sure this rises to the level of needing that. Especially when `(?s)` works and is fairly short on its own, although perhaps a bit more awkward to type than a short flag.

---

_Label `question` added by @BurntSushi on 2020-01-22 14:28_

---

_Comment by @waldyrious on 2020-01-22 14:51_

Thanks for the incredibly fast and detailed response!

Indeed I was reading through similar issues (especially the very informative #1150!) and realized that `(?s)` could be used as a somewhat more convenient alternative to spelling out `--multiline-dotall`. Not nearly as convenient as a single-letter option, but as you say, with the flag in the pattern itself and the config file method both being available, it may be hard to justify eating up yet another single-letter option.

Just to add a bit more concrete context: in order to type `rg -U '(?s)foo.+?bar'` I need 26 key-presses in my Portuguese keyboard layout, including modifiers like Shift:
- <kbd>r</kbd>, <kbd>g</kbd>, <kbd>␣</kbd>, <kbd>-</kbd>, <kbd>Shift</kbd>+<kbd>u</kbd>, <kbd>␣</kbd>, <kbd>'</kbd>, <kbd>Shift</kbd>+<kbd>8</kbd>, <kbd>Shift</kbd>+<kbd>'</kbd>, <kbd>s</kbd>, <kbd>Shift</kbd>+<kbd>9</kbd>, <kbd>f</kbd>, <kbd>o</kbd>, <kbd>o</kbd>, <kbd>.</kbd>, <kbd>+</kbd>, <kbd>Shift</kbd>+<kbd>'</kbd>, <kbd>b</kbd>, <kbd>a</kbd>, <kbd>r</kbd>, <kbd>'</kbd>

...while for `rg -µ 'foo.+?bar'` (assuming, again, that µ would become a plain ASCII letter), I would only need 18 (19 at worst, if it's uppercase):
- <kbd>r</kbd>, <kbd>g</kbd>, <kbd>␣</kbd>, <kbd>-</kbd>, <kbd>µ</kbd>, <kbd>␣</kbd>, <kbd>'</kbd>, <kbd>f</kbd>, <kbd>o</kbd>, <kbd>o</kbd>, <kbd>.</kbd>, <kbd>+</kbd>, <kbd>Shift</kbd>+<kbd>'</kbd>, <kbd>b</kbd>, <kbd>a</kbd>, <kbd>r</kbd>, <kbd>'</kbd>

That said, I would be OK with this issue being closed, but I'd suggest keeping this open for a while to gauge community interest. It may be that the trade-off is indeed worth if many people would support it.

ps - I will make sure to include some reference to the above options in [ripgrep's tldr page](https://github.com/tldr-pages/tldr/blob/master/pages/common/rg.md) to make this more discoverable.

---
