```yaml
number: 12576
title: Add delay between updating a file
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-flaky-file-watching-tests
created_at: 2024-07-30T10:11:31Z
updated_at: 2024-07-30T16:53:13Z
url: https://github.com/astral-sh/ruff/pull/12576
synced_at: 2026-01-10T21:47:02Z
```

# Add delay between updating a file

---

_Pull request opened by @MichaReiser on 2024-07-30 10:11_

## Summary

This PR should fix some flaky integration tests in file watching. 

The test became flaky with https://github.com/astral-sh/ruff/pull/12566 because we now only write the last
modified timestamp if it has changed. However, the last modified timestamp might remain unchanged
if the write operations happen only a few ns seconds a part and the duration is larger than the system's precision of the last modified time.

## Test Plan

I can't reproduce the test failures localy, which makes it difficult to verify that the flakiness is fixed.
We can increment the delay if necessary. 

<!-- How was it tested? -->


---

_Review requested from @carljm by @MichaReiser on 2024-07-30 10:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-30 10:11_

---

_Label `red-knot` added by @MichaReiser on 2024-07-30 10:11_

---

_Comment by @github-actions[bot] on 2024-07-30 10:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot/tests/file_watching.rs`:188 on 2024-07-30 10:52_

I'm concerned by the warnings in the documentation here:

> **Disclaimer**: These system calls might change over time.

And

> Many I/O functions throughout the standard library are documented to indicate what various library or syscalls they are delegated to. This is done to help applications both understand what’s happening under the hood as well as investigate any possibly unclear semantics. Note, however, that this is informative, not a binding contract. The implementation of many of these functions are subject to change over time and may call fewer or more syscalls/library functions.

(https://doc.rust-lang.org/nightly/std/time/struct.SystemTime.html#platform-specific-behavior and https://doc.rust-lang.org/nightly/std/io/index.html#platform-specific-behavior respectively.)



---

_@AlexWaygood reviewed on 2024-07-30 11:28_

In general I share @carljm's concerns from https://github.com/astral-sh/ruff/pull/12382#discussion_r1685218148 about using hardcoded sleeps in tests. It's usually pretty hard to get them right and avoid flakiness.

What if we did something like this?

```rs
use std::time::SystemTime;

fn next_io_tick() {
    let start = SystemTime::now();
    std::thread::sleep(Duration::from_nanos(100));
    // Continue sleeping until the system clock
    // recognises that the time is later than it was
    // when we started this function...
    loop {
        let Ok(elapsed) = SystemTime::now().duration_since(start) else {
            std::thread::sleep(Duration::from_nanos(50));
            continue;
        }
        if elapsed.as_nanos() == 0 {
            std::thread::sleep(Duration::from_nanos(50));
            continue;
        }
        return;
    }
}

---

_Comment by @MichaReiser on 2024-07-30 11:43_

I also share the concern about sleeping in tests, but I'm not sure if it warrants the extra complexity. The only "proper" solution would be to create a file and write to it in a spin loop until the last modified time changes. Because only this approach would account for different precisions between different file systems. 

I would favor us leaning towards the *easiest* fix and to only rely on more complex solution if the flakiness remains and proves a problem in CI.

Regarding the warning in the documentation. This is a test and not production code. We can change the code if Rust decides to change some precision. 

---

_Comment by @MichaReiser on 2024-07-30 11:51_

I don't mind a proper solution but I want to avoid "wasting" time on it if a much simpler solution works reasonably well. We could even bump the timeout to 100ms or 1s without regressing the CI time significantly. 

---

_Comment by @AlexWaygood on 2024-07-30 12:04_

Slowing down CI isn't my primary concern here; Ruff's CI speed is ~okay, and I agree that a tiny sleep here isn't going to make a material difference considering the time it takes to build Ruff in CI. My primary concern is that adding the sleep won't get rid of the flakes completely, even if it's a large sleep that "should" be guaranteed to be longer than the system clock precision. That's been my experience working on other projects such as CPython.

Maybe adding this sleep means that the test only flakes 1 in 500 times instead of 1 in 10 times. That's okay when you only have 1 test like this, but it ends up creating a reasonable amount of test flakiness in CI if we have 100 tests like this in a few weeks.

I'm okay with this landing if you're confident it fixes the flakes, but I'd _prefer_ a more principled solution, especially if we expect to use the `next_io_tick()` function more widely in our tests going forward.

---

_Comment by @carljm on 2024-07-30 12:47_

I don't like sleeping in tests, but it's probably unavoidable in this case, and I suspect the most important factor here is that we shouldn't ever have hundreds or thousands of tests relying on this; most of our tests should not be integration tests at a level where we are testing file-watching responsiveness. Plus, as long as we _have_ the abstraction of the `next_io_tick` function in place, we can always change its implementation as needed in the future without needing to change a bunch of tests.

IMO the more likely issue that will cause us to have more flaky tests in future is just the fact that it's required to remember to call this manually in the test, rather than having it built-in to a file-writing utility used by all the file-watching tests.

---

_@carljm approved on 2024-07-30 12:48_

---

_@AlexWaygood approved on 2024-07-30 12:57_

---

_Merged by @MichaReiser on 2024-07-30 16:31_

---

_Closed by @MichaReiser on 2024-07-30 16:31_

---

_Branch deleted on 2024-07-30 16:31_

---

_Comment by @AlexWaygood on 2024-07-30 16:53_

The test still appears to be flaky: https://github.com/astral-sh/ruff/actions/runs/10166452757/job/28116662757?pr=12582

---
