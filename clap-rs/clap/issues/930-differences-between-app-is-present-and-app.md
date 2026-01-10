---
number: 930
title: "Differences between App::is_present and App::occurrences_of"
type: issue
state: closed
author: RazrFalcon
labels: []
assignees: []
created_at: 2017-04-11T17:39:28Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/930
synced_at: 2026-01-10T01:26:38Z
---

# Differences between App::is_present and App::occurrences_of

---

_Issue opened by @RazrFalcon on 2017-04-11 17:39_

I have an option with a default value and I want to check that this value was set explicitly. How can I do this?

Example:
```rust
    let m = App::new("myprog")
        .arg(Arg::with_name("test")
        .short("t")
        .default_value("yes"))
        .get_matches();

    println!("{} {}", m.is_present("test"), m.occurrences_of("test"));
```

```
cargo run
true 0

cargo run -- -t yes
true 1
```

So `is_present` returns `true` even when a value is not set explicitly but has a default value. And `occurrences_of` processes such case "correctly".

The idea is that I want an option like `--no-defaults`, which will reset/ignore default values, but that I have to detect cases when user set default value manually.

Can I rely on `occurrences_of`, as in the example above, or this behavior may change in future?

---

_Comment by @kbknapp on 2017-04-18 00:45_

> Can I rely on occurrences_of, as in the example above, or this behavior may change in future?

Yes you can rely on it. This this the primary reasons for `occurrences_of`, i.e. to know how many times a particular arg was explicitly used.

`is_present` is primarily meant to be used with flags (i.e. args with no associated value).

---

_Label `T: RFC / question` added by @kbknapp on 2017-04-18 00:46_

---

_Closed by @kbknapp on 2017-04-18 00:46_

---
