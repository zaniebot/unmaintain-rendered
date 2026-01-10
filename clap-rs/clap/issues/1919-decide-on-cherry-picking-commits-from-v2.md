---
number: 1919
title: Decide on cherry picking commits from v2
type: issue
state: closed
author: pksunkara
labels:
  - C-bug
assignees: []
created_at: 2020-05-11T19:09:02Z
updated_at: 2020-05-13T06:04:57Z
url: https://github.com/clap-rs/clap/issues/1919
synced_at: 2026-01-10T01:27:09Z
---

# Decide on cherry picking commits from v2

---

_Issue opened by @pksunkara on 2020-05-11 19:09_

So, I am working on the CHANGELOG which means looking at the commit history. I found the following commits which looks like that wasn't cherry picked to v3.

- [ ] ~ef92e2b639ed305bdade4741f60fa85cb0101c5a~
- [x] bada4f525acf9434518ac4bb3d0e1638a10163a0
- [x] e531182e22bc598da1ca327b434be4f85a6eaf4e
- [x] eaa2d8144ff704c8a4215edc1a51f700ebaf9761
- [x] fed0e85ac18bf2ea0f5adde5b71cff8afaadfd16
- [x] 0a26c2c3e3ad8e827345dc59974faba8e2094206
- [x] a563b8e635e84b52bac740eaad54c238320d7ac3
- [x] 9e7a352e13aaf8025d80f2bac5c47fb32528672b
- [x] 0eca1b42a587fda995a1a1be699c6be58a1a4b87
- [x] ~9601c95a03d2b82bf265c328b4769238f1b79002 (partial?)~
- [x] dbd80a2c72381691194fb8f916726d9b745ffe7d
- [x] 5a08ff295b2aa6ce29420df6252a0e3ff4441bdc
- [x] ~356c69e508fd25a9f0ea2d27bf80ae1d9a8d88f4~

---

_Label `T: bug` added by @pksunkara on 2020-05-11 19:09_

---

_Added to milestone `3.0` by @pksunkara on 2020-05-11 19:09_

---

_Assigned to @pksunkara by @pksunkara on 2020-05-11 19:29_

---

_Comment by @pksunkara on 2020-05-11 22:03_

Note: Jan 22, 2018 is when the v2 branch has diverged.

---

_Referenced in [clap-rs/clap#1920](../../clap-rs/clap/pulls/1920.md) on 2020-05-11 23:14_

---

_Comment by @CreepySkeleton on 2020-05-12 01:26_

I reimplemented https://github.com/clap-rs/clap/commit/9601c95a03d2b82bf265c328b4769238f1b79002 in #1883 

---

_Comment by @pksunkara on 2020-05-12 01:28_

Yup, which is why I only cherry picker the test from that commit.

On Tue, May 12, 2020, 03:27 CreepySkeleton <notifications@github.com> wrote:

> I reimplemented 9601c95
> <https://github.com/clap-rs/clap/commit/9601c95a03d2b82bf265c328b4769238f1b79002>
> in #1883 <https://github.com/clap-rs/clap/pull/1883>
>
> —
> You are receiving this because you were assigned.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/1919#issuecomment-627053481>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU362RJ7OSBXX6OWB6FTRRCQWHANCNFSM4M6FPADA>
> .
>


---

_Comment by @kbknapp on 2020-05-12 01:58_

ef92e2b is not implemented on v3, but I also don't have strong feelings about it. It changes the error message when `suggestions` are enabled. The old v2 and current v3 implementation will suggest a subcommand with the appropriate args even when no subcommand is used. Whereas that commit only suggest a subcommand when that subcommand is used.

I think the current v3 (and old v2) is fine, but if we want to cherry pick it across (and I'm not sure it can be cleaning picked) that's fine too.

---

_Referenced in [clap-rs/clap#1923](../../clap-rs/clap/issues/1923.md) on 2020-05-12 08:52_

---

_Comment by @pksunkara on 2020-05-12 08:52_

I moved it to a separate issue.

---

_Closed by @bors[bot] on 2020-05-13 06:04_

---
