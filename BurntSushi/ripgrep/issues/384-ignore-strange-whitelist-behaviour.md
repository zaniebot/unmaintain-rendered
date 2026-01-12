```yaml
number: 384
title: "[ignore] Strange whitelist behaviour"
type: issue
state: closed
author: Yamakaky
labels: []
assignees: []
created_at: 2017-02-28T03:22:18Z
updated_at: 2017-03-13T01:10:00Z
url: https://github.com/BurntSushi/ripgrep/issues/384
synced_at: 2026-01-12T16:13:21Z
```

# [ignore] Strange whitelist behaviour

---

_@Yamakaky_

```rust
let overrides = "
    !/*
    /tmp
";
let walker = WalkBuilder::new("/")
                         .overrides(OverrideBuilder::new("/")
                                                    .add(overrides)
                                                    .unwrap()
                                                    .build()
                                                    .unwrap())
                         .build();
for res in  walker {
    let entry = res.unwrap();
    println!("{:?}", entry);
}
```
I expect this code to list the files in `/tmp`, but it also lists the files in all `/`, like `/home`. Did I do something wrong?

---

_Comment by @BurntSushi on 2017-03-13 00:09_

The docs for the [`add`](https://docs.rs/ignore/0.1.7/ignore/overrides/struct.OverrideBuilder.html#method.add) method say:

> Add a glob to the set of overrides.

In your code, you're adding a whole bunch of globs. You need to add them one at a time.

---

_Closed by @BurntSushi on 2017-03-13 00:09_

---

_Comment by @Yamakaky on 2017-03-13 01:10_

Oh, OK, thanks.

---
