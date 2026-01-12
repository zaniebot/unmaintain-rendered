```yaml
number: 1859
title: Regression in error messages when two args from a non-multiple group were passed
type: issue
state: closed
author: CreepySkeleton
labels:
  - E-hard
assignees: []
created_at: 2020-04-24T10:43:48Z
updated_at: 2021-01-30T14:52:03Z
url: https://github.com/clap-rs/clap/issues/1859
synced_at: 2026-01-12T16:14:11Z
```

# Regression in error messages when two args from a non-multiple group were passed

---

_@CreepySkeleton_

This is what it used to look like:
```
error: The argument '--delete' cannot be used with '<base>'
USAGE:
    clap-test <base|--delete>
For more information try --help"
```

After https://github.com/clap-rs/clap/pull/1856 has landed, it looks like
```
error: Found argument '--all' which wasn't expected, or isn't valid in this context
If you tried to supply `--all` as a PATTERN use `-- --all`
USAGE:
    clap-test <-a|--delete>
For more information try --help"
```

This is undesirable and must be corrected, but the bugfix is more important.


---

_Label `T: bug` added by @CreepySkeleton on 2020-04-24 10:43_

---

_Label `T: bug` removed by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `C: errors` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `D: hard` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `P3: want to have` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `T: refactor` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `T: regression` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Label `W: postponed` added by @CreepySkeleton on 2020-04-24 10:44_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-04-24 10:44_

---

_Renamed from "Regression in error messages when two args from non-mulpiple groups were passed" to "Regression in error messages when two args from a non-multiple group were passed" by @CreepySkeleton on 2020-04-24 10:51_

---

_Label `W: 3.x` removed by @pksunkara on 2020-04-24 10:53_

---

_Label `W: postponed` removed by @pksunkara on 2020-04-24 10:53_

---

_Referenced in [clap-rs/clap#1856](../../clap-rs/clap/pulls/1856.md) on 2020-04-24 11:20_

---

_Comment by @issuehunt-oss[bot] on 2020-05-28 15:25_

[@pksunkara](https://issuehunt.io/u/pksunkara) has funded $20.00 to this issue.

---
- Submit pull request via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1859) to receive this reward.
- Want to contribute? Chip in to this issue via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1859).
- Checkout the [IssueHunt Issue Explorer](https://issuehunt.io/issues) to see more funded issues.
- Need help from developers? [Add your repository](https://issuehunt.io/r/new) on IssueHunt to raise funds.

---

_Label `:dollar: Funded on Issuehunt` added by @issuehunt-oss[bot] on 2020-05-28 15:25_

---

_Comment by @pksunkara on 2020-11-07 14:11_

@ldm0 Do you want this one next?

---

_Comment by @ldm0 on 2020-11-07 14:59_

Yep.

---

_Referenced in [clap-rs/clap#2297](../../clap-rs/clap/pulls/2297.md) on 2021-01-20 13:54_

---

_Referenced in [clap-rs/clap#2309](../../clap-rs/clap/pulls/2309.md) on 2021-01-23 07:50_

---

_Comment by @issuehunt-oss[bot] on 2021-01-27 18:49_

[@pksunkara](https://issuehunt.io/u/pksunkara) has cancelled funding for this issue.(Cancelled amount: $20.00) [See it on IssueHunt](https://issuehunt.io/repos/31315121/issues/1859)

---

_Label `:dollar: Funded on Issuehunt` removed by @issuehunt-oss[bot] on 2021-01-27 18:49_

---

_Closed by @pksunkara on 2021-01-30 14:52_

---
