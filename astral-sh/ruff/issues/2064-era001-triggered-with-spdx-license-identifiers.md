```yaml
number: 2064
title: ERA001 triggered with SPDX license identifiers
type: issue
state: closed
author: jdknight
labels: []
assignees: []
created_at: 2023-01-21T17:34:25Z
updated_at: 2023-01-21T18:46:05Z
url: https://github.com/astral-sh/ruff/issues/2064
synced_at: 2026-01-10T11:09:45Z
```

# ERA001 triggered with SPDX license identifiers

---

_Issue opened by @jdknight on 2023-01-21 17:34_

Have a project which uses `SPDX-License-Identifier` definitions at the top of each source file. A minimized example file (`__init__.py`) is as follows:

```
# SPDX-License-Identifier: BSD-2-Clause

def test():
    print('hello')
    return 0
```

This causes an `ERA001` error to be generated:

```
$ ruff __main__.py
__init__.py:1:1: ERA001 Found commented-out code
Found 1 error(s).
1 potentially fixable with the --fix option.
```
---

Other notes:

- `ruff --version` reports `ruff 0.0.228`
- Using `eradicate` or `flake8-eradicate` does not generate an error
- Removing the colon will prevent the error, but breaks the format of the [SPDX ID](https://spdx.dev/ids/)
- Appending `  # noqa: ERA001` will prevent the error, but results in an invalid SPDX ID value
- Existing workaround is to ignore `ERA001`

---

_Comment by @charliermarsh on 2023-01-21 17:49_

I'm not certain _why_ that's being detected, but you can instruct the plugin to ignore comments of that form with:

```toml
[tool.ruff]
task-tags = ["SPDX-License-Identifier"]
```

(We use that abstraction to filter out comments like `# TODO: ...`, etc.)

Just tested it out myself, so gonna close for now, but let me know if it doesn't solve the problem for you!

---

_Closed by @charliermarsh on 2023-01-21 17:49_

---

_Comment by @jdknight on 2023-01-21 18:46_

Thanks! Adding the identifier to the `task-tags` option removes the errors for me.

---
