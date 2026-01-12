```yaml
number: 3008
title: support trailing whitespace (W291) and blank lines contains whitespace (W293)
type: issue
state: closed
author: kapilt
labels: []
assignees: []
created_at: 2023-02-18T12:57:13Z
updated_at: 2023-02-19T00:39:29Z
url: https://github.com/astral-sh/ruff/issues/3008
synced_at: 2026-01-12T15:54:43Z
```

# support trailing whitespace (W291) and blank lines contains whitespace (W293)

---

_@kapilt_

Hi folks, ruff is great, we just switched a project that was using flake8 over to ruff, its unfortunately still in progress on converting to black (lots of extant prs re spurious conflicts re delay)

one challenge is the lack of whitespace linting, both for trailing and empty lines. I see some previous closed tickets on this but it's also clear it's not been implemented, https://github.com/charliermarsh/ruff/issues/1734  . It would still be super useful to have builtin, I'm fine with skipping the trailing whitespace in multi line strings.

❯ ruff --version
ruff 0.0.24



---

_Comment by @ngnpope on 2023-02-18 18:01_

Duplicate of #2402.

---

_Comment by @charliermarsh on 2023-02-19 00:39_

On the roadmap! But I want to keep tracking these via #2402. (Thank you for trying stuff :))

---

_Closed by @charliermarsh on 2023-02-19 00:39_

---
