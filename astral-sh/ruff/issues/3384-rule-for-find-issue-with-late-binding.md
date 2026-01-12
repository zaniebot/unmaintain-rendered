```yaml
number: 3384
title: Rule for find issue with late binding
type: issue
state: closed
author: worldmind
labels:
  - question
assignees: []
created_at: 2023-03-07T12:06:15Z
updated_at: 2023-03-07T18:59:37Z
url: https://github.com/astral-sh/ruff/issues/3384
synced_at: 2026-01-12T15:54:43Z
```

# Rule for find issue with late binding

---

_@worldmind_

Thank you for amazing project!

Does ruff has rule for check [issue with late binding](https://docs.python-guide.org/writing/gotchas/#late-binding-closures)?


---

_Comment by @charliermarsh on 2023-03-07 14:53_

Yeah!

Given:

```py
def create_multipliers():
    return [lambda x : i * x for i in range(5)]
```

Running Ruff shows:

```
❯ ruff --select B foo.py
foo.py:2:24: B023 Function definition does not bind loop variable `i`
Found 1 error.
```

(So you just need to enable the flake8-bugbear rules with `--select B`.)

---

_Closed by @charliermarsh on 2023-03-07 14:53_

---

_Label `question` added by @charliermarsh on 2023-03-07 14:53_

---

_Comment by @worldmind on 2023-03-07 17:12_

Cool, thank you again! It's really amazing how fast ruff works and how many rules you have implemented!
Really nice idea to have one tool for all kind of static analysis.

---

_Comment by @charliermarsh on 2023-03-07 18:59_

Thank you! :)

---
