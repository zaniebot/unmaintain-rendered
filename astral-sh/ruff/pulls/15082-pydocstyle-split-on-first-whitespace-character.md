```yaml
number: 15082
title: "[`pydocstyle`] Split on first whitespace character (`D403`)"
type: pull_request
state: merged
author: my1e5
labels:
  - rule
assignees: []
merged: true
base: main
head: d403-split-on-whitespace
created_at: 2024-12-20T14:42:15Z
updated_at: 2024-12-20T19:37:04Z
url: https://github.com/astral-sh/ruff/pull/15082
synced_at: 2026-01-10T20:42:27Z
```

# [`pydocstyle`] Split on first whitespace character (`D403`)

---

_Pull request opened by @my1e5 on 2024-12-20 14:42_

## Summary

This PR fixes an issue where Ruff's `D403` rule (`first-word-uncapitalized`) was not detecting some single-word edge cases that are picked up by `pydocstyle`.

The change involves extracting the first word of the docstring by identifying the first whitespace character. This is consistent with `pydocstyle` which uses `.split()` - see https://github.com/PyCQA/pydocstyle/blob/8d0cdfc93e86e096bb5753f07dc5c7c373e63837/src/pydocstyle/checker.py#L581C13-L581C64

## Example

Here is a playground example - https://play.ruff.rs/eab9ea59-92cf-4e44-b1a9-b54b7f69b178

```py
def example1():
    """foo"""

def example2():
    """foo
    
    Hello world!
    """

def example3():
    """foo bar

    Hello world!
    """

def example4():
    """
    foo
    """

def example5():
    """
    foo bar
    """
```

`pydocstyle` detects all five cases:
```bash
$ pydocstyle test.py --select D403
dev/test.py:2 in public function `example1`:
        D403: First word of the first line should be properly capitalized ('Foo', not 'foo')
dev/test.py:5 in public function `example2`:
        D403: First word of the first line should be properly capitalized ('Foo', not 'foo')
dev/test.py:11 in public function `example3`:
        D403: First word of the first line should be properly capitalized ('Foo', not 'foo')
dev/test.py:17 in public function `example4`:
        D403: First word of the first line should be properly capitalized ('Foo', not 'foo')
dev/test.py:22 in public function `example5`:
        D403: First word of the first line should be properly capitalized ('Foo', not 'foo')
```
Ruff (`0.8.4`) fails to catch example2 and example4.

## Test Plan

* Added two new test cases to cover the previously missed single-word docstring cases.


---

_Review requested from @dylwil3 by @MichaReiser on 2024-12-20 14:47_

---

_Comment by @github-actions[bot] on 2024-12-20 14:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @my1e5 on 2024-12-20 14:59_

Closes https://github.com/astral-sh/ruff/issues/13167

---

_@dylwil3 approved on 2024-12-20 15:47_

Good catch! This looks good to me, thank you for your contribution!

---

_@dylwil3 reviewed on 2024-12-20 15:49_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pydocstyle/D403.py`:1 on 2024-12-20 15:49_

Would you mind adding the example from #13167 ?

---

_Review requested from @dylwil3 by @my1e5 on 2024-12-20 17:08_

---

_@dylwil3 approved on 2024-12-20 18:54_

---

_Comment by @dylwil3 on 2024-12-20 18:55_

Wonderful, thanks so much for your contribution!

---

_Merged by @dylwil3 on 2024-12-20 18:55_

---

_Closed by @dylwil3 on 2024-12-20 18:55_

---

_Label `rule` added by @dylwil3 on 2024-12-20 18:58_

---

_Branch deleted on 2024-12-20 19:37_

---
