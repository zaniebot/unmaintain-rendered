---
number: 1963
title: "Remove `_clap_count_exprs` macro"
type: issue
state: closed
author: pksunkara
labels:
  - C-enhancement
  - E-easy
assignees: []
created_at: 2020-06-06T15:46:23Z
updated_at: 2020-06-10T07:23:21Z
url: https://github.com/clap-rs/clap/issues/1963
synced_at: 2026-01-07T13:12:19-06:00
---

# Remove `_clap_count_exprs` macro

---

_Issue opened by @pksunkara on 2020-06-06 15:46_

We don't need it to be public anymore.

---

_Label `T: enhancement` added by @pksunkara on 2020-06-06 15:46_

---

_Added to milestone `3.0` by @pksunkara on 2020-06-06 15:46_

---

_Label `D: easy` added by @pksunkara on 2020-06-08 08:48_

---

_Label `Z: good first issue` added by @pksunkara on 2020-06-08 08:48_

---

_Comment by @asteding on 2020-06-10 06:13_

Hello,

what do you mean with hiding?
Right now I don't see the macro being used in the current master, am I missing something here?


---

_Comment by @pksunkara on 2020-06-10 06:16_

You are right. I removed it's usage when I removed `arg_enum` macro. In that case, we can remove the macro completely.

---

_Renamed from "Hide `_clap_count_exprs` macro" to "Remove `_clap_count_exprs` macro" by @pksunkara on 2020-06-10 06:16_

---

_Referenced in [clap-rs/clap#1970](../../clap-rs/clap/pulls/1970.md) on 2020-06-10 06:32_

---

_Closed by @bors[bot] on 2020-06-10 07:23_

---
