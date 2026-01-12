```yaml
number: 1102
title: "puffin-client: rejigger error type"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/client-smaller-error
created_at: 2024-01-25T18:10:26Z
updated_at: 2024-01-25T18:13:23Z
url: https://github.com/astral-sh/uv/pull/1102
synced_at: 2026-01-12T16:04:25Z
```

# puffin-client: rejigger error type

---

_@BurntSushi_

This PR changes the error type to be boxed internally so that it uses
less size on the stack. This makes functions returning `Result<T,
Error>`, in particular, return something much smaller.

The specific thing that motivated this was Clippy lints firing when I
tried to refactor code in this crate.

I chose to achieve boxing by splitting the enum out into a separate
type, and then wiring up the necessary `From` impl to make error
conversions easy, and then making `Error` itself opaque. We could expose
the `Box`, but there isn't a ton of benefit in doing so because one
cannot pattern match through a `Box`.

This required using more explicit error conversions in several places.
And as a result, I was able to remove all `#[from]` attributes on
non-transparent error variants.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-25 18:10_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-01-25 18:10_

---

_Review requested from @konstin by @BurntSushi on 2024-01-25 18:10_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-25 18:10_

---

_Comment by @BurntSushi on 2024-01-25 18:11_

I was secretly hoping we'd get a surprise speed-up from this (although that wasn't necessarily my motivation for doing this), but alas, in ad hoc benchmarks (in the warm case) perf seems unchanged:

```
(.venv) [andrew@duff puffin]$ hyperfine -w5 "puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null"
Benchmark 1: puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     279.4 ms ±   2.6 ms    [User: 265.6 ms, System: 116.6 ms]
  Range (min … max):   275.1 ms … 283.7 ms    10 runs

Benchmark 2: puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     280.6 ms ±   3.9 ms    [User: 263.1 ms, System: 120.6 ms]
  Range (min … max):   273.7 ms … 285.8 ms    10 runs

Summary
  puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.00 ± 0.02 times faster than puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

---

_@charliermarsh approved on 2024-01-25 18:12_

I'm a fan of a little less `#[from]`, especially when we don't have `#[transparent]`.

---

_Merged by @BurntSushi on 2024-01-25 18:13_

---

_Closed by @BurntSushi on 2024-01-25 18:13_

---

_Branch deleted on 2024-01-25 18:13_

---
