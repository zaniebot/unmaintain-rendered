```yaml
number: 2440
title: "feat: Add isort option lines-after-imports"
type: pull_request
state: merged
author: reidswan
labels: []
assignees: []
merged: true
base: main
head: isort-lines-after-import
created_at: 2023-02-01T14:03:24Z
updated_at: 2023-02-02T02:39:46Z
url: https://github.com/astral-sh/ruff/pull/2440
synced_at: 2026-01-12T04:52:00Z
```

# feat: Add isort option lines-after-imports

---

_Pull request opened by @reidswan on 2023-02-01 14:03_

Fixes https://github.com/charliermarsh/ruff/issues/2243

Adds support for the isort option [lines_after_imports](https://pycqa.github.io/isort/docs/configuration/options.html#lines-after-imports) to insert blank lines between imports and the follow up code.

---

_@charliermarsh reviewed on 2023-02-01 17:53_

---

_Review comment by @charliermarsh on `ruff.schema.json`:961 on 2023-02-01 17:53_

I'm tempted to use `null` as the automatic determination value... But, I guess in that case, we wouldn't be able to tell the difference between a config using the default value and the config using the automatic value, in the event that we need to merge two configuration files (via `extend`).

---

_Comment by @charliermarsh on 2023-02-01 17:54_

This looks good! Just curious: what's the typical use-case here? E.g., what styling do you typically enforce with this setting?

---

_Comment by @spaceone on 2023-02-01 18:23_

it separates the import lines from every following.
e.g. with `lines-after-imports=2` you prevent `E302` from happening.

---

_@charliermarsh reviewed on 2023-02-01 19:05_

---

_Review comment by @charliermarsh on `src/rules/isort/rules/organize_imports.rs`:92 on 2023-02-01 19:05_

We respect the `0` here to avoid things like:

```py
if True:
  import foo


x = 1
```

That is: the extra newlines when an import comes at the end of a block. Is that correct? (Was wondering why the `if settings` condition wasn't first.)

---

_@charliermarsh reviewed on 2023-02-02 02:17_

---

_Review comment by @charliermarsh on `src/rules/isort/rules/organize_imports.rs`:92 on 2023-02-02 02:17_

I think this isn't quite the right place for this. If I set `lines-after-imports` to 3, and run over `resources/test/fixtures/isort/lines_after_imports_class_after.py`, I get:

```py
from __future__ import annotations

from typing import Any

from my_first_party import my_first_party_object
from requests import Session

from . import my_local_folder_object


    self.name = name
```

I believe that `num_trailing_lines` here is meant to be the number of existing trailing lines, not the number of _desired_ trailing lines.

Instead, I believe this needs to go in `format_imports`, where we do this:

```rs
    match trailer {
        None => {}
        Some(Trailer::Sibling) => {
            output.push_str(stylist.line_ending());
        }
        Some(Trailer::FunctionDef | Trailer::ClassDef) => {
            output.push_str(stylist.line_ending());
            output.push_str(stylist.line_ending());
        }
    }
```

---

_@charliermarsh reviewed on 2023-02-02 02:20_

---

_Review comment by @charliermarsh on `src/rules/isort/rules/organize_imports.rs`:92 on 2023-02-02 02:20_

I'll change and merge.

---

_Merged by @charliermarsh on 2023-02-02 02:39_

---

_Closed by @charliermarsh on 2023-02-02 02:39_

---
