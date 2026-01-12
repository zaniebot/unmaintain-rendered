```yaml
number: 3375
title: Include entire prefix when reporting rule selector errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/cli
created_at: 2023-03-06T23:43:07Z
updated_at: 2023-03-07T00:04:54Z
url: https://github.com/astral-sh/ruff/pull/3375
synced_at: 2026-01-12T15:55:12Z
```

# Include entire prefix when reporting rule selector errors

---

_@charliermarsh_

This is a little more intuitive:

```shell
❯ cargo run -p ruff_cli -- foo.py --select BUG
    Finished dev [unoptimized + debuginfo] target(s) in 0.34s
     Running `target/debug/ruff foo.py --select BUG`
error: invalid value 'BUG' for '--select <RULE_CODE>': Unknown rule selector: `BUG`

For more information, try '--help'.
```

It also now looks like a proper Clippy error:

![Screen Shot 2023-03-06 at 7 00 21 PM](https://user-images.githubusercontent.com/1309177/223284010-ba0f242c-e3a4-4a3b-8e2a-9c19b38ac26b.png)

Closes #3374.

---

_Label `bug` added by @charliermarsh on 2023-03-06 23:43_

---

_Label `cli` added by @charliermarsh on 2023-03-06 23:43_

---

_Merged by @charliermarsh on 2023-03-07 00:04_

---

_Closed by @charliermarsh on 2023-03-07 00:04_

---

_Branch deleted on 2023-03-07 00:04_

---
