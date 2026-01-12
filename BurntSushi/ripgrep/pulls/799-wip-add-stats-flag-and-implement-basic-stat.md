```yaml
number: 799
title: "[WIP] Add --stats flag and implement basic stat tracking (closes #411)"
type: pull_request
state: closed
author: balajisivaraman
labels: []
assignees: []
base: master
head: fix_411
created_at: 2018-02-13T18:46:06Z
updated_at: 2018-03-10T16:22:57Z
url: https://github.com/BurntSushi/ripgrep/pull/799
synced_at: 2026-01-12T18:23:13Z
```

# [WIP] Add --stats flag and implement basic stat tracking (closes #411)

---

_@balajisivaraman_

Creating this PR for review purposes and as a WIP thread for adding the `--stats` flag to ripgrep.

- Implement tracking for basic stats (no. of matches, files with matches, files searched, and time taken)
- Ensure `--stats` is ignored when `--files`, `--files-with-matches` or `--files-without-match` are passed
- Implement tracking of bytes searched for `--no-mmap` searches using the `counting_reader` trick. This will require changing the worker's `run` method to return an Optional `bytes_searched` attribute along with the existing `match_count`. We also need to ensure this happens only when `--stats` is passed.

Other thoughts:

- Have used the default `Duration` implementation from `std` as did not want to pull in any other dependencies such as [`time`](https://crates.io/crates/time) or [`chrono`](https://crates.io/crates/chrono), and this served the purpose very well.
- Have defaulted the time to always display in seconds since this is what `ag` does as well. `rg` also prints only 3 numbers after the decimal point, as opposed to `ag`'s 6.

  The only reasoning behind this is that Rust only allows us to control the digits printed after the decimal point, not overall. If we run a very long search, we don't want to print something like `2751.648931 seconds`. So have chosen the nicer option of 3 digits after the decimal point.

  I think `ag` prints 6 digits overall, with the number of digits printed after the decimal point depending on the overall width of the string. (This is the behaviour I've observed; have not verified this with the code.) 

---

_Comment by @balajisivaraman on 2018-02-20 04:26_

I'll close this PR as the current implementation is simply wrong. When I looked at the code for this originally, I overlooked the fact that the searchers return matching line count and not the individual match count, which is what we want for the `--stats` flag. My apologies for that.

I'll work on the proper implementation, which I expect to be a bit more involving than this one. In the meantime, I don't want this clogging up the PR space here.

---

_Closed by @balajisivaraman on 2018-02-20 04:26_

---

_Branch deleted on 2018-02-20 04:27_

---

_Review comment by @BurntSushi on `src/app.rs`:1460 on 2018-02-20 12:31_

Can you put the `");` on its own line please?

---

_Review comment by @BurntSushi on `src/app.rs`:1464 on 2018-02-20 12:31_

Remove this blank line please.

---

_Review comment by @BurntSushi on `src/main.rs`:398 on 2018-02-20 12:35_

All lines of source code in ripgrep should be wrapped to 79 columns inclusive (excepting URLs or other things that are inconvenient to wrap). Please see examples of other function declarations for formatting tips.

---

_@BurntSushi reviewed on 2018-02-20 12:35_

@balajisivaraman Thanks for working on this! I know you closed this, but you might consider that it is possible to get this merged without much additional work. For example, if you change "match count" to "matched line count" (or similar), then that would be correct and still useful IMO. :) I defer to you! Regardless, I left some small amount of feedback.

---

_Branch restored on 2018-02-20 14:16_

---

_Comment by @balajisivaraman on 2018-02-20 15:06_

@BurntSushi, I wouldn't mind getting this PR merged at all. :-)

The only reason why I closed this was I'm going to begin work on the full implementation of it with bytes searched and individual count, so I thought we wouldn't want this anyway.

But seeing as that is more involving change with multiple moving parts, it might take some time. So it makes sense to go ahead with this partial implementation in the meanwhile.

I'll update the PR to reflect your comments. Thanks! :-)

---

_Reopened by @balajisivaraman on 2018-02-20 15:06_

---

_Closed by @BurntSushi on 2018-03-10 16:00_

---

_Comment by @BurntSushi on 2018-03-10 16:01_

@balajisivaraman This was great work, thanks so much! I merged this in https://github.com/BurntSushi/ripgrep/commit/00520b30f5f38e543e17b1a4cc5e8417bc488ea4 with no real changes other than fixing up style (lines should be limited to 79 columns inclusive) and resolving conflicts. I also tweaked the wording of output slightly to indicate that total matches actually corresponds to the total number of matched lines. (So we leave the door open to emit another statistic that reports the total matches.)

---

_Branch deleted on 2018-03-10 16:05_

---

_Comment by @balajisivaraman on 2018-03-10 16:07_

@BurntSushi, Thank you for merging this. And my apologies for not updating this PR. I was waiting for the other PR (`match_line_count`) to get merged so I could update it properly and completely forgot about it. Thanks again. 😄 

---

_Comment by @BurntSushi on 2018-03-10 16:22_

@balajisivaraman No worries! Sometimes my work style isn't amenable to waiting too long, and is susceptible to short bursts of maintenance activity. :-) Context switches are extremely expensive for me, so I figured that I would just amortize them by processing all of your PRs, which all were somewhat related!

---
