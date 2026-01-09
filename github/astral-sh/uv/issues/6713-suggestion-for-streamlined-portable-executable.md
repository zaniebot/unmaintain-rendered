---
number: 6713
title: Suggestion for streamlined portable executable script
type: issue
state: closed
author: taranlu-houzz
labels: []
assignees: []
created_at: 2024-08-27T18:52:23Z
updated_at: 2024-09-19T18:57:15Z
url: https://github.com/astral-sh/uv/issues/6713
synced_at: 2026-01-07T13:12:17-06:00
---

# Suggestion for streamlined portable executable script

---

_Issue opened by @taranlu-houzz on 2024-08-27 18:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I saw a twitter post mentioned by @charliermarsh where someone suggested using `#!/usr/bin/env -S uv run` to run a script. Apparently, `env -S` is not portable, so I was looking around for other more portable options. This is what I have come to (suggested by Copilot):

```python
#!/bin/sh
# fmt: off
'''exec' uv run -q "$0" "$@"
' '''
# fmt: on
```

The logic seems sound, and I added the `fmt: off` guard around it prevents `ruff` from auto-formatting it. It works on my macOS machine, but I haven't tested it yet on Linux. Assuming it does work, it seems like maybe it could be a workflow/tip that could be mentioned in the docs?

One part of the UX that could be improved is the use of `-q` to prevent `uv` from saying "Reading inline script metadata from:" I want to hide that so that it feels like I am just calling the script directly, but I would actually like it to have output the first time if it needs to download something (e.g. it has a dep on a large package that is not cached).

---

_Comment by @taranlu-houzz on 2024-08-27 19:03_

Would also be interesting to see this combined with a tool that is able to take a python project and merge all of it into a single file (I'm pretty sure there was a `xonsh` related project that does something like that).

---

_Comment by @bluss on 2024-08-27 19:06_

That should work on linux, this technique is already used in one odd corner, see https://github.com/astral-sh/uv/issues/6489#issuecomment-2308550586

---

_Closed by @taranlu-houzz on 2024-09-19 18:57_

---
