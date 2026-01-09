---
number: 10014
title: The option lines-after-imports does not work after if block
type: issue
state: open
author: BranislavBajuzik
labels:
  - isort
assignees: []
created_at: 2024-02-17T00:02:19Z
updated_at: 2024-02-25T09:23:16Z
url: https://github.com/astral-sh/ruff/issues/10014
synced_at: 2026-01-07T13:12:15-06:00
---

# The option lines-after-imports does not work after if block

---

_Issue opened by @BranislavBajuzik on 2024-02-17 00:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hello. We have recently decided to move to Ruff and while going over the diff I noticed a small inconsistency:
```
> ruff --version
ruff 0.2.1
```
Full Config:
```toml
[lint]
select = ["I"]
[lint.isort]
lines-after-imports = 2
```
Code snippet:
```python
from random import random
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from random import Random
print(random)
```
Command:
```shell
ruff report.py --config report.toml --fix
```
Expected output:
```python
from random import random
from typing import TYPE_CHECKING


if TYPE_CHECKING:
    from random import Random


print(random)
```
Actual output:
```python
from random import random
from typing import TYPE_CHECKING


if TYPE_CHECKING:
    from random import Random
print(random)
```

What would be even better than the "expected", is if Ruff could treat the `TYPE_CHECKING` block as just another section and thus ignore the `lines-after-imports` and use only one line to separate the `TYPE_CHECKING` block

Thank you for your time and this project

---

_Label `isort` added by @MichaReiser on 2024-02-17 17:14_

---

_Comment by @MichaReiser on 2024-02-23 21:15_

Thanks for reporting this issue. I've also been surprised today that `isort` does not enforce any blank lines inside of nested blocks but that seems intentional 

https://github.com/astral-sh/ruff/blob/727e389cac222e64c6f89ea1341f99ee54109b9d/crates/ruff_linter/src/rules/isort/block.rs#L71-L74

@charliermarsh do you know what the motivation is behind this?

You can either use our formatter or the `E3*` rules (preview) if you want to enforce blank lines at the top level. 

---

_Comment by @BranislavBajuzik on 2024-02-24 20:47_

I turned on all of the E3* (one-by-one) rules, but there are still no blank lines after the if block. 

I use the ruff pre-commit hook, in the "standard" configuration, where I call the `ruff` hook and then the `ruff-format` hook. this still leaves 0, 1, or 2 lines after that block. I apologize for not stating this, as I only copy-pasted the hook config and was thinking of ruff as having one entry point.

This is not an issue, we can live with the amount of blank lines we get. This also happens only with top-level statements (mostly `log = ...`), not function/class definitions.

---

_Comment by @MichaReiser on 2024-02-25 09:23_

@BranislavBajuzik sorry that was my mistake. I forgot that the `E3` rules only enforce blank lines between functions and classes but not between other statements. 

Let's see what @charliermarsh opinion is on this



---
