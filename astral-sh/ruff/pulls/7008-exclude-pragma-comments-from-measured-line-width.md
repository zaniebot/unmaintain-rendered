```yaml
number: 7008
title: Exclude pragma comments from measured line width
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: exclude-pragmas
created_at: 2023-08-30T12:02:06Z
updated_at: 2023-09-01T16:32:32Z
url: https://github.com/astral-sh/ruff/pull/7008
synced_at: 2026-01-12T02:45:38Z
```

# Exclude pragma comments from measured line width

---

_Pull request opened by @cnpryer on 2023-08-30 12:02_

Closes #6772

## Summary

~~If the comment doesn't include a NBSP and starts with `noqa`, `type:`, `pyright:`, or `pylint:` then we assign it `0` reserved width for its line suffix. Otherwise measure width.~~

See https://github.com/astral-sh/ruff/issues/6772#issuecomment-1699899884

This PR aligns with `ruff` linting behavior in that it will disregard the *kind* of white-space in order to identify pragmas to exclude from measured line width.

## Test Plan

Current fixtures and new trailing comments fixtures.

---

_Marked ready for review by @cnpryer on 2023-08-30 22:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:364 on 2023-08-31 06:37_

```suggestion
        let reserved_width = if ["noqa:", "type:", "pyright:", "pylint:"]
```

This logic also iterates over every string 4 times. We can do slightly better by using:

```rust
let is_pragma = trimmed.split_once(':').is_some_and(|(maybe_pragma, _)| {
    matches!(maybe_pragma, "noqa" | "type" | "pyright" | "pylint")
});
```

and rely on rust to generate an efficient string matching (e.g. skip if the pragma part is only 2 characters long)

---

_Label `formatter` added by @MichaReiser on 2023-08-31 06:38_

---

_@MichaReiser approved on 2023-08-31 07:00_

Looking good. I've one small nit and would like to ask you to add a few pragma comment-specific tests that test the behavior outside of black's test suite (you may want to include some of your interesting NBSP cases too!)

**This PR**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76079 |              1789 |              1634 |
| django       |           0.99948 |              2760 |                85 |
| transformers |           0.99925 |              2587 |               495 |
| **twine**        |           0.99982 |                33 |                 2 |
| **typeshed**     |           0.99978 |              3496 |              2172 |
| **warehouse**    |           0.99661 |               648 |                39 |
| **zulip**        |           0.99942 |              1437 |                42 |

**Base**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76079 |              1789 |              1635 |
| django       |           0.99948 |              2760 |                85 |
| transformers |           0.99925 |              2587 |               495 |
|twine        |           0.99965 |                33 |                 3 | <== improved
| typeshed     |           0.99947 |              3496 |              2173 | <== improved
| warehouse    |           0.99659 |               648 |                41 | <== improved
| zulip        |           0.99938 |              1437 |                47 | <== improved

Funny how typeshed is close to a similarity index of 1 but every other file gets reformatted :see_no_evil: 

---

_@cnpryer reviewed on 2023-08-31 10:18_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:364 on 2023-08-31 10:18_

😍 

I also put them in that order intentionally lol. Used download stats for pyright and pylint :).

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:364 on 2023-08-31 12:58_

To make sure I'm clear, your solution works since we're intentionally disregarding the *bare* `# noqa` lints `ruff` no longer expects to support?

```
warning: Invalid `# noqa` directive on scratch.py:16: expected `:` followed by a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

---

_@cnpryer reviewed on 2023-08-31 12:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:364 on 2023-08-31 13:04_

Hmm good point, we may need the `starts_width` in the case the colon is missing to cover ruff's `noqa` :(

---

_@MichaReiser reviewed on 2023-08-31 13:04_

---

_Comment by @cnpryer on 2023-08-31 13:19_

NBSP is such a niche thing to focus on, so I'll move on soon, but while adding these fixtures I noticed the following isn't aligned between the formatter and `black`:

```py
# Input: \u{a0}\u{a0}type
i = ""  #  type: Add space before two leading NBSP

# Ruff: \u{a0}type
i = ""  #  type: Add space before two leading NBSP

# Black: \u{a0}\u{a0}type
i = ""  #   type: Add space before two leading NBSP
```

Should be simple, so I'll submit it in a follow up PR and correct any snapshots.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/trailing_comments.py`:25 on 2023-08-31 13:26_

Note I'll address this in a follow up PR. See https://github.com/astral-sh/ruff/pull/7008#issuecomment-1701026971

---

_@cnpryer reviewed on 2023-08-31 13:26_

---

_@cnpryer reviewed on 2023-08-31 13:27_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/trailing_comments.py`:29 on 2023-08-31 13:27_

Same comment about aligning with black for NBSP edge cases

```py
# Input: #\u{a0}\u{a0}noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  #  noqa

# Black: # \u{a0}noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  #  noqa

# Ruff: # noqa
i = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"  # noqa
```

---

_Comment by @cnpryer on 2023-08-31 13:31_

I'm going to mark as a draft and do another review of this during lunch today (~3 hours from now; would review my fixtures and the resolved merge conflict again).

If you decide to merge anyway I'm cool with that too.

---

_Converted to draft by @cnpryer on 2023-08-31 13:31_

---

_Comment by @codspeed-hq[bot] on 2023-08-31 13:38_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:exclude-pragmas)

### Merging #7008 will **degrade performances by 1.52%**

<sub>Comparing <code>cnpryer:exclude-pragmas</code> (a8c1f55) with <code>main</code> (f4ba0ea)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cnpryer:exclude-pragmas)._

### Benchmarks breakdown

|     | Benchmark | `main` | `cnpryer:exclude-pragmas` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 71.8 ms | 72.9 ms | -1.52% |


---

_Marked ready for review by @cnpryer on 2023-08-31 16:45_

---

_@cnpryer reviewed on 2023-08-31 16:48_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/comments/format.rs`:364 on 2023-08-31 16:48_

fccdcfe2550c3fdb40bc73bb3e974ae3d1b8958f

---

_Renamed from "Exclude pragma comments from measured width" to "Exclude pragma comments from measured line width" by @cnpryer on 2023-08-31 23:15_

---

_@MichaReiser approved on 2023-09-01 06:20_

---

_Merged by @MichaReiser on 2023-09-01 06:34_

---

_Closed by @MichaReiser on 2023-09-01 06:34_

---

_Branch deleted on 2023-09-01 16:32_

---
