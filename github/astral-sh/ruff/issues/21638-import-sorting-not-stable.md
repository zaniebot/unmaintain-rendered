---
number: 21638
title: Import Sorting not stable
type: issue
state: closed
author: noahdormann
labels:
  - question
  - great writeup
assignees: []
created_at: 2025-11-26T10:04:04Z
updated_at: 2025-12-29T18:00:46Z
url: https://github.com/astral-sh/ruff/issues/21638
synced_at: 2026-01-07T13:12:16-06:00
---

# Import Sorting not stable

---

_Issue opened by @noahdormann on 2025-11-26 10:04_

### Question


Hi, we’re using Ruff in our project and it’s been great so far.
However, we’ve run into a strange inconsistency with import sorting. Two machines that should be configured identically are producing different sorted imports.

Environment (both machines):
- Ubuntu 24.04.3 LTS 
- uv installed through the official installer script
- same Git project checked out on the same commit
- Virtual environment created via `uv sync` (uv.lock file is in git)
- `ruff --version` reports 0.14.6
-  `ruff -v check --select I --show-settings` report identical settings
- pyproject.toml is the only config source found running `ruff -v check --select I --fix 2>&1 | grep conf`

The issue:
On one machine, this file gets sorted like this:
```python
from logging.config import fileConfig

from alembic import context
from sqlalchemy import engine_from_config, pool
```

On the other machine, the result is:
```python
from logging.config import fileConfig

from sqlalchemy import engine_from_config, pool

from alembic import context
```

Both machines consistently produce the same ordering every time, but they disagree with each other.

What I’ve tried / verified:
 - [x] Same Ruff version
 - [x] Same project directory
 - [x] Same virtual environment setup
 - [x] Same command invocation
 - [x] Same --show-settings output
 - [x] Same config file detected (pyproject.toml)
 - [x] Same source tree

At this point I’m not sure what else could cause two Ruff installs with identical configs to classify alembic and sqlalchemy differently.
Is there anything I might be missing, or any additional diagnostic step I could run to narrow this down?
I would write this up as a bug report, if I had an idea how to create a reproducible setup.

Thanks!

### Version

0.14.6

---

_Label `question` added by @noahdormann on 2025-11-26 10:04_

---

_Label `great writeup` added by @MichaReiser on 2025-11-26 10:06_

---

_Comment by @MichaReiser on 2025-11-26 10:10_

Thanks for the great write up! 

There are two possible causes:

* There's a known issue with isort that Ruff's caching sometimes isn't correctly invalidated. Can you try running `ruff clean` on both machines?
* isort looks at what folders exist on your file system to decide whether a package is a first-party or third-party library. Any chance there's a local `sqlalchemy` or `alembic` folder on one of the two machines? 

What I've found most useful in the past is to run ruff with `ruff check -v <problematic_file>`. It makes Ruff print debug logs, which include Ruff's reasoning for classifying a specific module in a specific way. The easiest might be to run the command with `-v` on both machines and than compare the log output (using something like https://www.diffchecker.com/)

---

_Comment by @noahdormann on 2025-11-26 13:34_

Thank you for the quick reply!

I was able to pinpoint the issue with your help 🥳 . We recently refactored our project structure and moved the standard alembic directory from `./alembic` to `./core/alembic`. On my colleague’s machine the old `./alembic` directory still existed, because Git doesn’t remove empty folders. Removing that leftover directory resolved the inconsistent sorting.

The only remaining question is that the directory was completely empty and contained no `__init__.py`, so it’s a bit surprising that Ruff detected it as a Python package or used it for module classification.

To answer my own question, I was not aware of [PEP 420](https://peps.python.org/pep-0420/) allowing *namespace packages*. Thus, I assume this is expected behavior on Ruff’s side. Thanks again for the quick assistance!

---

_Referenced in [astral-sh/ruff#21647](../../astral-sh/ruff/issues/21647.md) on 2025-11-26 21:50_

---

_Comment by @MichaReiser on 2025-11-27 06:48_

> To answer my own question, I was not aware of [PEP 420](https://peps.python.org/pep-0420/) allowing namespace packages. Thus, I assume this is expected behavior on Ruff’s side. Thanks again for the quick assistance!

Yes, this is because of namespace packages. Depending on your project layout, setting [`src`](https://docs.astral.sh/ruff/settings/#src) can also help (e.g. if you have a src layout, set it to `["src"]`)

---

_Comment by @noahdormann on 2025-11-27 12:01_

Thanks you! I close the question as solved.

---

_Closed by @noahdormann on 2025-11-27 12:01_

---

_Comment by @txomon on 2025-12-29 17:50_

Hello, I was just hit with this "feature" and after spending almost all Christmas on it, would it be possible to remove it? or at least deactivate it?

---
