```yaml
number: 1424
title: Provide example of file based package install.
type: pull_request
state: merged
author: dylanbstorey
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-02-16T04:17:50Z
updated_at: 2024-03-22T21:24:57Z
url: https://github.com/astral-sh/uv/pull/1424
synced_at: 2026-01-10T14:49:08Z
```

# Provide example of file based package install.

---

_Pull request opened by @dylanbstorey on 2024-02-16 04:17_

Added example to install packages from a file w/o editable mode.

I use `pip install .` in a number of CI/CD and build scripts - it wasn't obvious to me how to achieve this with uv as  `uv pip install .` throws an error about package names being expected to start with an alphanumeric character. 




---

_@zanieb reviewed on 2024-02-16 04:33_

---

_Review comment by @zanieb on `README.md`:72 on 2024-02-16 04:33_

I think `file://` shouldn't be needed, should we omit it?

---

_Comment by @zanieb on 2024-02-16 04:34_

Thank you! xref #313 

---

_Label `documentation` added by @zanieb on 2024-02-16 04:34_

---

_@dylanbstorey reviewed on 2024-02-16 04:38_

---

_Review comment by @dylanbstorey on `README.md`:72 on 2024-02-16 04:38_

oh i hadn't tried that yet , it works !

---

_@zanieb reviewed on 2024-02-16 04:58_

---

_Review comment by @zanieb on `README.md`:72 on 2024-02-16 04:58_

Should we separate so it's easier to read? (honestly not sure) cc @charliermarsh 

```suggestion
uv pip install "package @ ."        # Install the current project from disk
```

---

_Review comment by @zanieb on `README.md`:72 on 2024-02-16 05:00_

I'm a little worried about people thinking they should do this instead of `-e .` (more common)

---

_@zanieb reviewed on 2024-02-16 05:00_

---

_@dylanbstorey reviewed on 2024-02-16 05:08_

---

_Review comment by @dylanbstorey on `README.md`:72 on 2024-02-16 05:08_

I don't have strong opinions about the formatting,  and based on opened issues a I'd suggest that its a common enough pattern that is also non obvious that adding the pattern might save others some frustration.


---

_@T-256 reviewed on 2024-02-16 11:46_

---

_Review comment by @T-256 on `README.md`:72 on 2024-02-16 11:46_

fwiw, this syntax will put in `pyproject.toml` when using `rye add mypkg --path D:/test/mypkg`:
```toml
dependencies = ["mypkg @ file:///D:/test/mypkg"]
```

---

_Merged by @zanieb on 2024-02-16 14:34_

---

_Closed by @zanieb on 2024-02-16 14:34_

---

_Comment by @charliermarsh on 2024-03-22 21:24_

This is supported as of `v0.1.24`. You can `uv pip install .` directly, without including a package name.

---
