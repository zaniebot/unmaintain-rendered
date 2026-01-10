```yaml
number: 17980
title: "[ty] Change range of `revealed-type` diagnostic to be the range of the argument passed in, not the whole call"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/reveal-type-range
created_at: 2025-05-09T13:04:48Z
updated_at: 2025-05-09T13:15:40Z
url: https://github.com/astral-sh/ruff/pull/17980
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Change range of `revealed-type` diagnostic to be the range of the argument passed in, not the whole call

---

_Pull request opened by @AlexWaygood on 2025-05-09 13:04_

## Summary

I think we've discussed a few times that it would be desirable for the highlighted range of a `reveal_type` call to be this:

```
reveal_type(x)
            ^ Revealed type is `int`
```

Rather than this:

```
reveal_type(x)
^^^^^^^^^^^^^^ Revealed type is `int`
```

This PR implements that change

## Test Plan

Lots of snapshot changes


---

_Label `ty` added by @AlexWaygood on 2025-05-09 13:04_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-09 13:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-09 13:04_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-09 13:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-09 13:04_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-09 13:04_

---

_Comment by @github-actions[bot] on 2025-05-09 13:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- info[revealed-type] tests/dataclass_transform_example.py:13:1: Revealed type: `(a: str, b: int) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:13:13: Revealed type: `(a: str, b: int) -> None`
- info[revealed-type] tests/dataclass_transform_example.py:21:1: Revealed type: `(with_converter: int = Unknown) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:21:13: Revealed type: `(with_converter: int = Unknown) -> None`
- info[revealed-type] tests/dataclass_transform_example.py:34:1: Revealed type: `str`
+ info[revealed-type] tests/dataclass_transform_example.py:34:13: Revealed type: `str`
- info[revealed-type] tests/dataclass_transform_example.py:45:1: Revealed type: `str`
+ info[revealed-type] tests/dataclass_transform_example.py:45:13: Revealed type: `str`
- info[revealed-type] tests/dataclass_transform_example.py:56:1: Revealed type: `(_a: int = Any) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:56:13: Revealed type: `(_a: int = Any) -> None`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- info[revealed-type] tests/annotations/declarations.py:121:5: Revealed type: `Unknown`
+ info[revealed-type] tests/annotations/declarations.py:122:9: Revealed type: `Unknown`
- info[revealed-type] tests/annotations/declarations.py:232:5: Revealed type: `Unknown`
+ info[revealed-type] tests/annotations/declarations.py:233:9: Revealed type: `Unknown`

```
</details>


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:4786 on 2025-05-09 13:09_

Is it guaranteed that we always have an argument here?

---

_@AlexWaygood reviewed on 2025-05-09 13:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:1 on 2025-05-09 13:09_

I had to fiddle about with the assertions here a bit because with this PR, the `undefined-reveal` diagnostic can now be attached to a different line than the `revealed-type` diagnostic. The mechanism mdtest uses to suppress `undefined-reveal` diagnostics is that it sees a `# revealed:` assertion as matching both diagnostics simultaneously, but it only recognises a match if the `revealed:` assertion is on the same line as the `undefined-reveal` diagnostic.

Ideally we'd get rid of this limitation in mdtest. (I still think a simpler solution would just be to deselect the `undefined-reveal` lint altogether in mdtest, and have that specific rule tested solely through integration tests -- this would also solve the issue that `undefined-reveal` diagnostics tend to be very noisy in snapshots.) However, it didn't seem worth spending much time on right now. So for now I just fiddled with the Markdown in this file to make sure that the `undefined-reveal` diagnostics are always attached to the same line as the `revealed-type` diagnostics. It leads to slightly odd Python formatting in some cases, but it means that mdtest continues to recognise the `# revealed:` assertions as matching the `undefined-reveal` diagnostics as well as the `revealed-type` diagnostics.

---

_@MichaReiser approved on 2025-05-09 13:09_

Nice, thank you

---

_@AlexWaygood reviewed on 2025-05-09 13:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:4786 on 2025-05-09 13:11_

Yes, because otherwise we wouldn't have entered the `if let [Some(revealed_type)] = overload.parameter_types()` branch a couple of lines above. I can add a comment if you like?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:4786 on 2025-05-09 13:14_

Nah, it's fine. Just wanted to make sure it's the case and it wasn't immediately obvious from skimming over the code

---

_@MichaReiser reviewed on 2025-05-09 13:14_

---

_Merged by @AlexWaygood on 2025-05-09 13:15_

---

_Closed by @AlexWaygood on 2025-05-09 13:15_

---

_Branch deleted on 2025-05-09 13:15_

---
