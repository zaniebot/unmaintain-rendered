```yaml
number: 8468
title: Add TRIO106 rule
type: pull_request
state: closed
author: karpetrosyan
labels: []
assignees: []
base: main
head: add-trio-106-support
created_at: 2023-11-03T12:49:35Z
updated_at: 2023-11-04T13:35:01Z
url: https://github.com/astral-sh/ruff/pull/8468
synced_at: 2026-01-12T15:55:26Z
```

# Add TRIO106 rule

---

_@karpetrosyan_

## Summary

This PR is a part of #8451 and adds TRIO106 rule.

Examples

Bad
```python
import trio as asyncio
```

Bad
```python
from trio import move_on_after
````

Good
```python
import trio
```



---

_Comment by @github-actions[bot] on 2023-11-03 13:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2023-11-03 15:05_

I'm not sure this limitation exists in Ruff. I believe we can detect the renamed import when determining if `trio` is being used.

---

_Comment by @charliermarsh on 2023-11-03 15:39_

Yeah we can actually detect `from trio import foo as bar`, `import trio as foo`, etc.

---

_Comment by @karpetrosyan on 2023-11-03 23:18_

> I'm not sure this limitation exists in Ruff. I believe we can detect the renamed import when determining if `trio` is being used.

I am not sure I understand what you mean here.

---

_Comment by @charliermarsh on 2023-11-03 23:50_

@karpetrosyan - It seems like this rule exists in flake8-trio because flake8-trio needs `trio` to be imported as `trio` -- as in, `import trio` rather than `import trio as foo`.

In Ruff, we abstract that away. Whenever you use `resolve_call_path`, we resolve aliases. So if the user does `import trio as foo; foo.do_trio_thing()`, we'd understand that `foo.do_trio_thing()` is a reference to `trio.do_trio_thing`.

In other words, it seems like the limitation that exists in flake8-trio doesn't apply to Ruff, and so the rule wouldn't be needed. Does that make sense?

---

_Comment by @karpetrosyan on 2023-11-04 00:06_

Yes, that makes perfect sense. Thanks @charliermarsh !

I am new to Ruff, so sorry if this PR was useless. Do I need to close it?

---

_Comment by @charliermarsh on 2023-11-04 13:35_

@karpetrosyan - Not at all, no need to apologize! On the contrary, I'm sorry that you invested time in it. Yes, we can close the PR, since the rule doesn't seem relevant here.

---

_Closed by @charliermarsh on 2023-11-04 13:35_

---
