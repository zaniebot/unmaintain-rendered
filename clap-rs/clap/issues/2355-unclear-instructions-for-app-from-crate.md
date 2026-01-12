```yaml
number: 2355
title: "Unclear instructions for `app_from_crate!`"
type: issue
state: closed
author: xaocon
labels:
  - A-docs
assignees: []
created_at: 2021-02-20T20:15:30Z
updated_at: 2021-02-20T21:15:13Z
url: https://github.com/clap-rs/clap/issues/2355
synced_at: 2026-01-12T16:14:13Z
```

# Unclear instructions for `app_from_crate!`

---

_@xaocon_

### Clap version

2.33.3 - though I checked masters docstrings and I don't see any improvement.

### Where

https://github.com/clap-rs/clap/blob/3907e116210884f9259ae94e78dc031e9511ca6f/src/macros.rs#L169
https://docs.rs/clap/2.33.3/clap/macro.app_from_crate.html

### What's wrong

It's not clear how to develop this into a fully formed app. I was hoping to use it like `clap_app!` but with sections removed that had already been filled but that didn't work. I tried chaining them a couple of different ways. I tried using the result from `app_from_crate!` as the first argument to `clap_app!` but none of that works. Macros in Macros don't seem to work anywhere as I originally tried using the `crate_*!` macros to fill in `clap_app!`. I'm not sure there is a good way to set up options if you use `app_from_crate!` but it doesn't seem like it's a very helpful macro if not.

### How to fix

I'm not really sure. If someone could explain it to me I wouldn't mind putting in a PR for it but I don't have enough understanding of macros to make sense of it currently.


---

_Label `C: docs` added by @xaocon on 2021-02-20 20:15_

---

_Comment by @pksunkara on 2021-02-20 20:31_

Here's an example, https://github.com/clap-rs/clap/blob/6ce1e4fda211a7373ce7c4167964e6ca073bdfc1/tests/app_from_crate.rs#L16

And the macro code is pretty self-explanatory. https://github.com/clap-rs/clap/blob/3907e116210884f9259ae94e78dc031e9511ca6f/src/macros.rs#L194

---

_Comment by @xaocon on 2021-02-20 21:01_

The `app_from_crate!` code is pretty self-explanatory but I had hoped to use `app_from_crate!` with the argument building style from `clap_app!` and I can't make sense of that one. I guess you're saying that there's not a good way to build out arguments for the app using macros if you use `app_from_crate!`? It turns out I had had a mistake in my syntax that was keeping me from using the `crate_*!` macros in `clap_app!` so it's not really much of a big deal to just use `clap_app!`.

If there is some reasonable way to use the macro arguments from `clap_app!` with `app_from_crate!` then I think it would be helpful mention it in the documents as I believe it wasn't completely unreasonable for me to think that was possible. If there is, I think it would be helpful to show how in the example. In either case, I'd be happy to open a PR for it. I also noticed a small typo in the `crate_license!` macro docs that I could correct at the same time.

---

_Comment by @pksunkara on 2021-02-20 21:12_

Can you paste your code?

---

_Comment by @pksunkara on 2021-02-20 21:15_

And no, `app_from_crate!` cannot be  used with `clap_app`

---

_Closed by @pksunkara on 2021-02-20 21:15_

---

_Locked by @clap-rs on 2021-02-20 21:15_

---
