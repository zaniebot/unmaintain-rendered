```yaml
number: 512
title: Should not send color to a pipe by default
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-05-23T14:53:16Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/512
synced_at: 2026-01-12T16:14:09Z
```

# Should not send color to a pipe by default

---

_@joshtriplett_

Clap prints color unconditionally, even when sending output somewhere other than a terminal.  Redirecting output to a file, or to a pager that doesn't understand terminal escape sequences like `less -R` does, produces undesirable terminal escape sequences in the file or pager.

Clap should detect when output goes somewhere other than a terminal, and suppress terminal escape sequences otherwise.


---

_Comment by @kbknapp on 2016-05-23 16:55_

Thanks for filing this. Yeah I agree, and this should be an easy detection on whether not something is a terminal. I'm undecided if this should be something `clap` handles, or simply makes easy to handle in consumer code. If `clap` handles it, it's done at compile time. Even though `App` instances are built at runtime, you'd run into a chicken/egg problem if a user tried passing some imaginary option for to change the functionality from a `--color=auto` to a `--color=never` or the like.

I think the best way to accomplish this would be a setting of some sort, perhaps default to `ColorAlways`, and then allow `ColorAuto` or `ColorNever` instead. This leaves it at compile time, but allows for some hacky workarounds if someone _does_ want to support some type of `--color=WHEN` option (such as env vars, or looping upon conditionally receiving the `--color` option.)

Thoughts? This should be an easy addition if that option works for you.


---

_Label `T: enhancement` added by @kbknapp on 2016-05-23 16:55_

---

_Label `C: help message` added by @kbknapp on 2016-05-23 16:55_

---

_Label `D: easy` added by @kbknapp on 2016-05-23 16:55_

---

_Label `P3: want to have` added by @kbknapp on 2016-05-23 16:55_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-23 16:55_

---

_Label `C: errors` added by @kbknapp on 2016-05-23 16:55_

---

_Label `C: settings` added by @kbknapp on 2016-05-23 16:55_

---

_Label `T: new setting` added by @kbknapp on 2016-05-23 16:55_

---

_Comment by @joshtriplett on 2016-05-23 18:27_

I don't think it makes sense to have a command-line option to change this or similar; all of clap's color (or output in general) happens when emitting usage, help, or errors, which happen as part of command-line option processing.  A program might want to have an auto/always/never configuration for color, and if it does (perhaps via config file read before the command line) then clap might want to provide a way to feed that in.  But I'm suggesting that clap should handle this by default, and default to "auto".

I think it would make sense to detect whether the target terminal (stdout/stderr) is a TTY, and only emit terminal escape sequences if so.

Rust has code to implement this; see https://github.com/rust-lang/rust/blob/master/src/libtest/lib.rs#L809 and https://github.com/rust-lang/rust/blob/master/src/libsyntax/errors/emitter.rs#L476 .  This likely needs to move into a library function somewhere; see https://github.com/rust-lang/rust/issues/33736 .  Once that library function exists, I'd suggest using it in clap to detect whether stderr goes to a terminal, and only producing escape sequences if so.

Entirely optionally, you could also provide a clap option (for instance, on App) to override this detection and always/never use color.


---

_Comment by @kbknapp on 2016-05-24 01:50_

Sounds good. Since this is for colored output which already isn't implemented on Windows systems I'm not too worried about using `libc` or not being able to detect this on Windows. `libc` is already a dep by default, so again, this isn't too huge a deal.

Should have this implemented shortly. :+1: 


---

_Comment by @kbknapp on 2016-05-30 07:07_

Just an update; I've been on vacation and should have this completed in a few days once I'm home.


---

_Label `D: easy` removed by @kbknapp on 2016-06-01 23:20_

---

_Label `D: intermediate` added by @kbknapp on 2016-06-01 23:20_

---

_Comment by @kbknapp on 2016-06-04 15:58_

#520 implements this


---

_Closed by @homu on 2016-06-08 01:34_

---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:06_

---
