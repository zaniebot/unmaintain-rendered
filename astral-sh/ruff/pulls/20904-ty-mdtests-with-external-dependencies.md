```yaml
number: 20904
title: "[ty] mdtests with external dependencies"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-external-packages
created_at: 2025-10-15T19:46:47Z
updated_at: 2025-12-08T10:44:23Z
url: https://github.com/astral-sh/ruff/pull/20904
synced_at: 2026-01-12T15:57:12Z
```

# [ty] mdtests with external dependencies

---

_@sharkdp_

## Summary

This PR adds the possibility to write mdtests that specify external dependencies in a `project` section of TOML blocks. For example, here is a test that makes sure that we understand Pydantic's dataclass-transform setup:

````markdown
```toml
[environment]
python-version = "3.12"
python-platform = "linux"

[project]
dependencies = ["pydantic==2.12.2"]
```

```py
from pydantic import BaseModel

class User(BaseModel):
    id: int
    name: str

user = User(id=1, name="Alice")
reveal_type(user.id)  # revealed: int
reveal_type(user.name)  # revealed: str

# error: [missing-argument] "No argument provided for required parameter `name`"
invalid_user = User(id=2)
```
````

## How?

Using the `python-version` and the `dependencies` fields from the Markdown section, we generate a `pyproject.toml` file, write it to a temporary directory, and use `uv sync` to install the dependencies into a virtual environment. We then copy the Python source files from that venv's `site-packages` folder to a corresponding directory structure in the in-memory filesystem. Finally, we configure the search paths accordingly, and run the mdtest as usual.

