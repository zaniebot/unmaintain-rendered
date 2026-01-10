---
number: 2869
title: 3.0 Release Checklist
type: issue
state: closed
author: epage
labels:
  - A-meta
  - C-tracking-issue
  - S-blocked
assignees: []
created_at: 2021-10-13T11:24:03Z
updated_at: 2022-01-01T19:41:01Z
url: https://github.com/clap-rs/clap/issues/2869
synced_at: 2026-01-10T01:27:28Z
---

# 3.0 Release Checklist

---

_Issue opened by @epage on 2021-10-13 11:24_

- [ ] 1 month of release candidate feedback
- [x] Double check all docs
  - Match 3.0?
  - Generally, use more specific derive traits than Parser?
  - Good jumping off point for most users
  - Examples use `global_settings` where possible to raise awareness
- [x] Double check all examples
- [x] Double Check readme
- [x] Double check examples on clap.rs
  - Compile?
- [x] Changelog
- [x] Clippy Run (CI)
- [x] Rustfmt Run (CI)
- [x] Spell Check Docs
- [ ] Blog Post

---

_Label `C: meta` added by @epage on 2021-10-13 11:24_

---

_Label `M: blocked` added by @epage on 2021-10-13 11:24_

---

_Added to milestone `3.0` by @epage on 2021-10-13 11:24_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2021-10-13 11:24_

---

_Comment by @wldevries on 2021-10-17 09:48_

Not sure if this is the right place, but the readme in this repository for "Using Derive Macros" is incorrect at the moment. It uses `Parser` instead of the replacement type `Clap`.

---

_Comment by @tranzystorekk on 2021-10-17 10:05_

@wldevries According to d840d5650e9a7085a39c620697b11d6010d4fcf7 the `Clap` trait has been renamed to `Parser` to show what it does more specifically.

---

_Referenced in [clap-rs/clap#2909](../../clap-rs/clap/pulls/2909.md) on 2021-10-19 01:42_

---

_Label `W: blocked` added by @pksunkara on 2021-10-27 09:59_

---

_Label `M: blocked` removed by @pksunkara on 2021-10-27 09:59_

---

_Referenced in [diesel-rs/diesel#2932](../../diesel-rs/diesel/pulls/2932.md) on 2021-10-27 17:18_

---

_Referenced in [heroku/libcnb.rs#199](../../heroku/libcnb.rs/pulls/199.md) on 2021-11-30 23:29_

---

_Referenced in [epage/clapng#220](../../epage/clapng/issues/220.md) on 2021-12-06 22:18_

---

_Referenced in [PyO3/maturin#738](../../PyO3/maturin/pulls/738.md) on 2021-12-10 06:33_

---

_Referenced in [atuinsh/atuin#233](../../atuinsh/atuin/pulls/233.md) on 2021-12-10 17:23_

---

_Referenced in [cucumber-rs/cucumber#188](../../cucumber-rs/cucumber/pulls/188.md) on 2021-12-13 15:07_

---

_Label `C-tracking-issue` added by @epage on 2021-12-13 22:41_

---

_Closed by @epage on 2022-01-01 18:51_

---

_Comment by @sylvestre on 2022-01-01 19:41_

Bravo @epage thanks for the hard work!


---
