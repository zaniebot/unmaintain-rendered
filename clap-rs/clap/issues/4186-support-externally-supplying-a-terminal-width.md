---
number: 4186
title: Support externally supplying a terminal width when running outside a tty
type: issue
state: closed
author: Nemo157
labels:
  - C-enhancement
assignees: []
created_at: 2022-09-06T21:31:47Z
updated_at: 2022-09-13T02:18:00Z
url: https://github.com/clap-rs/clap/issues/4186
synced_at: 2026-01-10T01:27:51Z
---

# Support externally supplying a terminal width when running outside a tty

---

_Issue opened by @Nemo157 on 2022-09-06 21:31_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

v3.2.20

### Describe your use case

I am attempting to verify my readme contains the latest version of the help text from my utility. To do so I need an appropriately widthed help text to compare against (and auto-update from).

### Describe the solution you'd like

I'm not sure what the best interface for this would be. One thought I had was to obey the `COLUMNS` variable if that is set and the output is not a tty, that would make `text="$(COLUMNS=60 cargo run -- --help)"` work fine.

### Alternatives, if applicable

Locally I found a way to get a sub-tty with a limited width

```sh
script -q -c 'sh -c "stty cols 60 && cargo run -q -- --help"' $(mktemp)
```

but unfortunately that doesn't work in GitHub Actions for some unknown reason.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Nemo157 on 2022-09-06 21:31_

---

_Comment by @epage on 2022-09-06 21:43_

Already [terminal_size](https://docs.rs/terminal_size/latest/terminal_size/fn.terminal_size.html) doesn't do detection if STDOUT is not a tty:

> Another option is to not use If STDOUT is not a tty, returns `None`

Is that sufficient or something going wrong with it?  Or are you wanting a different fallback terminal width?

Also, as a workaround, you can implement this all yourself by calling `terminal_size` directly and setting `term_width` under the specific circumstances for your app.

---

_Comment by @Nemo157 on 2022-09-06 22:01_

> Or are you wanting a different fallback terminal width?

Yes, this; I want to override the fallback terminal width in my script to have a consistent width no matter what terminal I'm running under.

> Also, as a workaround, you can implement this all yourself

Yep, it doesn't seem _too_ hard (just misses out on a lot of the `Parser` nicities to be able to override the derive). I just feel like it's pretty common to include `--help` output into a readme, where the default 100 column fallback is far too wide, so surely there must be other people running into this.

---

_Comment by @epage on 2022-09-12 12:33_

> Yes, this; I want to override the fallback terminal width in my script to have a consistent width no matter what terminal I'm running under.

In my initial research on this, it sounded like `COLUMNS` isn't actually env-variable but is instead something built directly into some shells that some users forward on to the environment and that application support is very half-hazard.  This made me less interested in supporting it.

Then I saw it suggested in https://clig.dev and saw it used in git which makes me feel more comfortable in supporting it.

The question is what should the level of precedence be?

1. COLUMNS
2. `terminal_size``
3. default

or

1. `terminal_size``
2. COLUMNS
3. default

---

_Comment by @epage on 2022-09-12 12:33_

Also, I feel safer putting this into clap v4 rather than clap v3 (if we make the release window)

---

_Referenced in [clap-rs/clap#4208](../../clap-rs/clap/pulls/4208.md) on 2022-09-13 01:17_

---

_Closed by @epage on 2022-09-13 02:18_

---
