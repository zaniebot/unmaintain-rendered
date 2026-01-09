---
number: 16187
title: uv build is different to uvx hatch build
type: issue
state: closed
author: michealroberts
labels:
  - question
assignees: []
created_at: 2025-10-08T13:16:15Z
updated_at: 2025-10-08T14:17:23Z
url: https://github.com/astral-sh/uv/issues/16187
synced_at: 2026-01-07T13:12:19-06:00
---

# uv build is different to uvx hatch build

---

_Issue opened by @michealroberts on 2025-10-08 13:16_

### Summary

I have noticed a difference between:

```bash
uv build
```

and

```bash
uvx hatch build
```

even with the following set in the project's `pyproject.toml`:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

`uv build` output:

<img width="760" height="126" alt="Image" src="https://github.com/user-attachments/assets/431fbf1d-68dd-4b8d-9840-00f5996ecaf5" />

vs

`uvx hatch build` output:

<img width="737" height="309" alt="Image" src="https://github.com/user-attachments/assets/5a3b62fe-2c9a-49dc-9fb5-39bd6cbdced9" />

### Platform

Linux 6.15.11-orbstack-00539-g9885ebd8e3f4 aarch64 GNU/Linux

### Version

uv 0.9.0

### Python version

CPython 3.13.7 interpreter at: /usr/local/bin/python3.13

---

_Label `bug` added by @michealroberts on 2025-10-08 13:16_

---

_Comment by @konstin on 2025-10-08 13:17_

What is the output with `uv pip install build && python -m build`? It looks like `hatch build` is doing something extra, likely around `.gitignore`?

---

_Comment by @michealroberts on 2025-10-08 13:40_

@konstin Thank you for replying. However, can you give me the steps please that you would like me to test your assumption and ensure the commands you want me to run also work (the above won't work).

Irregardless, I thought that `uv build` should pick up on my build backend ... ?



---

_Comment by @konstin on 2025-10-08 13:43_

What doesn't work with the above command? Please share the output as a code block.

> Irregardless, I thought that `uv build` should pick up on my build backend ... ?

It does! It's `hatch build` that does some special instead of running hatchling through the standard PEP 517 interface.

---

_Comment by @michealroberts on 2025-10-08 13:44_

> What doesn't work with the above command? Please share the output as a code block.

```
/usr/local/bin/python: No module named build
```



---

_Comment by @konstin on 2025-10-08 13:45_

In that case you need to activate the venv or run directly from the Python venv, e.g.:

```
uv venv
uv pip install build
.venv/bin/python -m build
```

---

_Comment by @michealroberts on 2025-10-08 13:49_

@konstin That gives the same output as `uv build`. It is ignoring all python files. The relevant lines in the pyproject:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/zwo"]
exclude = ["/.devcontainer", "/.github", "/.vscode", "/docs"]

[tool.hatch.build.targets.wheel.force-include]
"sdk" = "sdk"

[tool.hatch.build.targets.sdist]
packages = ["src/zwo"]
exclude = ["/.devcontainer", "/.github", "/.vscode", "/docs"]

[tool.hatch.build.targets.sdist.force-include]
"sdk" = "sdk"
```


---

_Comment by @konstin on 2025-10-08 13:51_

This means it's something that `hatch build` does extra, not a bug in uv. You should consult with hatch over the difference.

---

_Label `bug` removed by @konstin on 2025-10-08 13:51_

---

_Label `question` added by @konstin on 2025-10-08 13:51_

---

_Comment by @michealroberts on 2025-10-08 13:54_

@konstin Ok fair, I think I agree with your assessment. In my opinion, `uvx hatch build` has built the output correctly, the package is also now installing properly on the receiving side:

<img width="135" height="39" alt="Image" src="https://github.com/user-attachments/assets/f052a3ab-db0e-4ed2-9d46-495f2e80df71" />

... whereas before just the sdist files were being installed minus the wheel.

Two different outcomes with the same configuration in my `pyproject.toml`.

cc @ofek FYI 👀 we have a discrepancy with hatch build processes.

---

_Closed by @michealroberts on 2025-10-08 14:17_

---
