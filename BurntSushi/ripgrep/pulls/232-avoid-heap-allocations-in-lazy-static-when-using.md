```yaml
number: 232
title: Avoid heap allocations in lazy_static when using nightly
type: pull_request
state: closed
author: cesarb
labels: []
assignees: []
base: master
head: lazy_static-nightly
created_at: 2016-11-12T13:51:18Z
updated_at: 2016-11-12T16:19:59Z
url: https://github.com/BurntSushi/ripgrep/pull/232
synced_at: 2026-01-12T18:23:12Z
```

# Avoid heap allocations in lazy_static when using nightly

---

_@cesarb_

On nightly, the lazy_static crate is able to use `const fn` to avoid a heap allocation for each `lazy_static!`.

---

_Comment by @BurntSushi on 2016-11-12 14:00_

Is there a real world use case the benefits from this?


---

_Comment by @cesarb on 2016-11-12 16:17_

It should in theory make ripgrep start slightly faster (less allocations in docopt and in the two `lazy_static!` uses within ripgrep itself), though it's in the noise for me.


---

_Comment by @BurntSushi on 2016-11-12 16:19_

Yeah, I don't at all buy that this has any impact on startup time. (The number of allocations perform by, say, building the regex itself is going to dwarf the one used by `lazy_static!`.) Because of that, I don't think it's worth adding a new feature for it. It will be nice once we get `const fn` on stable Rust, but we can wait for that. Thanks anyway!


---

_Closed by @BurntSushi on 2016-11-12 16:19_

---
