```yaml
number: 21911
title: "[ty] Avoid inferring types for invalid binary expressions in string annotations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/en
created_at: 2025-12-11T00:55:44Z
updated_at: 2025-12-11T11:56:26Z
url: https://github.com/astral-sh/ruff/pull/21911
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Avoid inferring types for invalid binary expressions in string annotations

---

_Pull request opened by @charliermarsh on 2025-12-11 00:55_

## Summary

Closes https://github.com/astral-sh/ty/issues/1847.


---

_Marked ready for review by @charliermarsh on 2025-12-11 00:55_

---

_Review requested from @carljm by @charliermarsh on 2025-12-11 00:55_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-11 00:55_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-11 00:55_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-11 00:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-11 00:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 41 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-11 02:38_

---

_Renamed from "Avoid inferring types for invalid binary expressions in string annotations" to "[ty] Avoid inferring types for invalid binary expressions in string annotations" by @AlexWaygood on 2025-12-11 02:38_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 07:57_

We used to have the requirement that inference had to run for every expression in the AST, or otherwise we might panic when requesting a type for that expression. We have changed that and now fall back to `Unknown` in case no type has been inferred.

Since we're not going to infer useful types here anyway, I think we can also just remove this entirely.

---

_@sharkdp approved on 2025-12-11 08:15_

---

_Merged by @sharkdp on 2025-12-11 08:40_

---

_Closed by @sharkdp on 2025-12-11 08:40_

---

_Branch deleted on 2025-12-11 08:40_

---

_@AlexWaygood reviewed on 2025-12-11 10:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 10:34_

That leads to a worse experience for server users when they hover over sub-expressions in the invalid type expression, no? I think we should still aim to store a type for each sub-expression wherever possible 

---

_@sharkdp reviewed on 2025-12-11 10:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 10:53_

> That leads to a worse experience for server users when they hover over sub-expressions in the invalid type expression, no?

I mean... maybe? But the whole thing will be red-squiggly underlined anyway. It's completely wrong. Why is it helpful that they see `Literal[4]` if they hover over the `4` in `x: "3+4"`? Is `Literal[4]` even more correct than `Unknown` here?

---

_@AlexWaygood reviewed on 2025-12-11 11:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 11:04_

I don't think that's really a realistic example of an invalid type expression that someone is likely to use. If somebody is using the appeal library, for example, they might be making use of that library's [special DSL](https://github.com/larryhastings/appeal?tab=readme-ov-file#annotations-and-introspection) for annotations (which is incompatible with type checkers, and so is going to result in lots of `invalid-type-form` violations). Who knows if this user even has `invalid-type-form` enabled in their ty config; maybe they're only interested in our server functionality. I don't see why the invalid type expression should mean that we don't offer any on-hover information or go-to-definition support for any symbols in this user's annotations.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 11:07_

To be clear I'm specifically objecting to https://github.com/astral-sh/ruff/pull/21911/commits/f2e50c4f83045fe6f5c2d433260ba97a66df6370. I'm fine with not attempting to infer the sub-expressions of invalid stringized annotations if it makes us crash. That seems like a pretty rare edge case. It's the extension of that to "all invalid binary expressions in type expressions" that I'm uncomfortable with.

I just feel that as a general principle, we should continue to attempt to infer types for all sub-expressions wherever possible (on a best-effort basis)

---

_@AlexWaygood reviewed on 2025-12-11 11:07_

---

_@sharkdp reviewed on 2025-12-11 11:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:163 on 2025-12-11 11:56_

I'm still not convinced that it's better to run `infer_binary_expression` here just for the sake of having "some" type (because we're very explicitly in a type expression, and not in a value expression here).

But I certainly don't feel as strongly as you do, so let's just revert: https://github.com/astral-sh/ruff/pull/21914

---
