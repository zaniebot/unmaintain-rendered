```yaml
number: 8874
title: "`# fmt: skip` should have comments after the `skip` keyword"
type: issue
state: closed
author: georgettica
labels:
  - suppression
  - formatter
assignees: []
created_at: 2023-11-28T19:00:50Z
updated_at: 2023-12-13T09:53:41Z
url: https://github.com/astral-sh/ruff/issues/8874
synced_at: 2026-01-12T15:54:48Z
```

# `# fmt: skip` should have comments after the `skip` keyword

---

_@georgettica_

`# fmt: skip` should have a comment section to allow developers to explain why they don't want to format

my motives are to allow partial grouping of array inputs.

data that was requested:
---

python file:
```
commands_to_run = [
    "./command.sh",
    "-n", "5",
    "-v",
]  # fmt: skip - reason for skipping 
```

command:
```
ruff format --isolated <file.py>
```

output:
```
list_of_items = [
    "./command.sh",
    "-n",
    "5",
    "-v",
]  # fmt: skip - reason for skipping 
```

```
$ ruff --version
ruff 0.1.6
```

---

_Label `noqa` added by @zanieb on 2023-11-28 19:07_

---

_Label `formatter` added by @zanieb on 2023-11-28 19:07_

---

_Comment by @MichaReiser on 2023-11-29 01:41_

Related to https://github.com/astral-sh/ruff/issues/8892

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 01:41_

---

_Comment by @georgettica on 2023-12-13 09:53_

as a duplicate, I will close it in favor of #8892 

---

_Closed by @georgettica on 2023-12-13 09:53_

---
