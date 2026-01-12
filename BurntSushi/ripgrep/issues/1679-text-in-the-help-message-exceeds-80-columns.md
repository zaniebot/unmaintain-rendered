```yaml
number: 1679
title: "Text in the `--help` message exceeds 80 columns"
type: issue
state: closed
author: Lokathor
labels:
  - bug
  - wontfix
assignees: []
created_at: 2020-09-10T21:26:07Z
updated_at: 2021-05-29T13:30:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1679
synced_at: 2026-01-12T16:13:24Z
```

# Text in the `--help` message exceeds 80 columns

---

_@Lokathor_

When using the `--help` command, the output regularly exceeds 80 columns in width, causing it to awkwardly hard wrap in a standard terminal.

Please limit the help output width to 80 columns. Alternately, you could dynamically check the terminal width and limit output to that width (though that might be a little more complex).

---

_Comment by @BurntSushi on 2020-09-11 13:12_

This is supposed to work. It looks like a bug in clap to me. ripgrep sets [`max_term_width` here](https://github.com/BurntSushi/ripgrep/blob/11c7b2ae17c0c2547eaffc92ac80ff03bd9950d7/crates/core/app.rs#L60), which is [documented to do as you describe](https://docs.rs/clap/2.33.3/clap/struct.App.html#method.max_term_width), AIUI.

cc @kbknapp 

---

_Label `bug` added by @BurntSushi on 2020-09-11 13:13_

---

_Comment by @jacobherrington on 2020-09-22 21:22_

I tested this locally (Fedora 32) and seems that max_term_width is behaving as expected.

If I understood correctly @Lokathor would prefer the output to wrap at 80 columns and it is set to 100 columns. I opened #1686 to change that setting. :+1: 

---

_Comment by @BurntSushi on 2020-09-22 21:26_

No, the output should wrap correctly based on terminal width, even if it's less than 100 columns. As I understand it, clap should be inspecting the terminal size.

This might actually be ripgrep's fault, since I think it is responsible for printing the help message for reasons I forget right now. I'm on mobile.

---

_Comment by @cbiffle on 2021-02-18 21:17_

Inspecting the terminal size is gated behind the `wrap_help` feature in Clap, which ripgrep is not requesting.

One caution: Clap relies on the `term_size` crate to do this, which has limited platform support. Consider enabling the feature only on certain platforms. (Right now ripgrep works on, for example, Illumos, while `term_size` does not.)

---

_Comment by @BurntSushi on 2021-05-29 13:30_

I am unfortunately going to mark this as `wontfix` for now.

* Changing the default wrapping size to 79 columns makes the output of `rg -h` non-ideal. The help text was written with 100 columns in mind so that each option gets placed on its own line.
* Enabling the `wrap_help` feature of clap brings in the [`term_size`](https://github.com/clap-rs/term_size-rs) crate, which is unmaintained and thus a hazard.
* As mentioned above, apparently `term_size` "doesn't work" on Illumos. (It's not clear what that means. Doesn't compile? Had poor runtime behavior?) Illumos is not a supported ripgrep platform (how do I run it on CI?), and if there is a problem, it sounds like a problem with clap and/or term_size. So this alone isn't enough for me to not enable `wrap_help`, but contributes to it.
* I did actually try enabling `wrap_help` and the output doesn't look much better. Apparently, whatever is reflowing text doesn't do a good job of it. It looks like it can't handle the fact that the help text is currently written with line breaks at 79 columns, so the reflowing gets confused.

I don't see this getting solved on ripgrep's end unless we roll our own arg parser. I think the clap folks need to dig into this.

---

_Closed by @BurntSushi on 2021-05-29 13:30_

---

_Label `wontfix` added by @BurntSushi on 2021-05-29 13:30_

---
