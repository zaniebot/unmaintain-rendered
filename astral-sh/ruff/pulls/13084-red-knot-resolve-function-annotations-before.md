```yaml
number: 13084
title: "[red-knot] Resolve function annotations before adding function symbol"
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-fn-type-same
created_at: 2024-08-23T19:10:31Z
updated_at: 2024-08-27T23:45:27Z
url: https://github.com/astral-sh/ruff/pull/13084
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Resolve function annotations before adding function symbol

---

_@dylwil3_

This PR has the `SemanticIndexBuilder` visit function definition annotations before adding the function symbol/name to the builder.

For example, the following snippet no longer causes a panic:

```python
def bool(x) -> bool:
    Return True
```

Note: This fix changes the ordering of the global symbol table.

Closes #13069

---

_Review requested from @carljm by @dylwil3 on 2024-08-23 19:10_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-08-23 19:10_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-23 19:10_

---

_Comment by @dylwil3 on 2024-08-23 19:12_

I also changed the order that the `TypeInferenceBuilder` resolves function definitions in the same way, but I'm not sure if that was relevant.

---

_Comment by @codspeed-hq[bot] on 2024-08-23 19:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:red-knot-fn-type-same)

### Merging #13084 will **not alter performance**

<sub>Comparing <code>dylwil3:red-knot-fn-type-same</code> (2c5b453) with <code>main</code> (d19fd1b)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-08-23 19:22_

I believe this means that we'd understand the return annotation as referring to `builtins.bool` rather than referring to the first-party function `bool`. That mimics the evaluation order at runtime:

```pycon
>>> def bool() -> bool:
...   ...
...   
>>> bool.__annotations__
{'return': <class 'bool'>}
>>> bool.__annotations__['return'] is bool
False
```

And it also seems to be what pyright does: https://pyright-play.net/?code=CYUwZgBARg9jA2AKAlBAtAPmneAuAUBEQHSn74BOIAbiAIbwD6ALgJ4AOIisCylN9Jm07ccKPvgAeEALwQAKhQCuIKbOwIU-WgxYcuk5EA

But interestingly, mypy just understands the return annotation as being a forward reference to the function that the return annotation is part of: https://mypy-play.net/?mypy=latest&python=3.12&gist=6dd88ac3463dc662ab1a50ed0456685c

---

_Comment by @github-actions[bot] on 2024-08-23 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2024-08-23 19:43_

>I believe this means that we'd understand the return annotation as referring to `builtins.bool` rather than referring to the first-party function `bool`. That mimics the evaluation order at runtime

That feels right to me - it was the same experiment as yours that motivated the reordering.

P.S. Not sure what the deal is with CodSpeed? I don't think I touched anything linter-related... I hope!

---

_Comment by @AlexWaygood on 2024-08-23 20:14_

Nah, codspeed has been very unreliable recently :(

Best not to worry about it — sorry for the bother!

---

_Comment by @MichaReiser on 2024-08-23 20:24_

Wow nice, thank you. Could we add a test where the function and the return type name aren't a built in (e.g x as in the linked issue). Just to make sure the fix doesn't rely on any built in behavior 

---

_Comment by @dylwil3 on 2024-08-23 20:39_

> Wow nice, thank you. Could we add a test where the function and the return type name aren't a built in (e.g x as in the linked issue). Just to make sure the fix doesn't rely on any built in behavior

Done!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:423 on 2024-08-23 22:34_

While we're fixing the order of things here, I think this also belongs above the definition of the function name:

```
>>> def bool(x = bool):
...     return x
...
>>> bool()
<class 'bool'>
```

Would you be open to also adding a test for this and fixing the ordering?

---

_Review comment by @carljm on `crates/red_knot_workspace/resources/test/corpus/25_func_annotations_same_name.py`:11 on 2024-08-23 22:49_

In an ideal world, I'd prefer to have inference tests that validate we get the semantics right, rather than just corpus tests that validate we don't panic.

But at the moment it'd be a pain to write those inference tests; we don't have `Call` support yet, so you can't easily get access to the return type of a function by e.g. doing `y = x()` and then checking the type of the `y` symbol, you'd have to instead scrabble through the AST to find the return annotation expression node, and then assert on its type. So this is more just a note for future, not something that needs to be changed in this PR.

---

_@carljm approved on 2024-08-23 22:50_

This looks like a clear improvement as-is, though if you're willing to also fix up the default-value issue as well, that'd be great!

Thank you!!

---

_@dylwil3 reviewed on 2024-08-24 01:13_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:423 on 2024-08-24 01:13_

Done! Added another "no panic" test and then a mostly copy-pasted unit test that doesn't really check for much but is a baby step towards the sort of thing you mentioned in your other comment.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:620 on 2024-08-24 01:52_

Does this test fail before this PR? Just looking at the test, it doesn't seem to me that this test would be impacted by the bug we are fixing in this PR.

The test I was thinking of would go in `types/infer.rs`, not here, and would assert on the inferred type for the return (or default) expression. The test `local_inference` in that file is an example of an existing test that finds an AST node and then asserts on its type. But like I said, I wasn't necessarily expecting we'd add such a test in this PR.

Unless I'm missing something and this test does actually check for this bug, I would say let's remove this test. If you want to try your hand at adding the above style of test, that's great but not required!

---

_@carljm reviewed on 2024-08-24 01:52_

---

_@carljm reviewed on 2024-08-24 02:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:423 on 2024-08-24 02:04_

Thank you! As I mentioned in my other comment, I think the new copy-pasted test doesn't add enough value to be worth adding right now, so we can remove it. The inference test we eventually want would be at the type-inference level, not the semantic level. Everything else looks great!

---

_Comment by @carljm on 2024-08-24 02:31_

Awesome work, thank you!!

---

_Merged by @carljm on 2024-08-24 02:31_

---

_Closed by @carljm on 2024-08-24 02:31_

---

_Branch deleted on 2024-08-24 03:23_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:45_

---
