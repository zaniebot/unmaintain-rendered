---
number: 3922
title: Support delimited values for native completions
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-builder
  - A-completion
  - E-help-wanted
  - ":money_with_wings: $20"
assignees: []
created_at: 2022-07-13T14:19:57Z
updated_at: 2024-08-19T14:56:43Z
url: https://github.com/clap-rs/clap/issues/3922
synced_at: 2026-01-10T01:27:49Z
---

# Support delimited values for native completions

---

_Issue opened by @epage on 2022-07-13 14:19_

See #3166 for more context

- [code](https://github.com/clap-rs/clap/blob/master/clap_complete/src/dynamic.rs)
- [tests](https://github.com/clap-rs/clap/blob/master/clap_complete/tests/dynamic.rs)

---

_Label `C-enhancement` added by @epage on 2022-07-13 14:19_

---

_Label `A-builder` added by @epage on 2022-07-13 14:19_

---

_Label `A-completion` added by @epage on 2022-07-13 14:19_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2022-07-13 14:20_

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:36_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:29_

---

_Referenced in [clap-rs/clap#5587](../../clap-rs/clap/issues/5587.md) on 2024-07-22 18:02_

---

_Referenced in [clap-rs/clap#5602](../../clap-rs/clap/pulls/5602.md) on 2024-07-26 13:27_

---

_Comment by @epage on 2024-08-16 21:15_

@shannmu is there a reason #5602 didn't close this?

---

_Comment by @shannmu on 2024-08-19 06:46_

The issue still seems to be related to `allow_hyphen_values`, where the completion for the delimited option includes the possible values of positional arguments (if they are also delimited). This is not caused by this issue, but it shows that the issue has not been fully resolved.

---

_Comment by @epage on 2024-08-19 13:30_

I don't what `allow_hyphen_values` has to do with `value_delimiter`. I don't quite understand your explanation.  Mind re-phrasing or providing examples?

---

_Comment by @shannmu on 2024-08-19 14:32_

https://github.com/clap-rs/clap/blob/d81158599f5b3a2434845a1731377eae84780b9a/clap_complete/tests/testsuite/dynamic.rs#L673-L683

`--delimiter=comma` is treated as a delimited positional argument. We complete positional argument without conditions required when the current state is `ValueDone`, so it will call `complete_arg_value`.
https://github.com/clap-rs/clap/blob/d81158599f5b3a2434845a1731377eae84780b9a/clap_complete/src/dynamic/complete.rs#L216-L221

---

_Comment by @epage on 2024-08-19 14:56_

@shannmu I had missed that.  If there is a known issue in a PR, call it out.   That should have been discussed before merging.

---
