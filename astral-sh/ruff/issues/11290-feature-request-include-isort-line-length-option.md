```yaml
number: 11290
title: "Feature request: include isort `line_length` option (to allow decouping of linting from formatting)"
type: issue
state: closed
author: rwarren
labels:
  - isort
assignees: []
created_at: 2024-05-05T13:35:39Z
updated_at: 2024-05-05T13:41:54Z
url: https://github.com/astral-sh/ruff/issues/11290
synced_at: 2026-01-10T11:09:53Z
```

# Feature request: include isort `line_length` option (to allow decouping of linting from formatting)

---

_Issue opened by @rwarren on 2024-05-05 13:35_

Ruff version: 0.4.3

### Request in short:

It would be nice if Ruff's `isort` implementation supported isort's `line_length` setting independently from Ruff's global `line-length` setting.

The width threshold used for _warning_ about line length issues should not exactly match the value used for automatic _formatting_ (line wrapping). Warnings and auto-formatting are different things and it would be nice if Ruff didn't rigidly couple them.

This could be done here by just adding support for `line_length` in the `lint.isort` settings.

### Rationale:

I use Ruff for linting, but do not use it for overall file formatting.  I do use Ruff's embedded `isort` formatting extensively.

**For linting the line length:**

  * I want Ruff to tell me when a line is _definitely_ too long (using `E501`) according to Ruff's `line-length` setting
  * I set `line-length` to be fairly long (150) to allow for edge cases when it just happens (e.g. an inline comment extending a few characters past normal expected line width)

**For automatic line length formatting:**

  *  I do not use the black implementation
  *  I do use Ruff's isort extensively (through "Fix all auto-fixable problems")
  * I do _generally_ want lines wrapped at 100 chars, but not religiously so like `black` would do
    * fwiw, I handle line lengths in files with vertical rulers in vscode, as well as through the vscode Rewrap extension (with Ruff linting catching obscene line lengths)
    * I think the line length styling should mostly be up to the user/use-case, with the linter being configured to catch clear problems

In most cases long `from ... import ...` lines shouldn't happen in a code base, but (aside from this being a style choice) it is common in some libraries like PySide6, where lines like this are common:

```
from PySide6.QtWidgets import QGraphicsItem, QGraphicsLineItem, QGraphicsPixmapItem, QGraphicsScene, QGraphicsTextItem, QGraphicsView, QVBoxLayout
```

The `isort` implementation nicely generated that line... but it is too long because the linter `line-length` threshold is intentionally long, and Ruff is wrapping using it.

---

_Comment by @charliermarsh on 2024-05-05 13:37_

Thanks, though I think this is a duplicate of #3206?

---

_Label `isort` added by @charliermarsh on 2024-05-05 13:37_

---

_Closed by @charliermarsh on 2024-05-05 13:37_

---

_Comment by @rwarren on 2024-05-05 13:41_

... not sure how I missed #3206!!  🤦  It is a duplicate.

I think this is because I landed here after finding #4048 (about `use_parentheses`) and re-targeted the report while writing it.

Rationale here is basically in support of #3206. Shall I move the text over there?

---
