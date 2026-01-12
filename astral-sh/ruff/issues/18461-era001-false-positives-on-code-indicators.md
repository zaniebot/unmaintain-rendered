```yaml
number: 18461
title: ERA001 false positives on CODE_INDICATORS
type: issue
state: open
author: SXHRYU
labels:
  - rule
assignees: []
created_at: 2025-06-04T13:47:44Z
updated_at: 2025-06-17T11:40:21Z
url: https://github.com/astral-sh/ruff/issues/18461
synced_at: 2026-01-12T15:54:56Z
```

# ERA001 false positives on CODE_INDICATORS

---

_@SXHRYU_

### Summary

If the comment or comment line consists of one word derived from "return"/"break"/"continue"/"import" (i.e. [CODE_INDICATORS](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/eradicate/detection.rs#L10-L15)) it is considered valid python code.

Examples:

```python
# returnable  noqa: ERA001
# imports  noqa: ERA001
# breaking  noqa: ERA001
```

It can also be detrimental if it triggers on normal comment that contains function name on a new line:
```python
# this is a long comment explaining a bug that happens when calling
# on_return  noqa: ERA001
# (^ this can be a valid function name)
```

Perhaps it's viable to wrap [`if !CODE_INDICATORS.is_match(line)`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/eradicate/detection.rs#L79C1-L81C6) to some less-greedy function?

### Version

0.11.12

---

_Comment by @ntBre on 2025-06-04 18:05_

My somewhat naive idea would just be to use a `Regex` instead of an `AhoCorasick` for the keywords and throw `\b` on the ends.

It does look like the current implementation is consistent with the [upstream version](https://github.com/PyCQA/eradicate/blob/7ac47683e79ebdb8a86c8f66a24a6a8ced2ea028/eradicate.py#L89-L93), but it would make sense to me to check for an exact match instead.

---

_Label `rule` added by @ntBre on 2025-06-04 18:05_

---
