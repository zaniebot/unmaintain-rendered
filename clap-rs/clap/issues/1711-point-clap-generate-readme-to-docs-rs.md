---
number: 1711
title: Point clap_generate README to docs.rs
type: issue
state: closed
author: pickfire
labels:
  - A-docs
assignees: []
created_at: 2020-03-01T11:44:49Z
updated_at: 2021-08-18T18:51:35Z
url: https://github.com/clap-rs/clap/issues/1711
synced_at: 2026-01-10T01:27:03Z
---

# Point clap_generate README to docs.rs

---

_Issue opened by @pickfire on 2020-03-01 11:44_

### Clap version

2.x

### Where?

/clap_generate/examples/

### What's wrong

Missing example easily get started with man page and completions generations.

### How to fix

Add example to have a sample build.rs that generated all the man pages and completions in examples or in README.


---

_Label `C: docs` added by @pickfire on 2020-03-01 11:44_

---

_Comment by @pksunkara on 2020-03-01 11:47_

We would need to create an example crate for that instead of just example. Look at the docs here, https://github.com/clap-rs/clap/blob/master/clap_generate/src/lib.rs#L86

---

_Comment by @pickfire on 2020-03-01 13:09_

Oh, I did not noticed that. But why is the README empty? https://github.com/clap-rs/clap/blob/master/clap_generate/README.md Maybe it should say something?

---

_Comment by @pksunkara on 2020-03-01 13:10_

Yup, the readme needs to be updated. I had only recently refactored the crate.

---

_Comment by @pickfire on 2020-03-01 13:29_

So I guess this could be closed.

---

_Closed by @pickfire on 2020-03-01 13:29_

---

_Reopened by @pksunkara on 2020-03-01 13:30_

---

_Added to milestone `3.0` by @pksunkara on 2020-03-01 13:31_

---

_Renamed from "Add example to show man page / completion generation in build.rs" to "Point clap_generate README to docs.rs" by @pksunkara on 2020-03-01 13:31_

---

_Referenced in [clap-rs/clap#2720](../../clap-rs/clap/pulls/2720.md) on 2021-08-18 16:15_

---

_Closed by @pksunkara on 2021-08-18 18:51_

---
