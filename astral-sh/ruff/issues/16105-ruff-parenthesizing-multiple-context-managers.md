```yaml
number: 16105
title: "Ruff parenthesizing multiple context managers despite `requires-python = \">=3.8\"`"
type: issue
state: closed
author: tamird
labels:
  - configuration
  - cli
assignees: []
created_at: 2025-02-11T21:20:58Z
updated_at: 2025-02-11T21:51:33Z
url: https://github.com/astral-sh/ruff/issues/16105
synced_at: 2026-01-10T11:09:57Z
```

# Ruff parenthesizing multiple context managers despite `requires-python = ">=3.8"`

---

_Issue opened by @tamird on 2025-02-11 21:20_

### Description

ruff 0.9.6

Seen in https://github.com/mystor/git-revise/pull/141.

If I just have:

```toml
[project]
name = "git-revise"
dynamic = ["version"]
requires-python = ">=3.8"
authors = [{ name = "Nika Layzell", email = "nika@thelayzells.com" }]
description = "Efficiently update, split, and rearrange git commits"
readme = "README.md"
license = { file = "LICENSE" }
keywords = ["git", "revise", "rebase", "amend", "fixup"]
classifiers = [
  "Development Status :: 4 - Beta",
  "Intended Audience :: Developers",
  "Environment :: Console",
  "Topic :: Software Development :: Version Control",
  "Topic :: Software Development :: Version Control :: Git",
  "License :: OSI Approved :: MIT License",
  "Programming Language :: Python :: 3",
  "Programming Language :: Python :: 3.8",
  "Programming Language :: Python :: 3.9",
  "Programming Language :: Python :: 3.10",
]
```
and I run `uv run ruff format` I get:
```diff
diff --git a/tests/conftest.py b/tests/conftest.py
index 98350a2..47f70df 100644
--- a/tests/conftest.py
+++ b/tests/conftest.py
@@ -126,7 +126,11 @@ def editor_main(
     # pylint: disable=redefined-builtin
     input: Optional[bytes] = None,
 ) -> "Generator[Editor, None, subprocess.CompletedProcess[bytes]]":
-    with pytest.MonkeyPatch().context() as monkeypatch, Editor() as ed, ThreadPoolExecutor() as tpe:
+    with (
+        pytest.MonkeyPatch().context() as monkeypatch,
+        Editor() as ed,
+        ThreadPoolExecutor() as tpe,
+    ):
         host, port = ed.server_address
         host = host.decode() if isinstance(host, (bytes, bytearray)) else host
         editor_cmd = " ".join(

```

which is not valid in python 3.8.

However if I add
```toml

[tool.ruff]
target-version = "py38"
```

then ruff no longer suggests this edit.

---

_Comment by @MichaReiser on 2025-02-11 21:32_

Thanks for the detailed writeup. 

Can you try running ruff with:

```
uvx ruff check --verbose other.py
[2025-02-11][22:31:05][ruff::resolve][DEBUG] Using Ruff default settings
[2025-02-11][22:31:05][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2025-02-11][22:31:05][ruff::commands::check][DEBUG] Identified files to lint in: 3.22325ms
[2025-02-11][22:31:05][ruff::commands::check][DEBUG] Checked 1 files in: 37.458µs
```

You can see from the log that ruff ignores the `pyproject.toml` (I'm using your configuration in this example). Now, adding a `[tool.ruff]` section to your configuration should make Ruff to pick up the `pyproject.toml`

```
uvx ruff check --verbose other.py
[2025-02-11][22:33:50][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/micha/astral/test/pyproject.toml
[2025-02-11][22:33:50][ruff_linter::settings::types][DEBUG] Detected minimum supported `requires-python` version: 3.8
[2025-02-11][22:33:50][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2025-02-11][22:33:50][ruff::commands::check][DEBUG] Identified files to lint in: 3.301625ms
[2025-02-11][22:33:50][ruff::commands::check][DEBUG] Checked 1 files in: 53.875µs
```

I'm not quiet sure why Ruff ignores `pyproject.toml` files without a `tool.ruff` section by default, even if it doesn't find any other configuration in any parent directory. 

---

_Label `question` added by @MichaReiser on 2025-02-11 21:32_

---

_Comment by @tamird on 2025-02-11 21:39_

Without `[tool.ruff]`:
```
uvx ruff check --verbose 2>&1 | grep ruff::resolve
[2025-02-11][16:38:28][ruff::resolve][DEBUG] Using Ruff default settings
```

With empty `[tool.ruff]`:
```
tamird@Tamirs-MacBook-Pro git-revise % uvx ruff check --verbose 2>&1 | grep ruff::resolve
[2025-02-11][16:39:09][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/tamird/src/git-revise/pyproject.toml
```

So I guess the solution to my problem is an empty `[tool.ruff]`, but this certainly seems like a bug.

---

_Label `question` removed by @MichaReiser on 2025-02-11 21:49_

---

_Label `configuration` added by @MichaReiser on 2025-02-11 21:49_

---

_Label `cli` added by @MichaReiser on 2025-02-11 21:49_

---

_Closed by @MichaReiser on 2025-02-11 21:51_

---
