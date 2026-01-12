```yaml
number: 16957
title: "[syntax-errors] Multiple assignments in `case` pattern"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/multiple-assignment-case-pattern
created_at: 2025-03-24T16:43:25Z
updated_at: 2025-04-04T14:14:31Z
url: https://github.com/astral-sh/ruff/pull/16957
synced_at: 2026-01-12T15:55:59Z
```

# [syntax-errors] Multiple assignments in `case` pattern

---

_@ntBre_

Summary
--

This PR detects multiple assignments to the same name in `case` patterns by recursively visiting each pattern.

Test Plan
--

New inline tests.


---

_Label `rule` added by @ntBre on 2025-03-24 16:43_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-24 16:43_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-24 16:43_

---

_Comment by @github-actions[bot] on 2025-03-24 16:52_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:338 on 2025-03-24 17:04_

It seems that we only need the range comparison here because we visit the `PatternMatchAs` fields not in the source order. That took me a while to realise. I think I'd prefer to, instead, change `visit_pattern` so that it visits all fields in source order, so that we can remove the range handling here.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:332 on 2025-03-24 17:06_

This is `O(n^2)`. Should we change the visitor to first collect all names, then sort by name (`log (n)`), then identify duplicate names by comparing neighboring elements (`n`)? 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@multiple_assignment_in_case_pattern.py.snap`:683 on 2025-03-24 17:08_

Do you think we should mention the variable name here? It wasn't clear to me from the error message what's wrong in this example:  Can I only have a single variable? It might be especially hard to realize what's wrong if the use of the same identifier isn't next to each other (`{ 1: x, 2: aaaa_very_long_key, ..., 4: x }`)

 

---

_@MichaReiser approved on 2025-03-24 17:10_

This looks good to me. I've a few smaller suggestions

---

_@ntBre reviewed on 2025-03-24 17:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:338 on 2025-03-24 17:56_

Ah yes, I thought I had tried that first and got surprised when they didn't come out in source order, but I think I just had the order wrong in the `MatchAs` case.

---

_@ntBre reviewed on 2025-03-24 18:23_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:332 on 2025-03-24 18:23_

That's closer to what I had initially, but I ran into some issues with `MatchOr`, where the patterns needed to be visited independently. I found a way to work around it and think this is better, thanks!

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@multiple_assignment_in_case_pattern.py.snap`:683 on 2025-03-24 18:26_

That's a good point. I was trying to avoid putting data into the `SemanticSyntaxErrorKind` variants, but we can give better error messages if we do. I'll try something more like Python:

```pycon
>>> match 2:
...     case x as x: ...
...
  File "<python-input-4>", line 2
    case x as x: ...
         ^^^^^^
SyntaxError: multiple assignments to name 'x' in pattern
>>> match 2:
...     case [x, y, *x]: ...
...
  File "<python-input-5>", line 2
    case [x, y, *x]: ...
                ^^
SyntaxError: multiple assignments to name 'x' in pattern
```

---

_@ntBre reviewed on 2025-03-24 18:26_

---

_Label `preview` added by @ntBre on 2025-03-24 18:42_

---

_@dhruvmanila reviewed on 2025-03-24 21:24_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:399 on 2025-03-24 21:24_

I think it would be useful to provide the reason for why should this be done. From what I understand, I think it's because they're separate patterns and checking for duplicate names are isolated to a single pattern.

---

_@dhruvmanila approved on 2025-03-24 21:28_

---

_@ntBre reviewed on 2025-03-25 17:56_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:399 on 2025-03-25 17:56_

I expanded the comment and also moved the relevant `test_ok` case down.

I also switched to the hash set implementation we discussed yesterday and cleaned up some of the docs and code locations.

---

_Merged by @ntBre on 2025-03-26 17:02_

---

_Closed by @ntBre on 2025-03-26 17:02_

---

_Branch deleted on 2025-03-26 17:02_

---

_Comment by @sammorley-short on 2025-04-04 14:10_

@ntBre I am seeing an linting error thrown for `case Class(x=x)`. This hasn't been an issue for us in our code (i.e. it compiles and runs just fine), but it's not clear from the python docs whether this should or shouldn't be allowed.

Any idea what should be happening here?

---

_Comment by @ntBre on 2025-04-04 14:14_

@sammorley-short Yeah that was a bug in this PR, first reported in #17181. It's been fixed but not released yet. 0.11.4 should come out with the fix today. Thanks for the report!

---
