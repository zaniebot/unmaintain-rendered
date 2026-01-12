```yaml
number: 13425
title: "allow `--managed-python` to be used as arg when subprocesses"
type: issue
state: open
author: KRRT7
labels:
  - enhancement
assignees: []
created_at: 2025-05-13T07:09:38Z
updated_at: 2025-05-13T07:09:38Z
url: https://github.com/astral-sh/uv/issues/13425
synced_at: 2026-01-12T16:01:28Z
```

# allow `--managed-python` to be used as arg when subprocesses

---

_@KRRT7_

### Summary

when subprocessing `uv` / `uvx` --managed-python isn't recognized as an argument

![Image](https://github.com/user-attachments/assets/1c38e072-004c-4b40-8298-f98d330fab04)

### Example

used like so:
```py
        cmd = [
            "uvx",
            "--managed-python",
            "--refresh",
            "--isolated",
            "--with-requirements",
            deps.temp_requirements().as_posix(),
            "nuitka",
            "--remove-output",
            "--onefile",
            "--run",
            self.app.script,
        ]
        self.executer = await asyncio.create_subprocess_shell(
            " ".join(cmd),
            stdout=asyncio.subprocess.PIPE,
            stderr=asyncio.subprocess.STDOUT,
        )
```

---

_Label `enhancement` added by @KRRT7 on 2025-05-13 07:09_

---