I fully understand that there are valid concerns here:
* Doesn't this require network access? (yes, it does)
* Is this fast enough? (`uv` caching makes this almost unnoticeable, actually)
* Is this deterministic? ~~(probably not, package resolution can depend on the platform you're on)~~ (yes, hopefully)

For this reason, this first version is opt-in, locally. ~~We don't even run these tests in CI (even though they worked fine in a previous iteration of this PR).~~ You need to set `MDTEST_EXTERNAL=1`, or use the new `-e/--enable-external` command line option of the `mdtest.py` runner. For example:
```bash
# Skip mdtests with external dependencies (default):
uv run crates/ty_python_semantic/mdtest.py

# Run all mdtests, including those with external dependencies:
uv run crates/ty_python_semantic/mdtest.py -e

# Only run the `pydantic` tests. Use `-e` to make sure it is not skipped:
uv run crates/ty_python_semantic/mdtest.py -e pydantic
```

## Why?

I believe that this can be a useful addition to our testing strategy, which lies somewhere between ecosystem tests and normal mdtests. Ecosystem tests cover much more code, but they have the disadvantage that we only see second- or third-order effects via diagnostic diffs. If we unexpectedly gain or lose type coverage somewhere, we might not even notice (assuming the gradual guarantee holds, and ecosystem code is mostly correct). Another disadvantage of ecosystem checks is that they only test checked-in code that is usually correct. However, we also want to test what happens on wrong code, like the code that is momentarily written in an editor, before fixing it. On the other end of the spectrum we have normal mdtests, which have the disadvantage that they do not reflect the reality of complex real-world code. We experience this whenever we're surprised by an ecosystem report on a PR.

That said, these tests should not be seen as a replacement for either of these things. For example, we should still strive to write detailed self-contained mdtests for user-reported issues. But we might use this new layer for regression tests, or simply as a debugging tool. It can also serve as a tool to document our support for popular third-party libraries.

## Test Plan

* I've been locally using this for a couple of weeks now.
* `uv run crates/ty_python_semantic/mdtest.py -e`


---

_Label `testing` added by @sharkdp on 2025-10-15 19:46_

---

_Label `ty` added by @sharkdp on 2025-10-15 19:46_

---

_Comment by @github-actions[bot] on 2025-10-15 19:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-10-15 19:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5519 diagnostics
+ Found 5518 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/api/rest.py:1047:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2070 diagnostics
+ Found 2072 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "[ty] Allow usage of external packages in mdtests" to "[ty] mdtests with external dependencies" by @sharkdp on 2025-12-05 11:21_

---

_Marked ready for review by @sharkdp on 2025-12-05 12:39_

---

_Review requested from @carljm by @sharkdp on 2025-12-05 12:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-05 12:39_

---

_Review requested from @dcreager by @sharkdp on 2025-12-05 12:39_

---

_Review requested from @MichaReiser by @sharkdp on 2025-12-05 12:39_

---

_@sharkdp reviewed on 2025-12-05 12:41_

---

_Review comment by @sharkdp on `crates/ty_test/src/external_dependencies.rs`:90 on 2025-12-05 12:41_

I have not tested this, this part was written by Claude. Happy to remove it or leave it in, just in case it's correct.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/external/attrs.md`:5 on 2025-12-05 13:34_

We could combine it (and require) a `python-platform` and pass the platform to uv sync's `--python-platform`. This should give us predictable output and should be reproducible across platforms for as long as the dependencies build on all platforms (I think)

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:90 on 2025-12-05 13:36_

Could we reuse some of the logic from here https://github.com/astral-sh/ruff/blob/9cb5483e70166ec4f7b07bb45b692b9132442c1a/crates/ty_test/src/lib.rs#L291-L297

or maybe better, use ty's virtual environment detection https://github.com/astral-sh/ruff/blob/9cb5483e70166ec4f7b07bb45b692b9132442c1a/crates/ty_python_semantic/src/site_packages.rs#L394

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:132 on 2025-12-05 13:39_

It might be worth adding a method like this to `MemoryFileSystem`, given that we have the same/something similar in 

https://github.com/astral-sh/ruff/blob/9cb5483e70166ec4f7b07bb45b692b9132442c1a/crates/ruff_benchmark/src/real_world_projects.rs#L315-L374

---

_@MichaReiser approved on 2025-12-05 13:39_

This is great.

I think we should run the tests in CI or we'll keep regressing those tests. 

---

_@MichaReiser reviewed on 2025-12-05 15:19_

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:132 on 2025-12-05 15:19_

This would also be useful in `ty_wasm` if we want to support external dependencies in the playground (I think)

---

_@sharkdp reviewed on 2025-12-08 09:47_

---

_Review comment by @sharkdp on `crates/ty_test/src/external_dependencies.rs`:132 on 2025-12-08 09:47_

I spent an hour trying to unify these two use cases, but ultimately failed. Either it resulted in more code overall, or I ran into `sync` problems that I didn't understand :disappointed: 

---

_@sharkdp reviewed on 2025-12-08 09:48_

---

_Review comment by @sharkdp on `crates/ty_test/src/external_dependencies.rs`:90 on 2025-12-08 09:48_

Thank you, reusing `site_packages.rs` functionality makes this much cleaner now.

---

_Comment by @sharkdp on 2025-12-08 10:08_

> I think we should run the tests in CI or we'll keep regressing those tests.

I mainly didn't want to force this on anyone given that it's still relatively experimental. I was planning to enable it in CI later, but doing it now is fine with me, too.

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 10:20_


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

_Comment by @MichaReiser on 2025-12-08 10:31_

I haven't tested it but this is what we use in most tempdir tests:

https://github.com/astral-sh/ruff/blob/646ad154c37fbec184805b9b36626b7e8782a16c/crates/ty/tests/file_watching.rs#L379-L386

---

_Comment by @sharkdp on 2025-12-08 10:44_

> I haven't tested it but this is what we use in most tempdir tests:

There was a problem with a UNC prefix, which is why I used `dunce::canonicalize` now. Seems to work.

---

_Merged by @sharkdp on 2025-12-08 10:44_

---

_Closed by @sharkdp on 2025-12-08 10:44_

---

_Branch deleted on 2025-12-08 10:44_

---
