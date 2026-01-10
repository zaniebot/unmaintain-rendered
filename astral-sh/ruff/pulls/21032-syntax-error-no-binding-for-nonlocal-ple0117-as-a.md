```yaml
number: 21032
title: "[syntax-error]: no binding for nonlocal  PLE0117 as a semantic syntax error"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
assignees: []
merged: true
base: main
head: PLE0117
created_at: 2025-10-22T17:10:45Z
updated_at: 2025-11-05T19:20:22Z
url: https://github.com/astral-sh/ruff/pull/21032
synced_at: 2026-01-10T16:53:55Z
```

# [syntax-error]: no binding for nonlocal  PLE0117 as a semantic syntax error

---

_Pull request opened by @11happy on 2025-10-22 17:10_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR ports PLE0117 as a semantic syntax error.

## Test Plan

<!-- How was it tested? -->
Tests previously written


---

_Marked ready for review by @11happy on 2025-11-02 03:15_

---

_Review requested from @carljm by @11happy on 2025-11-02 03:15_

---

_Review requested from @AlexWaygood by @11happy on 2025-11-02 03:15_

---

_Review requested from @sharkdp by @11happy on 2025-11-02 03:15_

---

_Review requested from @dcreager by @11happy on 2025-11-02 03:15_

---

_Review requested from @MichaReiser by @11happy on 2025-11-02 03:15_

---

_Review requested from @dhruvmanila by @11happy on 2025-11-02 03:15_

---

_Comment by @github-actions[bot] on 2025-11-02 03:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-02 03:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @11happy on 2025-11-02 04:02_

@ntBre its ready for review : )

---

_Comment by @github-actions[bot] on 2025-11-02 04:03_

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

_Review request for @dcreager removed by @MichaReiser on 2025-11-03 14:46_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-03 14:46_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-03 14:46_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-03 14:46_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2025-11-03 14:46_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1170 on 2025-11-03 20:07_

It looks like most of our other error messages start with a lowercase letter. I might just stick with the CPython error message here:

```pycon
>>> def f():
...     nonlocal x
...
  File "<python-input-0>", line 2
    nonlocal x
    ^^^^^^^^^^
SyntaxError: no binding for nonlocal 'x' found
```

```suggestion
                write!(f, "no binding for nonlocal `{name}` found")
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:229 on 2025-11-03 20:07_

I'd probably revert this change since it just moves the test down, right?

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:937 on 2025-11-03 20:08_

I think we could also revert this whitespace change.

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:649 on 2025-11-03 20:09_

What do you think about just making this function something like `has_nonlocal_binding` that returns a bool? It doesn't look like we're using the returned range anywhere, and it might be easier for ty to implement `has_nonlocal_binding` (or at least the stub version is simpler).

---

_@ntBre reviewed on 2025-11-03 20:11_

Thanks! This looks good overall, I just had a few small suggestions.

---

_Label `internal` added by @ntBre on 2025-11-03 20:12_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:229 on 2025-11-05 12:03_

done

---

_@11happy reviewed on 2025-11-05 12:03_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/statement.rs`:937 on 2025-11-05 12:03_

done

---

_@11happy reviewed on 2025-11-05 12:03_

---

_@11happy reviewed on 2025-11-05 12:05_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:649 on 2025-11-05 12:05_

Yes, we can change it bool as the range is not used anywhere rightnow. 


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2701 on 2025-11-05 17:46_

```suggestion
    // in `TypeInferenceBuilder::infer_nonlocal_statement`, so this just returns `true`.
```

---

_@AlexWaygood reviewed on 2025-11-05 17:46_

---

_@ntBre approved on 2025-11-05 17:46_

Thanks! I just pushed a couple of commits removing the unrelated snapshot removal and updating the helper method's docstring. I also added a comment to the ty version of `has_nonlocal_binding` since we already check for this syntax error in ty here:

https://github.com/astral-sh/ruff/blob/ca7ca34909962137ac634b5ab35b22fdc1efc189/crates/ty_python_semantic/src/types/infer/builder.rs#L5395-L5404

Thanks @AlexWaygood for pointing that out!

(Ideally we'd use the `SemanticSyntaxErrorKind` here too, but I didn't see a great way to do that immediately. We'd need another helper like `InferContext::report_diagnostic`)

---

_Merged by @ntBre on 2025-11-05 19:13_

---

_Closed by @ntBre on 2025-11-05 19:13_

---
