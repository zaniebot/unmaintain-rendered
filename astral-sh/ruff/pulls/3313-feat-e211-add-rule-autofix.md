```yaml
number: 3313
title: "feat(E211): add rule + autofix"
type: pull_request
state: merged
author: carlosmiei
labels:
  - rule
assignees: []
merged: true
base: main
head: e211
created_at: 2023-03-02T21:58:48Z
updated_at: 2023-10-31T14:30:45Z
url: https://github.com/astral-sh/ruff/pull/3313
synced_at: 2026-01-10T23:40:55Z
```

# feat(E211): add rule + autofix

---

_Pull request opened by @carlosmiei on 2023-03-02 21:58_

- https://www.flake8rules.com/rules/E211.html

---

_Review comment by @carlosmiei on `crates/ruff/src/checkers/logical_lines.rs`:157 on 2023-03-02 22:05_

~~will probably move this check inside `whitespace_before_parameters` because the rule does not exist without the `logical_lines` feature~~ ended up using a directive here

---

_@carlosmiei reviewed on 2023-03-02 22:05_

---

_Label `rule` added by @charliermarsh on 2023-03-02 22:45_

---

_Merged by @charliermarsh on 2023-03-02 22:48_

---

_Closed by @charliermarsh on 2023-03-02 22:48_

---

_Comment by @chaitanya2692 on 2023-10-31 13:27_

Here's a test file where E211 seems to fail

```
from typing import Union
import sys

from logging import Logger


def fetch_name () -> Union[str, None]:
    """Fetch name from --person-name in sys.argv.

   Returns:
        name of the person if available, otherwise None
    """
    test = len(5)
    Logger.info(test)
    # test commented code
    # Logger.info("test code")
    for i in range (0, len (sys.argv)) :
        if sys.argv[i] == "--name" :
            return sys.argv[i + 1]
    return None
```
    
ruff v0.1.3 doesn't show any errors.
    
Here's my ruff.toml

```
line-length = 88

[lint]
preview = true
select = ["E", "F", "B", "W", "I", "PL", "C90", "RUF"]
ignore = ["E203","E231"]

[mccabe]
max-complexity = 10
```


---

_Comment by @charliermarsh on 2023-10-31 14:30_

Thanks @chaitanya2692, filed and fixed this as #8379.

---
