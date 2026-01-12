```yaml
number: 430
title: "Update (1.5.5 -> 2.1.1) broke my code"
type: issue
state: closed
author: matthiasbeyer
labels: []
assignees: []
created_at: 2016-02-20T20:22:47Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/430
synced_at: 2026-01-12T16:14:09Z
```

# Update (1.5.5 -> 2.1.1) broke my code

---

_@matthiasbeyer_

Nothing critical, though you might want to know:

``` rust
if let Some(spec) = matches.values_of("header") {
    // ...
    debug!("{:?}", spec);
}
```

As the return value of the `ArgMatches::values_of()` now returns something which does not implement `Debug`, the update ended in a compile error.

> ArgMatches::values_of returns an Values now which implements Iterator (should not break any code)

As said, this is nothing serious for me, and this issue is just FYI. Feel free to close this issue after reading.


---

_Referenced in [clap-rs/clap#433](../../clap-rs/clap/pulls/433.md) on 2016-02-21 18:00_

---

_Comment by @kbknapp on 2016-02-22 05:37_

Yeah, there were a few breaking changes from 1x to 2x, hence the major version bump. :smile:

Now for your specific issue, instead of returning a `Vec` like we used to, that method now returns an iterator which is far more flexible, but it does have the downside like you noted. To fix that code, all you need to do is add

``` rust
if let Some(spec) = matches.values_of("header") {
    // ...
    debug!("{:?}", spec.collect::<Vec<_>>());
}
```

I'll leave this open until you confirm that works for you :wink:


---

_Label `T: RFC / question` added by @kbknapp on 2016-02-22 05:38_

---

_Closed by @matthiasbeyer on 2016-02-22 06:54_

---
