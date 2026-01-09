---
number: 2326
title: "A `require*` method that expressing `and(&&)` condition when requesting an argument"
type: issue
state: closed
author: zzz845zz
labels:
  - ":money_with_wings: $10"
assignees: []
created_at: 2021-02-04T09:49:59Z
updated_at: 2021-02-28T13:50:51Z
url: https://github.com/clap-rs/clap/issues/2326
synced_at: 2026-01-07T13:12:19-06:00
---

# A `require*` method that expressing `and(&&)` condition when requesting an argument

---

_Issue opened by @zzz845zz on 2021-02-04 09:49_

### Make sure you completed the following tasks
- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case
I think the function that requires some arguments only if multiple conditions are satisfied is helpful.

The example of `required_ifs` below is similar to `or(||)` expression, not `and(&&)` (from [here](https://docs.rs/clap/2.33.3/clap/struct.Arg.html#examples-27)). 
- In this example, `cfg` is required when `--extra=val` **or** `--option=spec`
- I want a way to require `cfg` when `--extra=val` **and** `--option=spec`
```rust
let res = App::new("prog")
    .arg(Arg::with_name("cfg")
        .required_ifs(&[
            ("extra", "val"),
            ("option", "spec")
        ])
        .takes_value(true)
        .long("config"))
    .arg(Arg::with_name("extra")
        .takes_value(true)
        .long("extra"))
    .arg(Arg::with_name("option")
        .takes_value(true)
        .long("option"))
    .get_matches_from_safe(vec![
        "prog", "--option", "other"
    ]);

assert!(res.is_ok()); // We didn't use --option=spec, or --extra=val so "cfg" isn't required
```

For more simply, the case above likes
```c
if (extra=="val" || option=="spec") {
    // use cfg!!
}
```

But what i want is the example below
```c
if (extra=="val" && option=="spec") {
    // use cfg!!
} 
```
### Additional context
Related to #2323 

---

_Label `T: new feature` added by @zzz845zz on 2021-02-04 09:49_

---

_Comment by @pksunkara on 2021-02-04 11:59_

The name should be `require_if_eq_all`

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-02-04 11:59_

---

_Added to milestone `3.1` by @pksunkara on 2021-02-04 12:00_

---

_Referenced in [clap-rs/clap#2371](../../clap-rs/clap/pulls/2371.md) on 2021-02-27 18:14_

---

_Closed by @pksunkara on 2021-02-28 13:50_

---
