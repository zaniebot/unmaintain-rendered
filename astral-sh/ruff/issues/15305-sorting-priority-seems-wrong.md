---
number: 15305
title: Sorting priority seems wrong?
type: issue
state: closed
author: daenny
labels:
  - question
  - isort
assignees: []
created_at: 2025-01-06T16:52:26Z
updated_at: 2025-01-07T07:58:34Z
url: https://github.com/astral-sh/ruff/issues/15305
synced_at: 2026-01-10T01:22:56Z
---

# Sorting priority seems wrong?

---

_Issue opened by @daenny on 2025-01-06 16:52_

We prefix everything in our company. I would like to sort all company package before my current package. In `isort` that works. 

example `test.py`:

```python
import sys

import third_party

import my_company_package
import my_extra_package

import my_current_package
```

I have the following configuration:

```toml
[lint]
select = ["I"]

[lint.isort]
required-imports = ["from __future__ import annotations"]
known-first-party = ["my_current_package", "my_current package.*"]
section-order = ["future", "standard-library", "third-party", "my-company", "first-party","local-folder"]

[lint.isort.sections]
my-company = ["my_*"]
```

running:
```
ruff check test.py --fix
```

with `test.py`:

will result in:
```python
from __future__ import annotations

import sys

import third_party

import my_company_package
import my_current_package
import my_extra_package
```

Is this a bug or wrong configuration?

---

_Label `question` added by @MichaReiser on 2025-01-06 17:43_

---

_Comment by @MichaReiser on 2025-01-06 17:47_

Hi @daenny 

I played with this locally and the issue is that the custom `sections` take precedence over `known-local-folders` (`local-folders`) and `known-first-party`. For example, it works as expected if I change the "local" folder to `other` which doesn't match the `sections` regex:

```toml
[lint]
select = ["I"]

[lint.isort]
required-imports = ["from __future__ import annotations"]
  known-local-folder = ["my_current_package", "my_current package"]
section-order = ["future", "standard-library", "third-party", "my-company", "first-party", "local-folder"]

[lint.isort.sections]
my-company = ["my_*"]
```

Gets sorted to

```py
import abc
import sys

import abcd
import third_party

import my_company_package
import my_extra_package

import other
```


Now, I think you'd need a negative Glob for the `my-company` package selector, but Ruff doesn't support negative globs. I guess your best solution for now is to make the `my-company` glob more explicit, so that it doesn't select the local folder anymore.

---

_Label `isort` added by @AlexWaygood on 2025-01-06 17:48_

---

_Comment by @daenny on 2025-01-06 19:51_

Thanks for investigating. 
I did try to change the `my-company` glob in various ways, but without negative selection (which gets ignored indeed) I would need to list all packages separately, which is cumbersome and more difficult to template. 
Currently, we are using the python `isort` pre-commit, which works as expected with the corresponding configuration. I am replacing as much as I can with `ruff`. 
I am not on a pc now, I will test tomorrow , if it works if I add another custom section with `my_current_package`. 
But so this seems to be a bug in the resolution then. 

---

_Comment by @MichaReiser on 2025-01-07 07:19_

It's a bit hard to make an example without knowing the exact package names but working with what you provided:

```toml
[lint.isort.sections]
my-company = [
	# Match all packages except the once starting with my_c, e.g. `my_current_package`
	"my_[!c]*",
	# Add more specific patterns until all conflicts with other non-local packages are resolved
	"my_c[!u]*",
	"my_cu[!r]"
]
```

---

_Comment by @daenny on 2025-01-07 07:56_

Thanks for your suggestions. 
I solved it by using this now. This works for me:

```toml
[lint]
select = ["I"]

[lint.isort]
required-imports = ["from __future__ import annotations"]
section-order = ["future", "standard-library", "third-party", "my-company", "first-party", "my-package", "local-folder"]

[lint.isort.sections]
my-package = ["my_package"]
my-company = ["my_*"]
```

---

_Comment by @MichaReiser on 2025-01-07 07:58_

That's even better :)

---

_Closed by @MichaReiser on 2025-01-07 07:58_

---
