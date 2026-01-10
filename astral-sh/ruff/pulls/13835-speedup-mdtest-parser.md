```yaml
number: 13835
title: Speedup mdtest parser
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/perf-mdtest
created_at: 2024-10-20T21:18:37Z
updated_at: 2024-10-21T20:03:40Z
url: https://github.com/astral-sh/ruff/pull/13835
synced_at: 2026-01-10T20:59:37Z
```

# Speedup mdtest parser

---

_Pull request opened by @MichaReiser on 2024-10-20 21:18_

## Summary

A few small changes to speed up the mdtest parser:

* Use `memchr` to find possible header and codeblock positions over matching both regexes (fewer LazyLock lookups, skip over more text than line by line)
* Avoid use of `captures` for the header (because captures are "slow" and header parsing is easy)
* Avoid using unicode words for the markdown language

## Test Plan

I "benchmarked" the parser by commenting out the `run_test` invocation. 

Before: ±230ms
Now: ±170ms


---

_Review requested from @carljm by @MichaReiser on 2024-10-20 21:18_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-20 21:18_

---

_Label `red-knot` added by @MichaReiser on 2024-10-20 21:18_

---

_Comment by @github-actions[bot] on 2024-10-20 21:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2024-10-20 21:39_

This all looks fine to me but @carljm is obviously the expert on this code. I haven't benchmarked myself but I trust you when you say it's faster 😄

---

_@sharkdp reviewed on 2024-10-21 07:33_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:139 on 2024-10-21 07:33_

Minor: the doc-comment should be updated ("Matches an arbitrary amount of whitespace"…)

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:145 on 2024-10-21 07:36_

The new regex does not match empty code blocks such as
````markdown
```py path=a.py
```
````
in `mdtest/import/errors.md`, which is also why some tests are failing. The final newline was optional before: `\n?`.

---

_@sharkdp reviewed on 2024-10-21 07:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:145 on 2024-10-21 10:30_

It might be worth adding a specific unit test for this edge case in the `red_knot_test` crate, so that we still have tests that will fail even if the mdtest in `mdtest/import/errors.md` changes so it no longer has an empty code block

---

_@AlexWaygood reviewed on 2024-10-21 10:30_

---

_@MichaReiser reviewed on 2024-10-21 16:35_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:145 on 2024-10-21 16:35_

Agree, I'll follow up, once I have @carljm approval 

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:247 on 2024-10-21 17:45_

I don't think it's worth having a separate `scan` method with only one call site; it's quite short, let's just inline it here. That makes the behavior more parallel to the `HEADER_RE` case (the updating of `self.unparsed` is visible in both branches), which I think makes it easier to understand the logic.

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:139 on 2024-10-21 17:45_

Do we need the capture groups in this regex if they are not used?

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:489 on 2024-10-21 17:51_

```suggestion
            panic!("expected two files");
```

---

_@carljm approved on 2024-10-21 17:51_

Looks great, thank you!!

---

_@MichaReiser reviewed on 2024-10-21 19:22_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:139 on 2024-10-21 19:22_

Technically not but there should be no cost because we use `matches`. I do like them for documentation purposes. 

---

_Merged by @MichaReiser on 2024-10-21 19:49_

---

_Closed by @MichaReiser on 2024-10-21 19:49_

---

_Branch deleted on 2024-10-21 19:49_

---
