---
number: 1616
title: Testing with ArgMatches
type: issue
state: closed
author: MichaelAquilina
labels:
  - C-enhancement
assignees: []
created_at: 2019-12-29T21:15:32Z
updated_at: 2021-03-12T17:12:42Z
url: https://github.com/clap-rs/clap/issues/1616
synced_at: 2026-01-10T01:27:00Z
---

# Testing with ArgMatches

---

_Issue opened by @MichaelAquilina on 2019-12-29 21:15_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.38.0

### Affected Version of clap

2.33.0

### Expected Behavior Summary

I want to write some tests for functions that take `ArgMatches` as input. However this currently does not seem possible due to the fact that you cannot access `MatchedArg` to construct them. There also does not seem to be a way of generating `ArgMatches` from custom inputs (e.g. a list of strings) with `get_matches()`.

I would expect to be able to do something like:

```
let mut matches = ArgMatches::new();
matches.args["foo"] = MatchedArg { occurs: 1, indices: [1], vals: ["red"] };

let result = my_function(&matches);
```

Or alternatively something like

```
let app = get_app_config();
let matches = app.get_matches(&["--foo", "bar", "baz"]);

let result = my_function(&matches);
```

The first option could be easily solved by adding `MatchedArg` to the `pub use` line in lib.rs but I wanted to see what you all thought (perhaps there is already a solution to this that exists but is not documented)

---

_Renamed from "Testing" to "Testing with ArgMatches" by @MichaelAquilina on 2019-12-29 21:15_

---

_Label `C: tests` added by @CreepySkeleton on 2020-02-01 10:51_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 10:51_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 10:51_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 10:51_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 10:51_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 10:51_

---

_Comment by @pickfire on 2020-03-07 15:37_

Is `pub(crate)` useful for this?

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-09 08:15_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:15_

---

_Label `W: 2.x` removed by @pksunkara on 2020-04-09 08:15_

---

_Label `W: 3.x` removed by @pksunkara on 2020-04-09 08:15_

---

_Label `help wanted` removed by @pksunkara on 2020-04-09 08:15_

---

_Referenced in [clap-rs/clap#2290](../../clap-rs/clap/pulls/2290.md) on 2021-01-10 13:37_

---

_Referenced in [clap-rs/clap#2408](../../clap-rs/clap/pulls/2408.md) on 2021-03-11 13:41_

---

_Comment by @mgrachev on 2021-03-12 17:02_

@pksunkara Maybe close this issue?

---

_Comment by @pksunkara on 2021-03-12 17:12_

We recommend using https://docs.rs/clap/3.0.0-beta.2/clap/struct.App.html#method.get_matches_from

---

_Closed by @pksunkara on 2021-03-12 17:12_

---
