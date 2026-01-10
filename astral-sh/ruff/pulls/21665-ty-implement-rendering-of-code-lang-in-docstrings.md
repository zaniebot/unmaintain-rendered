```yaml
number: 21665
title: "[ty] implement rendering of `.. code:: lang` in docstrings"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/rst-more
created_at: 2025-11-27T16:52:09Z
updated_at: 2025-11-28T13:27:53Z
url: https://github.com/astral-sh/ruff/pull/21665
synced_at: 2026-01-10T16:48:02Z
```

# [ty] implement rendering of `.. code:: lang` in docstrings

---

_Pull request opened by @Gankra on 2025-11-27 16:52_

## Summary

* Fixes https://github.com/astral-sh/ty/issues/1650
* Part of https://github.com/astral-sh/ty/issues/1610

We now handle:

* `.. warning::` (and friends) by bolding the line and rendering the block as normal (non-code) text
* `.. code::` (and friends) by treating it the same as `::` (fully deleted if seen, introduce a code block)
* `.. code:: lang` (and friends) by letting it set the language on the codefence
* `.. versionchanged:: 1.2.3` (and friends) by rendering it like `warning` but with the version included and italicized
* `.. dsfsdf-unknown:: (lang)` by assuming it's the same as `.. code:: (lang)`

## Test Plan

Snapshots added/updated. I also deleted a bunch of useless checks on plaintext rendering. It's important for some edge-case tests but not for the vast majority of tests.


---

_Label `server` added by @Gankra on 2025-11-27 16:52_

---

_Review requested from @carljm by @Gankra on 2025-11-27 16:52_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-27 16:52_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-27 16:52_

---

_Label `ty` added by @Gankra on 2025-11-27 16:52_

---

_Review requested from @sharkdp by @Gankra on 2025-11-27 16:52_

---

_Review requested from @dcreager by @Gankra on 2025-11-27 16:52_

---

_Renamed from "implement handling of `.. code:: lang` in docstrings" to "[ty] implement rendering of `.. code:: lang` in docstrings" by @Gankra on 2025-11-27 16:52_

---

_Comment by @Gankra on 2025-11-27 16:53_

My awful son only gets worse

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 16:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @MichaReiser on 2025-11-27 16:53_

Potentially some more sphinx specific candidates https://github.com/astral-sh/ruff/issues/21640 

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 16:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 42 diagnostics
+ Found 41 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-27 17:05_

`.. versionadded::`, `.. versionchanged::` and `.. deprecated::` all appear a huge amount in CPython's online documentation (see https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#describing-changes-between-versions). They might also pop up in CPython docstrings as well from time to time -- not sure? CPython's docstrings aren't really meant to be ReST because they're intended to be viewed directly in a terminal, but sometimes ReST creeps in.

---

_Comment by @Gankra on 2025-11-27 17:44_

<img width="433" height="279" alt="Screenshot 2025-11-27 at 12 39 17 PM" src="https://github.com/user-attachments/assets/545648da-f1b2-45e3-8672-1f0c63de939b" />


---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:290 on 2025-11-28 08:22_

The comment suggests that it applies to the clippy allow, when it doesn't. 

I suggest using a different name for `_line`. I assumed the variable would be unused, when, in fact, it's a tricky work around lifetimes. Maybe `tmp_line` or `owned_line` or make line a `Cow`?

If you really want to use `_line`, use `expect`

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:235 on 2025-11-28 08:27_

Doesn't markdown allow you to use an arbitrary number of backticks?

> A code fence is a sequence of at least three consecutive backtick characters (`) or tildes (~). (Tildes and backticks cannot be mixed.) A fenced code block begins with a code fence, preceded by up to three spaces of indentation.


So, we could just use a silly amount of backticks :) https://spec.commonmark.org/0.31.2/#fenced-code-blocks

---

_Review comment by @MichaReiser on `crates/ty_ide/src/docstring.rs`:955 on 2025-11-28 08:30_

Why do we preseve the colon here but not in `literal_own_line`

---

_@MichaReiser approved on 2025-11-28 08:34_

---

_@Gankra reviewed on 2025-11-28 13:04_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:290 on 2025-11-28 13:04_

realtalk this was me getting Extremely flustered at the `_special_case_temp_owned_thing` idiom being flagged by clippy after 10 years of me using it. I have abandoned the underscore :(

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:955 on 2025-11-28 13:08_

https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#literal-blocks

> The handling of the :: marker is smart:
> * If it occurs as a paragraph of its own, that paragraph is completely left out of the document.
> * If it is preceded by whitespace, the marker is removed.
> * If it is preceded by non-whitespace, the marker is replaced by a single colon.

When translating to markdown this amounts to "check if there's a space immediately before the `::`, if there is, delete it. Otherwise, turn it into `:`".

---

_@Gankra reviewed on 2025-11-28 13:08_

---

_@Gankra reviewed on 2025-11-28 13:18_

---

_Review comment by @Gankra on `crates/ty_ide/src/docstring.rs`:235 on 2025-11-28 13:18_

This is genius.

---

_Merged by @Gankra on 2025-11-28 13:27_

---

_Closed by @Gankra on 2025-11-28 13:27_

---

_Branch deleted on 2025-11-28 13:27_

---
