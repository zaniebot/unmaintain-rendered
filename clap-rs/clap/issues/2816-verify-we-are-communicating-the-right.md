```yaml
number: 2816
title: Verify we are communicating the right recommendations for multiple_values vs multiple_occurrenes
type: issue
state: closed
author: epage
labels:
  - A-docs
assignees: []
created_at: 2021-10-05T15:25:41Z
updated_at: 2021-10-15T19:13:08Z
url: https://github.com/clap-rs/clap/issues/2816
synced_at: 2026-01-12T16:14:13Z
```

# Verify we are communicating the right recommendations for multiple_values vs multiple_occurrenes

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

v3.0.0-beta.4

### Where?

Everywhere

### What's wrong?

When we split `multiple` into `multiple_values` and `multiple_occurrences`, I suspect we might have used one in place of the other, especially before #2804 which limited where `multiple_occurrences` can be used.  In general `multiple_occurrences` is the safer option and generally what people were wanting from `multiple`

### How to fix?

Scrub the docs, updating them

---

_Label `C: docs` added by @epage on 2021-10-05 15:25_

---

_Added to milestone `3.0` by @epage on 2021-10-05 15:25_

---

_Referenced in [clap-rs/clap#2814](../../clap-rs/clap/pulls/2814.md) on 2021-10-05 15:25_

---

_Referenced in [clap-rs/clap#2886](../../clap-rs/clap/pulls/2886.md) on 2021-10-15 16:43_

---

_Closed by @bors[bot] on 2021-10-15 19:13_

---
