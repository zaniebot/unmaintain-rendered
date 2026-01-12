```yaml
number: 1804
title: Implement doc line length enforcement
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/max-doc-length
created_at: 2023-01-12T03:28:22Z
updated_at: 2023-01-12T19:54:21Z
url: https://github.com/astral-sh/ruff/pull/1804
synced_at: 2026-01-12T15:55:07Z
```

# Implement doc line length enforcement

---

_@charliermarsh_

This PR implements `W505` (`DocLineTooLong`), which is similar to `E501` (`LineTooLong`) but confined to doc lines.

I based the "doc line" definition on pycodestyle, which defines a doc line as a standalone comment or string statement. Our definition is a bit more liberal, since we consider any string statement a doc line (even if it's part of a multi-line statement) -- but that seems fine to me.

Note that, unusually, this rule requires custom extraction from both the token stream (to find standalone comments) and the AST (to find string statements).

Closes #1784.


---

_Merged by @charliermarsh on 2023-01-12 03:32_

---

_Closed by @charliermarsh on 2023-01-12 03:32_

---

_Branch deleted on 2023-01-12 03:32_

---

_Comment by @stinodego on 2023-01-12 19:48_

Thank you! Much appreciated ❤️ 

---

_Comment by @charliermarsh on 2023-01-12 19:52_

@stinodego - Of course! Let me know if you notice any discrepancies. I'm detecting "doc lines" using a different strategy, but it _should_ have identical behavior apart from a few weird edge cases (like multi-line statements that contain string expressions, e.g., `x = 1; "standalone string"; y = 2`).

---

_Comment by @stinodego on 2023-01-12 19:53_

I just tried to break it a few different ways, and it seems to work just fine for all the use cases I could come up with! Will let you know if I run into anything odd.


---
