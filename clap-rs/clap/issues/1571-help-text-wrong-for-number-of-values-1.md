---
number: 1571
title: help text wrong for number_of_values=1
type: issue
state: closed
author: metajack
labels:
  - C-enhancement
  - A-help
  - E-help-wanted
  - ":money_with_wings: $5"
assignees: []
created_at: 2019-10-10T15:34:54Z
updated_at: 2021-08-17T09:17:18Z
url: https://github.com/clap-rs/clap/issues/1571
synced_at: 2026-01-10T01:26:58Z
---

# help text wrong for number_of_values=1

---

_Issue opened by @metajack on 2019-10-10 15:34_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

Any

### Affected Version of clap

clap 2.33

### Behavior Summary

I'm moving this bug from structopt (TeXitoi/structopt#267) repo, since it is really about clap. Please forgive the syntax differences, but I think it should be clear how it maps to clap.

For a field defined like:
```
    #[structopt(long="package", short="p", name="name", number_of_values=1)]
    roots: Vec<String>,
```

The help text implies you can specify multiple values per option:
```
    -p, --package <name>...
```

It should probably remove the `...` in this case. Or at least it seems like the ability to specify an option multiple times should look different than allowing multiple values for a single option. I interpret `<value>...` as lots of values.

---

_Comment by @CreepySkeleton on 2019-10-10 15:57_

Pure `clap` equivalent:
```rust
Arg::new("name")
    .long("package")
    .short("p"),
    .number_of_values(1)
    .takes_value(true)
    .multiple_values(true)
```

---

_Comment by @jdollar on 2019-10-12 10:49_

Hey. I'd be more than willing to take this one, but I wanted to try and get some clarification on what we would expect in this scenario? I'm not really finding any good documentation out there for how people are detailing out that a parameter can be used repeatedly. I see tons on where the argument can have multiple, but no clear examples on where the parameter repeats. At least no clear example in terms of how the help text is ideally displayed in such a case.

In this case should we just completely remove the ellipsis and rely on the developer using clap to provide a description for that argument that informs the user that it can be used multiple times? Basing that suggestoin off the documentation for multiple boolean occurrences here: https://picocli.info/#_multiple_occurrences.

---

_Comment by @CreepySkeleton on 2020-02-01 11:31_

I like @jdollar proposal.

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 11:32_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 11:32_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 11:32_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 11:32_

---

_Referenced in [clap-rs/clap#1458](../../clap-rs/clap/issues/1458.md) on 2020-02-01 14:24_

---

_Comment by @pksunkara on 2020-02-01 19:37_

Instead of that, we can remove the ellipsis if the `number_of_values` is 1. This sounds like a much simple and straightforward solution to me.

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-02 02:38_

---

_Removed from milestone `3.0` by @CreepySkeleton on 2020-02-02 02:38_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-02 02:38_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-06-16 03:38_

---

_Assigned to @ldm0 by @ldm0 on 2021-08-14 10:17_

---

_Referenced in [clap-rs/clap#2701](../../clap-rs/clap/pulls/2701.md) on 2021-08-15 01:15_

---

_Closed by @pksunkara on 2021-08-17 09:17_

---

_Referenced in [coreos/coreos-installer#740](../../coreos/coreos-installer/pulls/740.md) on 2022-01-04 22:34_

---
