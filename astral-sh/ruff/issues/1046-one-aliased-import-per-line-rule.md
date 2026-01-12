```yaml
number: 1046
title: One aliased import per line rule
type: issue
state: closed
author: smackesey
labels:
  - isort
  - configuration
assignees: []
created_at: 2022-12-05T01:05:35Z
updated_at: 2022-12-05T02:23:13Z
url: https://github.com/astral-sh/ruff/issues/1046
synced_at: 2026-01-12T15:54:40Z
```

# One aliased import per line rule

---

_@smackesey_

The codebase I work on uses code like this for explicit re-exports in many `__init__.py` files:

```
from .utils import (
    test_directory as test_directory,
    test_id as test_id
)
```

It's useful to have code like this have exactly one import per line-- it's easier to tell at a glance what is being exported.

It's possible I've missed something but I don't think either `isort` or `ruff` offers a way to reliably achieve this formatting. By default, both formatters produce (1) below. If you turn on Isort's `combine-as` or ruff's `combine-as-imports` and you are under the line length limit, then you get (2).

```
# (1) One aliased import per line in separate import statements

from .utils import test_directory as test_directory
from .utils import test_id as test_id

# (2) Multiple imports per line if under line limit

from .utils import test_directory as test_directory, test_id as test_id
```

To get around this, we currently disable `isort` using `# isort: off` comments when we use this pattern. The lack of ability to do something similar in `ruff` is preventing us adopting `ruff` in place of `isort`.

I propose an option that forces aliased imports to a single line, but still groups them together in a single import statement-- then we wouldn't even need to turn off the import sorter.

---

_Comment by @charliermarsh on 2022-12-05 01:07_

Happy to support this.

If there's only a single import, are you imaging that it still enforces the same style? E.g.:

```py
from .utils import (
    test_directory as test_directory,
)
```

Or would single-imports be retained on their own lines, like:

```py
from .utils import test_directory as test_directory
```


---

_Label `isort` added by @charliermarsh on 2022-12-05 01:09_

---

_Label `configuration` added by @charliermarsh on 2022-12-05 01:09_

---

_Comment by @smackesey on 2022-12-05 01:10_

Somewhat ambivalent but I prefer (1) to (2):

```
# (1)
from .utils import (
    test_directory as test_directory,
)

# (2)
from .utils import test_directory as test_directory
```

---

_Comment by @charliermarsh on 2022-12-05 01:10_

Cool, I can add this now and cut another release.

---

_Comment by @charliermarsh on 2022-12-05 01:11_

Would it make sense to _always_ enforce this (one-per-line) behavior? Or at least for all `import-from` statements? Or would it be inconvenient if it applied beyond explicit re-exports?

---

_Comment by @charliermarsh on 2022-12-05 01:23_

(For now, I'll just apply this to `from` blocks where every member is a re-export.)

---

_Comment by @smackesey on 2022-12-05 01:25_

I think the rule makes sense for all `from X import Y as Z` imports-- visually parsing multiple `Y as Z` clauses on the same line can be difficult, regardless of whether they're explicit reexports (i.e. redundant aliases) or not. But if it only applies to re-exports, that would still solve our problem.

---

_Comment by @charliermarsh on 2022-12-05 01:49_

@smackesey - #1047 implements this as `force-wrap-aliases`. For now, the rules are: enforce multi-line formatting if an import-from has more than one member, and at least one member is aliased. But I'm happy to do whatever. (We don't combine straight imports, so it doesn't touch `import os as os`.)


---

_Comment by @smackesey on 2022-12-05 02:20_

Ask and ye shall receive-- thanks!

>For now, the rules are: enforce multi-line formatting if an import-from has more than one member, and at least one member is aliased.

Sounds reasonable.

---

_Closed by @charliermarsh on 2022-12-05 02:22_

---

_Comment by @charliermarsh on 2022-12-05 02:23_

Cool, that's going out in [v0.0.158](https://github.com/charliermarsh/ruff/releases/tag/v0.0.158) (building now). Since, as far as I know, you'll be the first user of the option, let me know if you'd like it tweaked to better fit the use-case!

---
