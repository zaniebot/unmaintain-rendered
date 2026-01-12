```yaml
number: 13309
title: "Pattern in `lint.isort.sections` overrides detected `first-party` "
type: issue
state: open
author: sscherfke
labels:
  - question
  - isort
assignees: []
created_at: 2024-09-10T14:14:16Z
updated_at: 2024-09-18T07:34:42Z
url: https://github.com/astral-sh/ruff/issues/13309
synced_at: 2026-01-12T15:54:52Z
```

# Pattern in `lint.isort.sections` overrides detected `first-party` 

---

_@sscherfke_

Ruff version: 0.6.3
Rule: `I001` `unsorted-imports`

----

All our internal packages have a common prefix (e.g., `spam_`).

We want to declare these internal packages as "second party" and create a corresponding import section between third-party and first party sections.

The minimal configuration for this is

```toml
[lint]
select = ["I"]

[lint.isort]
lines-after-imports = 2
section-order = ["future", "standard-library", "third-party", "second-party", "first-party", "local-folder"]

[lint.isort.sections]
second-party = ["spam_*"]
```

Or as cmd line args:  `ruff check --fix --isolated --select I --config 'lint.isort.section-order = ["future", "standard-library", "third-party", "second-party", "first-party", "local-folder"]' --config 'lint.isort.sections.second-party = ["spam_*"]'`

Expected result:

```python
import os
import sys

import attrs
import rich

import spam_lib_a
import spam_lib_b

import spam_current_project
```

Current result:

```python
import os
import sys

import attrs
import rich

import spam_current_project
import spam_lib_a
import spam_lib_b
```

It looks like the pattern for the `second-party` section overides the packages collected from the local folder.
The issue seems related to https://github.com/astral-sh/ruff/issues/11542


---

_Label `question` added by @MichaReiser on 2024-09-11 19:32_

---

_Label `isort` added by @MichaReiser on 2024-09-11 19:32_

---

_Comment by @MichaReiser on 2024-09-11 19:32_

@charliermarsh, do you know how to configure the desired behavior?

---

_Comment by @charliermarsh on 2024-09-18 03:27_

This behavior looks correct to me? Explicitly-defined sections do take precedence over the default sections. `spam_current_project` matches `spam_*`, so it will always be classified as `second-party`.

I think you could perhaps do something like this:

```toml
[lint]
select = ["I"]

[lint.isort]
lines-after-imports = 2
section-order = ["future", "standard-library", "third-party", "second-party", "current-project", "local-folder"]

[lint.isort.sections]
current-project = ["spam_current_project"]
second-party = ["spam_*"]
```

---

_Comment by @sscherfke on 2024-09-18 07:34_

We try to use a single, central ruff config for all projects so adding `current-project = [...]` is not really an option for us.

I could also live with the current behavior if it is the expected behavior.  It is not the most important thing ever to split second and third party imports into different blocks. :wink: 

However, the docs only state that the *sections* override the built-in sections.  It was not clear to me that packages matched by the sections definition would override the "current project".

---
