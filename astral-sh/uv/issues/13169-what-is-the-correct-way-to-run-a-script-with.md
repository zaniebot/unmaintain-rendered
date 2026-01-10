---
number: 13169
title: What is the correct way to run a script with declared dependencies alongside a module?
type: issue
state: closed
author: TuringTrails
labels:
  - question
assignees: []
created_at: 2025-04-28T08:41:20Z
updated_at: 2025-05-04T12:40:57Z
url: https://github.com/astral-sh/uv/issues/13169
synced_at: 2026-01-10T01:25:29Z
---

# What is the correct way to run a script with declared dependencies alongside a module?

---

_Issue opened by @TuringTrails on 2025-04-28 08:41_

### Question

I’m trying to debug a Python script using pdb, where the script declares its dependencies internally (e.g., via a comment block in toml). However, when I run it with:
`uv run -m pdb myscript.py`
the script fails on the import statements — it seems the dependencies are not being resolved.

## Alternatives I’ve considered
1. Running:
    `uv run --with 'dependency1 dependency2' -m pdb myscript.py`
    This works, but becomes tedious when there are many dependencies to list manually.
2. Adding import pdb; pdb.set_trace() in the code. This also works, but I’m looking for a way to use pdb without modifying the script itself.

Is there a recommended way to load modules like pdb while still allowing uv to resolve the dependencies defined in the script?

Thank you!

### Platform

NixOS 24.11.20250412.26d499f (Vicuna) - Linux 6.6.87 x86_64 GNU/Linux

### Version

uv 0.4.30

---

_Label `question` added by @TuringTrails on 2025-04-28 08:41_

---

_Comment by @konstin on 2025-04-28 13:29_

You can use e.g. `uv sync --script script.py` and then run `python -m pdb script.py`. uv can't see beyond the `-m pdb` and therefore can't tell that this is script from which the dependencies need to installed, so `uv run -m pdb` doesn't work.

---

_Comment by @TuringTrails on 2025-05-01 13:04_

Thanks 

However `uv sync --script script.py` is not working for me. I get the error:
`error: unexpected argument '--script' found`

I have multiple scripts in the same folder, as such i did not explore this line of thinking. 

are there any other recommended ways.


---

_Comment by @konstin on 2025-05-02 07:27_

You need to update your uv version for that, the current version is 0.7.2.

---

_Closed by @charliermarsh on 2025-05-04 12:40_

---
