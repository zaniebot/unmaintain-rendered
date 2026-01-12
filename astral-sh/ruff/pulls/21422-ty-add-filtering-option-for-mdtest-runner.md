```yaml
number: 21422
title: "[ty] Add filtering option for mdtest runner"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-runner-filters
created_at: 2025-11-13T09:59:34Z
updated_at: 2025-11-13T12:28:14Z
url: https://github.com/astral-sh/ruff/pull/21422
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Add filtering option for mdtest runner

---

_@sharkdp_

## Summary

This change to the mdtest runner makes it easy to run on a subset of tests/files. For example:

```
▶ uv run crates/ty_python_semantic/mdtest.py implicit
running 1 test
test mdtest__implicit_type_aliases ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 281 filtered out; finished in 0.83s

Ready to watch for changes...
```

Subsequent changes to either that test file or the Rust source code will also only rerun the `implicit_type_aliases` test.

Multiple arguments can be provided, and filters can either be partial file paths (`loops/for.md`, `loops/for`, `for`) or mangled test names (`loops_for`):
```
▶ uv run crates/ty_python_semantic/mdtest.py implicit binary/union

running 2 tests
test mdtest__binary_unions ... ok
test mdtest__implicit_type_aliases ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 280 filtered out; finished in 0.85s

Ready to watch for changes...
```

## Test Plan

Tested it interactively for a while

---

_Review requested from @carljm by @sharkdp on 2025-11-13 09:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-13 09:59_

---

_Review requested from @dcreager by @sharkdp on 2025-11-13 09:59_

---

_Label `testing` added by @sharkdp on 2025-11-13 09:59_

---

_Label `ty` added by @sharkdp on 2025-11-13 09:59_

---

_@sharkdp reviewed on 2025-11-13 10:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/mdtest.py`:246 on 2025-11-13 10:01_

I guess one could even argue that the filter shouldn't apply here? That is, if a MD file is changed that doesn't match the filter, maybe it makes sense to still run it for that file.

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 10:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 10:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-11-13 10:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fmdtest-runner-filters?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21422 will **not alter performance**

<sub>Comparing <code>david/mdtest-runner-filters</code> (fcd219a) with <code>main</code> (cd183c5)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Fmdtest-runner-filters?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:246 on 2025-11-13 11:00_

yeah, I think it makes sense to run any mdtest that changed, even if a filter was provided?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:177 on 2025-11-13 11:01_

isn't the type annotation still accurate? :P

---

_@AlexWaygood approved on 2025-11-13 11:05_

Nice!

In case I can nerd-snipe you -- the feature I'd most like added to mdtest.py would be a way to request it to re-run our whole mdtest suite (e.g. by entering `rerun` to stdin?) without me having to terminate the script and re-run it (which usually involves a "useless" recompilation of the test binary)

---

_@sharkdp reviewed on 2025-11-13 12:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/mdtest.py`:177 on 2025-11-13 12:06_

I think it was never accurate. Both ty and pyright complained. There's no way for a type checker to see that the loop runs forever.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:177 on 2025-11-13 12:09_

Oh I see, because it's a `for` loop rather than a `while` loop? Makes sense.

---

_@AlexWaygood reviewed on 2025-11-13 12:09_

---

_@AlexWaygood reviewed on 2025-11-13 12:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:177 on 2025-11-13 12:10_

You could add `assert False, "Should never get here"` at the bottom of the function to shut them up, I guess :P

---

_Merged by @sharkdp on 2025-11-13 12:20_

---

_Closed by @sharkdp on 2025-11-13 12:20_

---

_Branch deleted on 2025-11-13 12:20_

---
