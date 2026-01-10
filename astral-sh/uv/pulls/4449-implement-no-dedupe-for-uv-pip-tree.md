```yaml
number: 4449
title: "implement `--no-dedupe` for `uv pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels: []
assignees: []
merged: true
base: main
head: pip-tree-no-dedupe
created_at: 2024-06-22T19:26:40Z
updated_at: 2024-06-24T16:54:55Z
url: https://github.com/astral-sh/uv/pull/4449
synced_at: 2026-01-10T13:48:28Z
```

# implement `--no-dedupe` for `uv pip tree`

---

_Pull request opened by @ChannyClaus on 2024-06-22 19:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Resolves https://github.com/astral-sh/uv/issues/4439 partially.

Implements for `uv pip tree`:
- `--no-dedupe` flag, similar to `cargo tree --no-dedupe` .
- denote dependency cycles with `(#)` and add a footnote if there's a cycle (using `(*)` would require keeping track of the cycle state, so opted to do this instead).
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
The existing tests pass + added a couple of tests to validate `--no-dedupe` behavior.
<!-- How was it tested? -->


---

_Comment by @zanieb on 2024-06-22 19:30_

I'm confused, why do we need a separate character for cycles?

---

_Comment by @ChannyClaus on 2024-06-22 20:06_

> I'm confused, why do we need a separate character for cycles?

~if there's a cycle and `--no-dedupe` flag isn't given, wouldn't it potentially use `(*)` for two different things?~ never mind, just realized this is the current behavior. my intention was to disambiguate these two to make it clearer but maybe it isn't a big deal.

the behavior can be minorly confusing for cyclic dependencies though, probably (note the different error messages for the same set of installed distributions depending on the presence of `--no-dedupe`):
```
chankang@chans-Air ~/repos/uv -  (pip-tree-no-dedupe)
$ cargo run -q pip tree --no-dedupe
uv-cyclic-dependencies-c v0.1.0
└── uv-cyclic-dependencies-a v0.1.0
    └── uv-cyclic-dependencies-b v0.1.0
        └── uv-cyclic-dependencies-a v0.1.0 (*)
(*) Dependency cycle
chankang@chans-Air ~/repos/uv -  (pip-tree-no-dedupe)
$ cargo run -q pip tree
uv-cyclic-dependencies-c v0.1.0
└── uv-cyclic-dependencies-a v0.1.0
    └── uv-cyclic-dependencies-b v0.1.0
        └── uv-cyclic-dependencies-a v0.1.0 (*)
(*) Package tree already displayed
```

---

_Comment by @zanieb on 2024-06-23 01:00_

That seems fine, I'd just say "Package tree is a cycle and cannot be shown" instead

---

_@zanieb approved on 2024-06-24 16:54_

Nice!

---

_Merged by @zanieb on 2024-06-24 16:54_

---

_Closed by @zanieb on 2024-06-24 16:54_

---
