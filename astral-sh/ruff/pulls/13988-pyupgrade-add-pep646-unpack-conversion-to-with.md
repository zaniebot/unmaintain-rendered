```yaml
number: 13988
title: "[`pyupgrade`] - add PEP646 Unpack conversion to `*` with fix (`UP044`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-pep646-unpack
created_at: 2024-10-30T03:50:27Z
updated_at: 2024-11-01T21:18:28Z
url: https://github.com/astral-sh/ruff/pull/13988
synced_at: 2026-01-12T15:55:46Z
```

# [`pyupgrade`] - add PEP646 Unpack conversion to `*` with fix (`UP044`)

---

_@diceroll123_

## Summary

Adds `Unpack[...]` -> `*...` for Py311+

Closes #13891 

## Test Plan

`cargo test`


---

_Label `rule` added by @charliermarsh on 2024-10-30 04:04_

---

_Comment by @charliermarsh on 2024-10-30 04:05_

Nice, thanks.

---

_Comment by @github-actions[bot] on 2024-10-30 04:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/cache.py#L73'>rotkehlchen/db/cache.py:73:36:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/cache.py#L77'>rotkehlchen/db/cache.py:77:36:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/cache.py#L81'>rotkehlchen/db/cache.py:81:36:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/cache.py#L85'>rotkehlchen/db/cache.py:85:36:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L699'>rotkehlchen/db/dbhandler.py:699:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L708'>rotkehlchen/db/dbhandler.py:708:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L717'>rotkehlchen/db/dbhandler.py:717:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L726'>rotkehlchen/db/dbhandler.py:726:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L735'>rotkehlchen/db/dbhandler.py:735:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L744'>rotkehlchen/db/dbhandler.py:744:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L753'>rotkehlchen/db/dbhandler.py:753:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L776'>rotkehlchen/db/dbhandler.py:776:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L786'>rotkehlchen/db/dbhandler.py:786:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L796'>rotkehlchen/db/dbhandler.py:796:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L806'>rotkehlchen/db/dbhandler.py:806:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L816'>rotkehlchen/db/dbhandler.py:816:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L826'>rotkehlchen/db/dbhandler.py:826:23:</a> UP044 [*] Use `*` for unpacking
+ <a href='https://github.com/rotki/rotki/blob/45b14b5f3d77221183d1a41d6a1cb48d92181891/rotkehlchen/db/dbhandler.py#L836'>rotkehlchen/db/dbhandler.py:836:23:</a> UP044 [*] Use `*` for unpacking
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP044 | 18 | 18 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:78 on 2024-10-30 07:05_

Unpacking anything other than a `Tuple` or `TypeVarTuple` is semantically incorrect but it isn't a syntax error. For example:

```py
def foo(*args: Unpack[int | str]) -> None:
```

Could we add a test asserting that the suggested fix doesn't suggest invalid syntax?



---

_@MichaReiser requested changes on 2024-10-30 07:18_

This is great. Thank you! 

Could you add one more test case for an unpacking where the target is a binary expression? This is is semantically invalid but it isn't a syntax error and we should at least avoid introducing a syntax error (I don't think we do, but we should make sure of it).

---

_@dhruvmanila reviewed on 2024-10-30 10:52_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:76 on 2024-10-30 10:52_

Why is this condition required when the rule itself is in preview?

---

_Label `preview` added by @dhruvmanila on 2024-10-30 10:52_

---

_@diceroll123 reviewed on 2024-10-30 23:23_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:76 on 2024-10-30 23:23_

Silly me!

---

_Review requested from @MichaReiser by @diceroll123 on 2024-10-31 01:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:75 on 2024-10-31 06:41_

Applying the fix seems "safe enough":

```
def foo(*args: *int | str) -> None: pass  # not supported
```

Isn't invalid syntax

---

_@MichaReiser reviewed on 2024-10-31 06:41_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:75 on 2024-10-31 06:49_

ah, true, it gives a TypeError actually.

---

_@diceroll123 reviewed on 2024-10-31 06:49_

---

_@MichaReiser reviewed on 2024-10-31 06:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep646_unpack.rs`:75 on 2024-10-31 06:51_

I went ahead and converted it to an allowlist. I don't think any expressions other than name, subscript, and attribute are valid in this position.

---

_@MichaReiser approved on 2024-10-31 06:54_

---

_Merged by @MichaReiser on 2024-10-31 06:58_

---

_Closed by @MichaReiser on 2024-10-31 06:58_

---

_Comment by @IbarraSamuel on 2024-11-01 21:14_

It's giving SyntaxError when changing this:
```python3
from typing import TypedDict, Unpack


class KWs(TypedDict):
    """kws."""

    foo: str


def func(**kwargs: Unpack[KWs]) -> None:  # <-- original
    """Func."""
    _ = kwargs
```

to this:
```python3
from typing import TypedDict, Unpack


class KWs(TypedDict):
    """kws."""

    foo: str


def func(**kwargs: *KWs) -> None:  # <-- modified
    """Func."""
    _ = kwargs
```

---

_Comment by @MichaReiser on 2024-11-01 21:18_

@sibarras see https://github.com/astral-sh/ruff/issues/14047

---
