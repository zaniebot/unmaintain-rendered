```yaml
number: 442
title: "rg --help | less assumes the terminal has 100 columns"
type: issue
state: closed
author: bluss
labels:
  - question
assignees: []
created_at: 2017-04-09T14:42:21Z
updated_at: 2021-12-08T12:06:55Z
url: https://github.com/BurntSushi/ripgrep/issues/442
synced_at: 2026-01-12T16:13:22Z
```

# rg --help | less assumes the terminal has 100 columns

---

_@bluss_

If we pipe rg's output to something, it assumes the terminal has 100 columns.

For example rg --help, which has output that is long and begs for pagination.

To reproduce: Install rg. Adjust terminal to a width of for example 90 columns. Use `rg --help | less`  (with no shell aliases or anything in between).

(Sorry! rg --help is now fixed, so the bug report now had its schedule unblocked)

---

_Comment by @BurntSushi on 2017-04-09 15:11_

ripgrep doesn't do anything explicitly smart with terminal size, so I assume this is a clap thing.

---

_Label `question` added by @BurntSushi on 2017-04-09 15:11_

---

_Comment by @BurntSushi on 2017-04-09 15:13_

I could have sworn clap had some setting or knob for this, but I can't find it after a quick skim.

---

_Comment by @kbknapp on 2017-04-09 16:00_

There's [`App::max_term_width`](https://docs.rs/clap/2.23.1/clap/struct.App.html#method.max_term_width) and [`App::set_term_width`](https://docs.rs/clap/2.23.1/clap/struct.App.html#method.set_term_width)

`max_term_width` will allow ~~freeform~~ "smart" wrapping at whatever the user terminal width is, *up to* some maximum. This is what ripgrep uses currently (at 100).

`set_term_width` sets the virtual width and always "smart" wraps at that column width.

---

_Comment by @kbknapp on 2017-04-09 16:01_

[I hit comment too early]

Not using either will just "smart" wrap at whatever width the user terminal width happens to be.

---

_Comment by @BurntSushi on 2017-04-09 16:09_

@kbknapp Oh interesting. Welp. I was clearly wrong about ripgrep not doing anything.

With that said, I can't seem to cause clap to do the right thing here. Namely, if `max_term_width` is set to `100`, then shouldn't it wrap text at `N < 100` after I shrink my terminal? (I also just tried `set_term_width`, which has the same behavior.)

If I remove the `max_term_width`/`set_term_width` setting completely, then the help text sprawls across my entire screen, which I suspect is why I set it in the first place.

---

_Comment by @BurntSushi on 2017-04-09 16:10_

I see. clap does do the right thing *unless* its output is piped to `less`. So presumably that interferes with `clap` figuring out the terminal width?

---

_Comment by @kbknapp on 2017-04-09 16:14_

Interesting, my initial thinking is `less` is being detected a terminal without a width (or rather `stdout` not being a terminal) which is causing the fallback to 100.

---

_Comment by @BurntSushi on 2017-04-09 16:14_

I haven't looked at clap's source code, but my best guess is that clap only does autowrapping when it thinks its printing to a tty (which seems reasonable). So if its output is piped to a file or to another program, then it won't auto-wrap because it isn't a tty. It should be possible to discriminate between "redirected to a file" and "piped to other process," but even then, it's not clear that respecting terminal size is the right choice (although it certainly seems like a pragmatic one).

Another choice is to just set the max width to `79` columns and hope that fits most use cases.

---

_Comment by @BurntSushi on 2017-04-09 16:15_

@kbknapp Ah right, beat me to it!

---

_Comment by @kbknapp on 2017-04-09 16:17_

Here's where the logic happens

https://github.com/kbknapp/term_size-rs/blob/0297b6af8a6072ef1bee30527b6cb39f55b44755/src/lib.rs#L58-L82

---

_Comment by @BurntSushi on 2017-04-09 16:19_

@kbknapp Oh, I suppose that is actually a different issue than what I was saying above. That's suggesting that `ioctl` itself will fail to find the terminal size if the process is being piped to another process? Or rather, finding the dimensions works by examining the stdout file descriptor? Interesting.

---

_Comment by @kbknapp on 2017-04-09 16:52_

@BurntSushi `ioctl` will fail when `stdout` isn't a tty. The crux of the problem is that the dimensions are determined by `stdout`, and where `stdout` is piped or redirected there are no dimensions. I don't know of a way to detect what the terminal width *was* (i.e. before `stdout` is checked).


---

_Comment by @kbknapp on 2017-04-09 17:46_

@BurntSushi I found a solution that seems to work! 🎉 

`term_size` can read stdout, and if that fails try *stdin*...if *that* fails then try stderr...*then* give up. Works in my tests so far.

---

_Comment by @BurntSushi on 2017-04-09 17:59_

Nice hack. I love it.

On Apr 9, 2017 1:46 PM, "Kevin K." <notifications@github.com> wrote:

> @BurntSushi <https://github.com/BurntSushi> I found a solution that seems
> to work! 🎉
>
> term_size can read stdout, and if that fails try *stdin*...if *that*
> fails then try stderr...*then* give up. Works in my tests so far.
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/442#issuecomment-292800709>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oa1sa0OdFtgHVn28d9J-BOP-HB3ks5ruRl9gaJpZM4M4C3P>
> .
>


---

_Comment by @kbknapp on 2017-04-10 01:02_

Just did a `cargo install ripgrep` (to update `term_size`) and this is working.

---

_Closed by @BurntSushi on 2017-05-08 23:41_

---

_Comment by @eproxus on 2020-06-22 12:57_

I have this issue running `rg -h` and `rg --help` using 12.1.1 running on macOS. Was this really fixed?

---

_Comment by @mattiase on 2021-12-08 12:06_

> I have this issue running `rg -h` and `rg --help` using 12.1.1 running on macOS. Was this really fixed?

No, you are right – it seems that `clap` isn't used with the `wrap_help` feature. Presumably it should be added to the list of features in `Cargo.toml`.

With that done, it works even when stdout is a pipe, as in `rg -h | less` since it seems to look at the tty config for stdin (which is standard practice for interactive tools). Nice! 

However, the  `--help` output is less pleasing: `clap` doesn't reflow paragraphs but wraps each line separately instead and this leads to the infamous "comb" appearance of alternating long and short lines. To deal with this, we either need to remove newlines inside paragraphs (replacing each with a single space), or detect blank lines as paragraph delimiters. The latter means less boring legwork but some changes to `clap` it seems.

Oh, and it would be nice with a right margin of a few spaces (2, say) for readability. I suppose that would go into `clap` too?


---
