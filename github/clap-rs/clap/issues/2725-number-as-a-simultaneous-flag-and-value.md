---
number: 2725
title: Number as a simultaneous flag and value
type: issue
state: closed
author: mbhall88
labels: []
assignees: []
created_at: 2021-08-19T01:53:39Z
updated_at: 2021-09-23T12:43:26Z
url: https://github.com/clap-rs/clap/issues/2725
synced_at: 2026-01-07T13:12:19-06:00
---

# Number as a simultaneous flag and value

---

_Issue opened by @mbhall88 on 2021-08-19 01:53_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33

### Describe your use case

I would like to have a special option that I can designate that the flag passed on the command line is the actual value also.

### Describe the solution you'd like

The feature I am asking for is effectively how the compression level flag in `gzip` works. i.e., `gzip -9` indicates the best compression, `gzip -1` the fastest, and `gzip -5` is in-between.

The way I could envision this working would be to have a special symbol `#` to indicate this, something like

```rust
let m = App::new("gzip")
    .arg(Arg::with_name("#"))
    .get_matches_from(vec![
        "gzip", "-3"
    ]);
assert!(m.is_present("#"));
assert_eq!(m.value_of("#"), Some(3));
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @mbhall88 on 2021-08-19 01:53_

---

_Comment by @pksunkara on 2021-08-19 07:01_

Hey, this is definitely possible on 3.0.0-beta.4 and should be possible on 2.33 too. Not sure what gave you the impression that it is not. Please read the documentation and try it out and open a bug report if it's not working.

---

_Closed by @pksunkara on 2021-08-19 07:01_

---

_Comment by @mbhall88 on 2021-08-19 07:36_

I did read the documentation, but obviously I missed the relevant option for doing this. Are you able to point me in the right direction?

---

_Comment by @pksunkara on 2021-08-19 08:44_

What's the value you are getting for `m.is_present("#")` and `m.value_of("#")`?

---

_Comment by @mbhall88 on 2021-08-19 23:09_

When I try and build this I get

```
error: Found argument '-3' which wasn't expected, or isn't valid in this context

USAGE:
    gzip [#]

For more information try --help
```

---

_Comment by @pksunkara on 2021-08-31 10:48_

Use https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.allow_hyphen_values for that arg.

---

_Comment by @mbhall88 on 2021-09-01 00:52_

I'm still getting the same error.

```rust
use clap::{Arg, App};

fn main() {
    let m = App::new("gzip")
        .arg(Arg::with_name("#").allow_hyphen_values(true))
        .get_matches_from(vec!["gzip", "-3"]);
    assert!(m.is_present("#"));
    assert_eq!(m.value_of("#"), Some("3"));
}
```

```
$ cargo run
```

---

_Locked by @clap-rs on 2021-09-23 12:43_

---
