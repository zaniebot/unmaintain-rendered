```yaml
number: 5279
title: "Allow using `uv venv` with `--format=json`"
type: issue
state: open
author: InSyncWithFoo
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-07-22T04:05:37Z
updated_at: 2025-06-13T12:11:08Z
url: https://github.com/astral-sh/uv/issues/5279
synced_at: 2026-01-12T15:58:55Z
```

# Allow using `uv venv` with `--format=json`

---

_@InSyncWithFoo_

This is what `uv venv` currently outputs on Windows:

```text
Using Python 3.12.4 interpreter at: ...\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate
```

It is good for human consumption, but not so much for machines. I propose that `uv venv` outputs a JSON object in the following format when used with `--format=json`:

```typescript
interface VenvOutput {
  python: string;
  venv: string;
  activateCommand: string;
}
```

Example output:

```json
{
  "python": "...\\python.exe",
  "venv": ".venv",
  "activateCommand": ".venv\\Scripts\\activate"
}
```

This is a subtask of #3199.

---

_Label `enhancement` added by @charliermarsh on 2024-07-22 19:17_

---

_Label `cli` added by @charliermarsh on 2024-07-22 19:17_

---

_Comment by @zanieb on 2025-06-13 12:11_

Part of https://github.com/astral-sh/uv/issues/411

---
