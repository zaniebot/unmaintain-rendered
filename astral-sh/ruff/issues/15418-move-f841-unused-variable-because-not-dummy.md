---
number: 15418
title: Move F841 unused variable because not dummy syntax on preview into a specific rule
type: issue
state: closed
author: un-pogaz
labels:
  - question
  - rule
assignees: []
created_at: 2025-01-11T10:22:20Z
updated_at: 2025-01-12T09:48:49Z
url: https://github.com/astral-sh/ruff/issues/15418
synced_at: 2026-01-10T01:22:56Z
---

# Move F841 unused variable because not dummy syntax on preview into a specific rule

---

_Issue opened by @un-pogaz on 2025-01-11 10:22_

For this code:
```python
def make_tuple(idx):
    return idx, str(idx)


def foo(count):
    for i in range(count):
        value, dummy = make_tuple(i)
        print(value)
```

When run Ruff in normal mode, return no error. But when run into preview mode, Ruff sudendly raise unused variable error because the  dummy syntax is not used for the unpacking tuple, even if your use the `explicit-preview-rules` settings on `true`.

Except I don't want use dummy syntax. After having explored different options, settings and rules, I haven't found any solution that doesn't create more problems, and I'm obliged to ignore the rule 'F841' from the entire project (which is far from great, but clearly the lesser evil).

**Excepted behavior:**

Run Ruff in preview mode with `explicit-preview-rules` settings on `true`, the output should be identical to Ruff into normal mode.

**Suggestion:**

Create a specific (preview) rule to control and raise error if a unused variable should use dummy syntax instead.

This will make it possible to finely control Ruff's behavior, while preserving consistency output between normal and preview mode.

---

_Label `rule` added by @dylwil3 on 2025-01-11 14:46_

---

_Label `preview` added by @dylwil3 on 2025-01-11 14:46_

---

_Label `preview` removed by @MichaReiser on 2025-01-11 15:49_

---

_Label `question` added by @MichaReiser on 2025-01-11 15:49_

---

_Comment by @MichaReiser on 2025-01-12 09:48_

Hi @un-pogaz 

It's currently not possible to only opt-in to preview behavior of some rules but not of others because it would require an explicit syntax in the configuration.

Splitting this rule has already been proposed in another issue. I'll merge the two

---

_Closed by @MichaReiser on 2025-01-12 09:48_

---
