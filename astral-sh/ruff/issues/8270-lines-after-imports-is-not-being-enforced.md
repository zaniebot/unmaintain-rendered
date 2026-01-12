```yaml
number: 8270
title: lines-after-imports is not being enforced correctly on 0.1.3
type: issue
state: closed
author: qbedard
labels:
  - question
assignees: []
created_at: 2023-10-27T06:30:27Z
updated_at: 2023-10-28T22:54:04Z
url: https://github.com/astral-sh/ruff/issues/8270
synced_at: 2026-01-12T15:54:48Z
```

# lines-after-imports is not being enforced correctly on 0.1.3

---

_@qbedard_

Ruff version: 0.1.3

ruff.toml:
```toml
[isort]
lines-after-imports = 2
```

Python file:
```python
import os
print(os.environ["SHELL"])
```

Expected:
```python
import os


print(os.environ["SHELL"])
```

Actual:
```python
import os

print(os.environ["SHELL"])
```

In fact, it seems that the lines after the imports remain untouched if there are 1 or 2 lines and are only altered from 0 -> 1 and >2 -> 2.

With `lines-after-imports = 1`, it should always be 1 line.

With `lines-after-imports = 2`, it should always be 2 lines.

🙏

---

_Comment by @dhruvmanila on 2023-10-27 07:58_

Hey, I'm unable to reproduce this locally. Can you tell us how are you running Ruff? Are you using the CLI / VSCode extension / any other editor with ruff-lsp?

---

_Label `question` added by @dhruvmanila on 2023-10-27 07:59_

---

_Comment by @qbedard on 2023-10-27 15:25_

> Hey, I'm unable to reproduce this locally. Can you tell us how are you running Ruff? Are you using the CLI / VSCode extension / any other editor with ruff-lsp?

Via CLI.

I think I've figured it out though. The lines are only adjusted when running `ruff --fix`, not when running `ruff format`. That's a little surprising.

First thought, best thought: Shouldn't running `ruff format` also rope in the isort functionality? I understand the history of how we got here (integrating isort functionality before `ruff format` was around), but the end of the day, isort is primarily a formatter as well, so it's bit odd that only black functionality is tied to `ruff format` while isort is relegated to `ruff --fix`.

Thoughts?

---

_Comment by @MichaReiser on 2023-10-27 15:46_

We agree that `isort` is the weird one now and we are considering pulling it out of linting. Whether it will be part of formatting or entirely its own command, we don't know yet. 

---

_Comment by @qbedard on 2023-10-28 22:54_

> We agree that `isort` is the weird one now and we are considering pulling it out of linting. Whether it will be part of formatting or entirely its own command, we don't know yet.

Cool, cool. I'll close this issue and keep an eye out for changes down the line.

---

_Closed by @qbedard on 2023-10-28 22:54_

---
