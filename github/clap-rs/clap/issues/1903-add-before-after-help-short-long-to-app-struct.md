---
number: 1903
title: "add {before/after}_help_{short/long} to App struct"
type: issue
state: closed
author: jeffka11
labels:
  - A-help
assignees: []
created_at: 2020-05-03T16:15:09Z
updated_at: 2020-07-19T12:51:27Z
url: https://github.com/clap-rs/clap/issues/1903
synced_at: 2026-01-07T13:12:19-06:00
---

# add {before/after}_help_{short/long} to App struct

---

_Issue opened by @jeffka11 on 2020-05-03 16:15_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I'm looking to use the help menu to replace creating a separate man page. To not bombard users with info, I'd like to include detailed information when they use the long --help flag for sections that need more detail. As a random example of more detail, if you "man curl", you'll see sections at the end explaining error codes that you wouldn't see with "curl --help" and I think {before/after}_help_{short/long} would provide that location.


### Describe the solution you'd like

```
let myapp = App::new("My App")
    .after_help("footer message")  // this already exists
    .before_help("header message")  // this already exists

    .after_help_short("a concise footer")  // proposal, could be redundant with after_help
    .after_help_long("a much longer footer")  // proposal
    .before_help_short("a short header")  // proposal, could be redundant with before_help
    .before_help_long("a verbose header")  // proposal
```


### Alternatives, if applicable

make man pages or a hook into cargo doc from clap, although I think the suggested solution is easier.


---

_Label `T: new feature` added by @jeffka11 on 2020-05-03 16:15_

---

_Comment by @kbknapp on 2020-05-04 17:08_

I think this would be implemented as `{after,before}_help_long` and allow the current `{before,after}_help` serve as the `short` so that we don't have redundant APIs. This also would go along with the `short` somewhat being the default in `clap` as with all other settings that where both a `long` and `short` co-exist.

---

_Added to milestone `3.1` by @pksunkara on 2020-05-05 08:39_

---

_Label `C: help message` added by @CreepySkeleton on 2020-06-30 09:38_

---

_Label `D: easy` added by @CreepySkeleton on 2020-06-30 09:38_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-06-30 09:38_

---

_Referenced in [clap-rs/clap#2008](../../clap-rs/clap/pulls/2008.md) on 2020-07-11 18:54_

---

_Closed by @bors[bot] on 2020-07-19 12:51_

---
