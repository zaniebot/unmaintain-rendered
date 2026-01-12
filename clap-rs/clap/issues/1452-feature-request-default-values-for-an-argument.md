```yaml
number: 1452
title: "[Feature request] Default values for an argument with multiple values"
type: issue
state: closed
author: tprodanov
labels: []
assignees: []
created_at: 2019-04-08T23:10:10Z
updated_at: 2020-04-07T09:12:19Z
url: https://github.com/clap-rs/clap/issues/1452
synced_at: 2026-01-12T16:14:10Z
```

# [Feature request] Default values for an argument with multiple values

---

_@tprodanov_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rust 1.33.0

### Affected Version of clap

clap 2.33.0

### Feature Request

As I understand, there is no way to specify default values for arguments that have number of values higher than 1. For example, the argument requires exactly two values, and there is a way to name this values with `value_names(&[...])`, but there is no way to specify default values like `default_values(&[...])`.

### Expected Behavior

I can imagine two ways to have this feature:
- `.default_values(&["A", "B"])`
- `.default_value("A B")`

### Actual Behavior

The first option is not implemented, and if you specifying default values like `default_value("A B")` would produce one default value with a space in it.

### Sample Code

```
use clap::{App, Arg};

fn main() {
    App::new("multiple_default")
        .arg(Arg::with_name("argument")
                .short("a")
                .long("argument")
                .takes_value(true)
                .number_of_values(2)
                .value_names(&["INT", "INT"])
                .default_value("1 2")
                ).get_matches();
}
```

This code would compile, and it would show help on `./multiple_default --help`, and it would work fine with `./multiple_default --argument 1 2`. However, running it with `./multiple_default` would produce the following error:
```
The argument '--argument <INT> <INT>' requires 2 values, but 1 was provided
```

### Workarounds

- It is possible to not specify default values at all. However, in that case there is no automatic default values in the help message, and they will not be colored if I manually add them in there. Also, in this case I need to specify default values twice, once in the help message, and the second time when I extract the value.
- It is also possible to split the argument into several arguments. However, it increases the size of the help message and makes it more complicated if these values are highly related. Also, it is possible that there is no fixed number of values, for example 4 to 7 values, and the default are 4 values.
- Allow only one entry and then manually split on spaces and check the number of entries. However, this does not allow for multiple value names, as the following code would still fail (`The argument '--argument <INT> <INT>' requires 2 values, but 1 was provided`)
```
.value_names(&["INT", "INT"])
.number_of_values(1)
.default_value("1 2")
```
If I specify value names as `.value_name("INT INT")` it would look like `<INT INT>` in the help message.

### Possible implementation

In the `Valued` struct to replace `default_val: Option<&'b OsStr>` with `default_vals: Option<VecMap<&'b OsStr>>`, the same as for `val_names`.

Then we can have two functions:
- `Arg::default_value(self, val: &'a str)`
- `Arg::default_values(self, vals: &[&'a str])`, also similar to `value_names`

Another implementation would be to just allow single default value and then split in on spaces, but this sounds more like a workaround than a solid solution.

### Summary

There is no way to specify several default values. If `min_values > 1` it is impossible to add any default values at all.

I understand that it is not so straightforward to implement, however, I think that it is a logical feature, that is consistent with multiple value names, and which is neccessary for an argument parser that allows multiple values.


---

_Renamed from "Default values for an argument with multiple values" to "[Feature request] Default values for an argument with multiple values" by @tprodanov on 2019-04-10 17:34_

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 14:25_

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 09:10_

---

_Label `C: args` added by @CreepySkeleton on 2020-02-06 09:10_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-06 09:10_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-06 09:10_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 09:10_

---

_Added to milestone `3.0.0-alpha.2` by @CreepySkeleton on 2020-02-06 09:10_

---

_Removed from milestone `3.0.0-alpha.2` by @CreepySkeleton on 2020-02-06 09:10_

---

_Added to milestone `3.0.0-alpha.3` by @CreepySkeleton on 2020-02-06 09:10_

---

_Removed from milestone `3.0.0-alpha.3` by @CreepySkeleton on 2020-02-06 09:11_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-06 09:11_

---

_Comment by @CreepySkeleton on 2020-02-06 09:11_

Triaging it for before 3.0 because it seems to be commonly asked feature

---

_Comment by @NilsIrl on 2020-02-14 12:08_

For someone looking to implement this, is there anything that needs to be changed to the API described by @tprodanov ?

---

_Comment by @Dylan-DPC-zz on 2020-02-14 12:13_

Yeah go ahead. I like the idea of having the slice as a parameter

---

_Comment by @pksunkara on 2020-02-14 12:18_

Yup, slice as a parameter is the best here.

---

_Comment by @CreepySkeleton on 2020-02-14 12:36_

`default_values(&["a", "b"])` is good, `default_values("a b")` is not.

---

_Comment by @NilsIrl on 2020-02-14 21:00_

Hasn't this been already implemented in https://github.com/clap-rs/clap/commit/c3b7c01f54d95c4907e33256f9d4dfe5f20ce1cc?

---

_Comment by @CreepySkeleton on 2020-02-14 21:26_

Oops, yeah, it done already 😮 

---

_Closed by @CreepySkeleton on 2020-02-14 21:26_

---

_Comment by @kahing on 2020-04-05 22:48_

will there be a release soon with `default_values`?

---

_Comment by @pksunkara on 2020-04-05 22:50_

It won't be backported to v2 since it is a feature. Expect this to be in v3

---

_Comment by @kahing on 2020-04-05 22:56_

@pksunkara thanks for the super quick response! It's a little hard to figure out how to use this from https://github.com/clap-rs/clap/commit/c3b7c01f54d95c4907e33256f9d4dfe5f20ce1cc . Could you provide a small example?

---

_Comment by @NilsIrl on 2020-04-06 08:42_

> Could you provide a small example?

https://doc.rust-lang.org/cargo/reference/overriding-dependencies.html is what you're looking for.

---

_Comment by @pksunkara on 2020-04-07 09:12_

This is an example in the test, https://github.com/clap-rs/clap/commit/c3b7c01f54d95c4907e33256f9d4dfe5f20ce1cc#diff-d8218b866ec5a9148d9d1d31ca8c87b3R457

---
