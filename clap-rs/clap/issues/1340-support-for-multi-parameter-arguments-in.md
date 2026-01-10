---
number: 1340
title: "Support for multi-parameter arguments in \"structupt mode\""
type: issue
state: closed
author: jansol
labels: []
assignees: []
created_at: 2018-09-10T05:15:29Z
updated_at: 2020-02-02T05:19:41Z
url: https://github.com/clap-rs/clap/issues/1340
synced_at: 2026-01-10T01:26:49Z
---

# Support for multi-parameter arguments in "structupt mode"

---

_Issue opened by @jansol on 2018-09-10 05:15_

There should be an easy way to implement arguments that require multiple parameters, for à la `--color 0.5 0.5 1.0`.

I'm not sure if this is currently possible with clap's builders, but at least I didn't find any reasonable way to do it in structopt, so this can serve as a tracking issue for that side of clap 3.

In my case I require precisely 3 parameters iff the corresponding argument is present, otherwise I want to use a default value for all 3 parameters. Ideally I'd like to have clap parse these into a struct, tuple or array. Unfortunately currently none of those are properly supported, and manually implementing FromStr for a custom struct means the values have to be passed as a single string, i.e. enclosed in quotes or they'll count as separate arguments -- which from a usage standpoint is just silly.

---

_Comment by @camdencheek on 2018-09-13 23:03_

Have you looked at the [`number_of_values`](https://docs.rs/clap/2.32.0/clap/struct.Arg.html#method.number_of_values) option? This seems to do exactly what you require

---

_Comment by @jansol on 2018-09-14 02:06_

Yes. How does one use that in "structopt mode"?

---

_Renamed from "Support for multi-parameter arguments" to "Support for multi-parameter arguments in "structupt mode"" by @jansol on 2018-09-14 02:06_

---

_Comment by @CreepySkeleton on 2020-02-02 05:19_

@jansol Something like that (`StructOpt` is the same)
```rust
#[derive(Clap)]
struct Opt {
    #[clap(long, number_of_values = 3)]
    color: Vec<f32>
}
```

---

_Closed by @CreepySkeleton on 2020-02-02 05:19_

---
