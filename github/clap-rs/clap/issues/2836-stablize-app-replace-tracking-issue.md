---
number: 2836
title: "Stablize `App::replace` Tracking Issue"
type: issue
state: closed
author: epage
labels:
  - A-builder
  - C-tracking-issue
  - S-blocked
assignees: []
created_at: 2021-10-09T15:42:23Z
updated_at: 2023-03-28T16:48:36Z
url: https://github.com/clap-rs/clap/issues/2836
synced_at: 2026-01-07T13:12:19-06:00
---

# Stablize `App::replace` Tracking Issue

---

_Issue opened by @epage on 2021-10-09 15:42_

Original request: #1603

Original PR: https://github.com/clap-rs/clap/pull/1697

Feature flag: `unstable-replace`

Known issues:
- [x] https://github.com/clap-rs/clap/issues/2011
- [ ] No presence in `--help`

---

_Label `T: enhancement` added by @epage on 2021-10-09 15:42_

---

_Label `C: app` added by @epage on 2021-10-09 15:42_

---

_Comment by @pksunkara on 2021-10-09 15:43_

We should link implementation also, right?

---

_Comment by @epage on 2021-10-09 15:46_

Works for me.

---

_Referenced in [clap-rs/clap#2011](../../clap-rs/clap/issues/2011.md) on 2021-10-09 15:50_

---

_Referenced in [clap-rs/clap#2817](../../clap-rs/clap/pulls/2817.md) on 2021-10-11 15:17_

---

_Label `T: stabilisation` added by @pksunkara on 2021-10-12 20:05_

---

_Label `T: enhancement` removed by @pksunkara on 2021-10-12 20:05_

---

_Comment by @pksunkara on 2021-10-26 20:42_

> So `-l` expands to `--source list` and `-r` expands to `--target residues`?

> The unstable replace feature gets you part way. It would allow a `-l` to expand out but it wouldn't allow `-lr`.

Another issue with this feature as the use case is documented [here](https://github.com/clap-rs/clap/discussions/2945#discussioncomment-1540387)

---

_Referenced in [epage/clapng#158](../../epage/clapng/issues/158.md) on 2021-12-06 20:16_

---

_Referenced in [epage/clapng#212](../../epage/clapng/issues/212.md) on 2021-12-06 22:17_

---

_Referenced in [clap-rs/clap#1030](../../clap-rs/clap/issues/1030.md) on 2021-12-09 17:09_

---

_Label `S-blocked` added by @epage on 2021-12-13 21:22_

---

_Referenced in [clap-rs/clap#4098](../../clap-rs/clap/issues/4098.md) on 2022-08-19 17:29_

---

_Referenced in [spinframework/spin#1117](../../spinframework/spin/issues/1117.md) on 2023-02-14 00:21_

---

_Comment by @epage on 2023-03-23 20:49_

Due to the lack of interest on this tracking issue, I'm considering removing this unstable feature.

Any concerns people want to raise?

---

_Referenced in [clap-rs/clap#1603](../../clap-rs/clap/issues/1603.md) on 2023-03-23 20:50_

---

_Comment by @pksunkara on 2023-03-24 00:54_

Would you happen to have ideas on how to solve the mentioned issues? Maybe by restricting `replace` behavior to only work on subcommands? There aren't any other new issues.

---

_Comment by @epage on 2023-03-24 00:59_

But the problem in #2011 is with subcommands and I thought they were the primary motivation for this.

There is also the item brought up about being invisible in `--help`.

I also feel like there has been little interest expressed in this that it makes me wonder if the feature is worth it in the first place.

---

_Comment by @pksunkara on 2023-03-24 01:05_

Yeah, that's what I meant. The current API replaces any and all input. Maybe we can restrict it to only replace subcommands by doing asserts. That, way we can use the replace config to solve the above-mentioned issues.

> I also feel like there has been little interest expressed in this that it makes me wonder if the feature is worth it in the first place.

People might not be talking about it because they don't have anything to complain.

That's something we might have different opinions about. I would recommend keeping it.

---

_Comment by @epage on 2023-03-24 01:21_

Except for the fact that its unstable which carries its own level of risk, including it being removed.

Features carry a cost and we need to balance that cost against the impact they have on end-users.  Right now, command aliases can be emulated by duplicating the commands.  This is also done in our "default subcommand" example (see the git examples).  People requested that workflow to be streamlined but with us having a workaround, the cost seemed hardly justified.  I feel like this might be another one of those scenarios.

---

_Referenced in [clap-rs/clap#4805](../../clap-rs/clap/pulls/4805.md) on 2023-03-28 16:29_

---

_Closed by @epage on 2023-03-28 16:45_

---

_Closed by @epage on 2023-03-28 16:48_

---

_Referenced in [clap-rs/clap#4948](../../clap-rs/clap/issues/4948.md) on 2023-06-05 14:42_

---

_Referenced in [clap-rs/clap#5727](../../clap-rs/clap/issues/5727.md) on 2024-09-17 21:07_

---

_Referenced in [clap-rs/clap#6031](../../clap-rs/clap/issues/6031.md) on 2025-08-07 20:17_

---
