```yaml
number: 21657
title: "[syntax-error] Default type parameter followed by non-default type parameter"
type: pull_request
state: merged
author: 11happy
labels:
  - parser
assignees: []
merged: true
base: main
head: type_parameter_default_order
created_at: 2025-11-27T09:40:42Z
updated_at: 2025-12-03T06:31:31Z
url: https://github.com/astral-sh/ruff/pull/21657
synced_at: 2026-01-10T16:48:02Z
```

# [syntax-error] Default type parameter followed by non-default type parameter

---

_Pull request opened by @11happy on 2025-11-27 09:40_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements syntax error where a default type parameter is followed by a non-default type parameter. https://github.com/astral-sh/ruff/issues/17412#issuecomment-3584088217


## Test Plan

<!-- How was it tested? -->
I have written inline tests as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-11-27 09:40_

---

_Review requested from @dhruvmanila by @11happy on 2025-11-27 09:40_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 09:58_


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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-11-27 11:12_

This check needs to be applied to type aliases as well:

```python
In [1]: type Int[T = int, U] = int
  Cell In[1], line 1
    type Int[T = int, U] = int
                      ^
SyntaxError: non-default type parameter 'U' follows default type parameter
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:635 on 2025-11-27 11:13_

Let's add some cases where the default is not at the start of the list and it has multiple non-default type parameter following a type parameter with default like:
```py
class C[T1, T2 = int, T3, T4]: ...
```

---

_@dhruvmanila requested changes on 2025-11-27 11:13_

---

_Label `parser` added by @dhruvmanila on 2025-11-27 11:13_

---

_@11happy reviewed on 2025-11-30 07:58_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-11-30 07:58_

done

---

_@11happy reviewed on 2025-11-30 07:59_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:635 on 2025-11-30 07:59_

done

---

_@11happy reviewed on 2025-11-30 08:03_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-11-30 08:03_

however some additional detection's are there for example -
```
type X[*Ts = x := int] = int
```
in type params:

type_params: TypeParams { range: 6..22, node_index: NodeIndex(None), type_params: [TypeVarTuple(TypeParamTypeVarTuple { node_index: NodeIndex(None), range: 7..14, name: Identifier { id: Name("Ts"), range: 8..10, node_index: NodeIndex(None) }, default: Some(Name(ExprName { node_index: NodeIndex(None), range: 13..14, id: Name("x"), ctx: Load })) }), TypeVar(TypeParamTypeVar { node_index: NodeIndex(None), range: 18..21, name: Identifier { id: Name("int"), range: 18..21, node_index: NodeIndex(None) }, bound: None, default: None })] 

int is getting detected as seperate non default typevar, whereas I think should it be parsed as single expression ?


---

_Review requested from @dhruvmanila by @11happy on 2025-12-02 07:37_

---

_@dhruvmanila reviewed on 2025-12-03 05:22_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-12-03 05:22_

> int is getting detected as seperate non default typevar, whereas I think should it be parsed as single expression ?

That's unrelated to this PR as it's more about how the parser is recovering from an invalid syntax.

---

_@dhruvmanila approved on 2025-12-03 05:22_

Thanks

---

_Renamed from "[syntax-error]: default type parameter followed by non-default type parameter" to "[syntax-error] Default type parameter followed by non-default type parameter" by @dhruvmanila on 2025-12-03 05:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-12-03 05:27_

Oh I see what you mean, it's related to the failing tests. I think it's fine for now as it's how the parser is recovering from an invalid syntax. Can you update the snapshots? I can merge then.

---

_@dhruvmanila reviewed on 2025-12-03 05:27_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:155 on 2025-12-03 05:59_

done I have updated snapshots

---

_@11happy reviewed on 2025-12-03 05:59_

---

_Merged by @dhruvmanila on 2025-12-03 06:31_

---

_Closed by @dhruvmanila on 2025-12-03 06:31_

---
