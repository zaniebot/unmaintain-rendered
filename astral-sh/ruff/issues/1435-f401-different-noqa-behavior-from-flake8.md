```yaml
number: 1435
title: F401 different noqa behavior from flake8
type: issue
state: closed
author: TylerYep
labels: []
assignees: []
created_at: 2022-12-29T07:00:18Z
updated_at: 2022-12-29T12:23:09Z
url: https://github.com/astral-sh/ruff/issues/1435
synced_at: 2026-01-12T15:54:41Z
```

# F401 different noqa behavior from flake8

---

_@TylerYep_

I have the following code, which contains intentionally unused imports (used for bringing fixtures into pytest scope)

In flake8, I am able to ignore all unused imports by putting a `# noqa` on the first line. However, ruff checking the same code errors and wants me to add # noqa to every line:

```
from tests.fixtures import (  # noqa
    extra_small,
    small,
    medium,
    large,
    xxl,
    super_super_super_mega_xxl,
)
```

```
tests/conftest.py:2:5: F401 `tests.fixtures.extra_small` imported but unused
tests/conftest.py:3:5: F401 `tests.fixtures.small` imported but unused
tests/conftest.py:4:5: F401 `tests.fixtures.medium` imported but unused
tests/conftest.py:5:5: F401 `tests.fixtures.large` imported but unused
tests/conftest.py:6:5: F401 `tests.fixtures.xxl` imported but unused
tests/conftest.py:7:5: F401 `tests.fixtures.super_super_super_mega_xxl` imported but unused
Found 6 error(s).
6 potentially fixable with the --fix option.
```

The code passes in both flake8 and ruff if I do this, but it's a bit verbose:
```
from tests.fixtures import (
    extra_small,  # noqa
    small,  # noqa
    medium,  # noqa
    large,  # noqa
    xxl,  # noqa
    super_super_super_mega_xxl,  # noqa
)
```

Wanted to call out the difference in behavior here.

---

_Comment by @charliermarsh on 2022-12-29 12:23_

Thank you for this! It's the one known point of `# noqa` incompatibility. I want to fix, but it requires some special-casing.

(Just closing as it's a duplicate of #1052.)

---

_Closed by @charliermarsh on 2022-12-29 12:23_

---
