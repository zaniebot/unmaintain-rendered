```yaml
number: 17374
title: ERA001 for decorators
type: issue
state: open
author: amotl
labels:
  - rule
assignees: []
created_at: 2025-04-13T12:39:12Z
updated_at: 2025-04-22T08:51:58Z
url: https://github.com/astral-sh/ruff/issues/17374
synced_at: 2026-01-12T15:54:55Z
```

# ERA001 for decorators

---

_@amotl_

### Question

Hi there,

CodeRabbit [just discovered](https://github.com/crate/cratedb-toolkit/pull/399#pullrequestreview-2762750642) commented-out code which apparently has not been discovered by Ruff? It looks like comment strings starting with `@` (Python decorators) are not considered "code" by the `ERA001` rule. Is that intended?

This is a little repro:
```python
# @cli.command()
def foo():
    return 42
```

![Image](https://github.com/user-attachments/assets/e7f7c3f6-cc7f-4ca6-aa62-e955d45ef809)

With kind regards,
Andreas.


### Version

ruff 0.11.5 (7186d5e9a 2025-04-10)

---

_Label `question` added by @amotl on 2025-04-13 12:39_

---

_Comment by @ntBre on 2025-04-14 01:36_

Oh interesting. It looks like the decorator makes it through all of the heuristics for identifying source code and then fails to parse as a python module, which is the very last check in [`comment_contains_code`](https://github.com/astral-sh/ruff/blob/67b5cf470eff84272255ca8b6baa64855ba733c0/crates/ruff_linter/src/rules/eradicate/detection.rs#L75).

This seems consistent with the upstream linter, which also fails to [`compile`](https://github.com/PyCQA/eradicate/blob/5e9eb212c99afd8734b6f33d9d5b5107d10ed9cd/eradicate.py#L116C13-L116C20) this line, but we could possibly consider a different approach.

---

_Label `question` removed by @MichaReiser on 2025-04-22 08:51_

---

_Label `rule` added by @MichaReiser on 2025-04-22 08:51_

---
