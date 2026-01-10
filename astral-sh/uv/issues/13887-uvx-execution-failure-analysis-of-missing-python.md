---
number: 13887
title: uvx Execution Failure — Analysis of Missing Python Interpreter Path in Startup Script
type: issue
state: closed
author: wang-qijia
labels:
  - question
assignees: []
created_at: 2025-06-06T15:19:28Z
updated_at: 2025-06-10T05:30:46Z
url: https://github.com/astral-sh/uv/issues/13887
synced_at: 2026-01-10T01:25:39Z
---

# uvx Execution Failure — Analysis of Missing Python Interpreter Path in Startup Script

---

_Issue opened by @wang-qijia on 2025-06-06 15:19_

### Question

When running the MCP Client integration or standalone, execution fails with a missing Python interpreter error.

Using the following commands:

```bash
uv tool install mcp_clickhouse
uvx mcp-clickhouse
````

The expected behavior is that the program runs without startup errors after installation.

However, the following error occurs when running `uvx mcp-clickhouse`:

```
/.../.cache/uv/archive-v0/.../bin/mcp-clickhouse: line 2: /Users/username/.local/bin/python: No such file or directory
/.../.cache/uv/archive-v0/.../bin/mcp-clickhouse: line 2: exec: /Users/username/.local/bin/python: line 5: exec: //python: cannot execute: No such file or directory
```

Inspecting the generated script `/.../.cache/uv/archive-v0/.../bin/mcp-clickhouse` shows that line 2 attempts to exec Python from `/Users/username/.local/bin/python`, which does not exist on the system.

---

### Platform

* ProductName: macOS
* ProductVersion: 11.7.8
* BuildVersion: 20G1351

---

### Version

* uv 0.7.8 (commit 0ddcc1905, 2025-05-23)

---

If additional logs or environment details are needed, please let me know.

```
```


---

_Label `question` added by @wang-qijia on 2025-06-06 15:19_

---

_Comment by @konstin on 2025-06-06 15:22_

Does clearing the cache and reinstalling the tool fix the problem?

---

_Comment by @wang-qijia on 2025-06-06 15:26_

After running `uv cache clean`, the issue still persists.


---

_Comment by @konstin on 2025-06-06 15:28_

It seems that `/Users/username/.local/bin/python` is broken in some way, maybe the underlying interpreter was uninstalled. A fresh tool installation shouldn't pick that interpreter up anymore. Alternatively, you can remove `/Users/username/.local/bin/python` manually and reinstall.

---

_Comment by @wang-qijia on 2025-06-06 15:32_

My environment is as follows, and everything appears to be normal.

Additionally, running uv tool install mcp_clickhouse before execution allows the program to run correctly.

```
wanqijia@wanqideMacBook-Pro /usr % python3
Python 3.12.0 (v3.12.0:0fb18b02c8, Oct  2 2023, 09:45:56) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> c
zsh: suspended  python3
wanqijia@wanqideMacBook-Pro /usr % uv -V
uv 0.7.8 (0ddcc1905 2025-05-23)

```

---

_Comment by @konstin on 2025-06-06 15:39_

It seems to be specifically the `~/.local/bin/python` file that is broken.

---

_Closed by @wang-qijia on 2025-06-10 05:30_

---
