```yaml
number: 15736
title: "[`flake8-tidy-imports`] Allow banning global symbols (`TID251`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - configuration
assignees: []
base: main
head: TID251
created_at: 2025-01-25T00:41:49Z
updated_at: 2025-03-06T10:09:35Z
url: https://github.com/astral-sh/ruff/pull/15736
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-tidy-imports`] Allow banning global symbols (`TID251`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-25 00:41_

## Summary

Resolves #10079.

This change makes the following settings work:

```toml
[tool.ruff.lint.flake8-tidy-imports.banned-api]
"list".msg = "Lists considered harmful"
"builtins.tuple".msg = "Mutability for the win"
```

Before:

```python
a = list()   # No error
b = tuple()  # No error

from builtins import tuple  # Error, but who imports from `builtins` anyway?
c = tuple()  # No error
```

After:

```python
a = list()   # TID251: `list` is banned: Lists considered harmful
b = tuple()  # TID251: `builtins.tuple` is banned: Mutability for the win

from builtins import tuple  # TID251: `builtins.tuple` is banned: Mutability for the win
c = tuple()  # TID251: `builtins.tuple` is banned: Mutability for the win
```

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-25 01:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/262046709ab48fa3babdabdccd7c1fd9e9255b82/src/scikit_build_core/file_api/_cattrs_converter.py#L66'>src/scikit_build_core/file_api/_cattrs_converter.py:66:17:</a> TID251 `typing.Callable` is banned: Use collections.abc.Callable instead.
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/262046709ab48fa3babdabdccd7c1fd9e9255b82/src/scikit_build_core/file_api/reply.py#L116'>src/scikit_build_core/file_api/reply.py:116:17:</a> TID251 `typing.Callable` is banned: Use collections.abc.Callable instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TID251 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-01-25 02:30_

This change seems to be for the worse, as both the imports and the usages are now reported:

[@scikit-build/scikit-build-core](https://github.com//blob/262046709ab48fa3babdabdccd7c1fd9e9255b82/src/scikit_build_core/file_api/_cattrs_converter.py#L66):

```python
from typing import Any, Callable, Dict, Type, TypeVar  # noqa: TID251
...
rich_print: Callable[[object], None]
```

Perhaps non-import references should only be reported if they are for global symbols?

---

_Label `configuration` added by @dylwil3 on 2025-01-30 14:02_

---

_@iFreilicht reviewed on 2025-03-06 10:09_

Just to make sure I understand correctly: To fully ban a built-in API, I have to add both the `list` and `builtins.list` cases to the configuration, correct? That's the one thing I would like to see improved, I think a single line should ban all usage of the API. Personally I would say `builtins.list` is clearer. That case is also easy to check for in the code, which would allow to fix the problem @InSyncWithFoo brought up.

Generally, I'm in favor of this change. In my case it would be particularly useful because of `round`, because that has undesirable properties and want to make sure other developers use my custom replacement in my codebase.

---
