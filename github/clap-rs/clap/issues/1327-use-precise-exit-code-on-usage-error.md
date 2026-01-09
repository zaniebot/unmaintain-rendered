---
number: 1327
title: Use precise exit code on usage error
type: issue
state: closed
author: killercup
labels: []
assignees: []
created_at: 2018-08-03T14:05:12Z
updated_at: 2020-01-30T19:30:58Z
url: https://github.com/clap-rs/clap/issues/1327
synced_at: 2026-01-07T13:12:19-06:00
---

# Use precise exit code on usage error

---

_Issue opened by @killercup on 2018-08-03 14:05_

Most people exit their program status code `1` on an error. To differentiate between program errors, the BSD folks made a guideline with exit codes (>1) to be more specific. For example, if the user doesn't have the necessary permissions, you should exit with `77`.

I'm not trying to advocate rigorously following this. I want to argue from a different perspective: If people tend to use `1` for generic errors, clap should probably _not_ use `1`, so if people want to detect usage errors, they have a chance to do so. Thus, I propose changing the exit code to `64` (or [`exitcode::USAGE`](https://docs.rs/exitcode/1.1.2/exitcode/constant.USAGE.html)).

It seems that only these places need to be touched:

https://github.com/clap-rs/clap/blob/eaa0700e7ed76f37d245a6878deb3b8e81754d6c/src/build/app/mod.rs#L1209

https://github.com/clap-rs/clap/blob/eaa0700e7ed76f37d245a6878deb3b8e81754d6c/src/build/app/mod.rs#L1285

As one might see this as a breaking change for consumers, it need to land before the 3.0 release.

---

_Referenced in [clap-rs/clap#1637](../../clap-rs/clap/pulls/1637.md) on 2020-01-17 15:25_

---

_Comment by @pksunkara on 2020-01-30 19:30_

Merged the relevant PR.

---

_Closed by @pksunkara on 2020-01-30 19:30_

---

_Referenced in [clap-rs/clap#1653](../../clap-rs/clap/pulls/1653.md) on 2020-02-02 06:13_

---

_Referenced in [clap-rs/clap#3332](../../clap-rs/clap/issues/3332.md) on 2022-01-23 22:25_

---

_Referenced in [clap-rs/clap#3426](../../clap-rs/clap/issues/3426.md) on 2022-02-09 09:04_

---

_Referenced in [solana-labs/solana-program-library#4511](../../solana-labs/solana-program-library/issues/4511.md) on 2023-06-08 23:26_

---

_Referenced in [clap-rs/clap#5311](../../clap-rs/clap/pulls/5311.md) on 2024-01-15 22:29_

---
