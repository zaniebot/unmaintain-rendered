---
number: 6843
title: "Add `constraints.txt` export to `uv export`"
type: issue
state: open
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2024-08-30T00:19:23Z
updated_at: 2024-08-31T17:04:46Z
url: https://github.com/astral-sh/uv/issues/6843
synced_at: 2026-01-10T01:24:06Z
---

# Add `constraints.txt` export to `uv export`

---

_Issue opened by @charliermarsh on 2024-08-30 00:19_

Constraints files are a little different... Specifically, every requirement has to be named, so we can't write relative paths. (You also can't include extras, but we already don't do that.)


---

_Label `cli` added by @charliermarsh on 2024-08-30 00:19_

---

_Comment by @zanieb on 2024-08-30 01:40_

Hm. An export of the `pyproject.toml` constraints?

---

_Comment by @charliermarsh on 2024-08-30 02:15_

No, it’s an export of the lockfile in constraints.txt format. requirements.txt files support things that constraints files do not. You can’t export in requirements.txt format and pass it as constraints.

---

_Comment by @carderne on 2024-08-31 11:54_

I've had this line in every rye-based Dockerfile for the last few years to work around this.
```Dockerfile
RUN sed -i '/^-e ./d' requirements.txt
```

---

_Comment by @charliermarsh on 2024-08-31 16:24_

@carderne -- If you structure your project as a non-package (i.e., omit a `[build-system]` or set `package = false` under `[tool.uv]`), that line should be omitted from the export.

---

_Comment by @carderne on 2024-08-31 17:04_

Sorry maybe I crossed a wire. To be super explicit, this works:
```bash
uv init --package foo
cd foo
uv sync
uv run python -c 'import foo'
```

But if I set `tool.uv.package = false` then I obviously can't `import foo`.

I want `foo` to be installed in the venv, but I don't want it in the constraints file, as pip rejects that.

(Happy to have a thumbs up and go on my way, I might be derailing the point of this thread.)

---
