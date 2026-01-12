```yaml
number: 1648
title: Move external licenses to separate directory
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/license-files
created_at: 2023-01-05T00:59:18Z
updated_at: 2023-01-28T09:20:55Z
url: https://github.com/astral-sh/ruff/pull/1648
synced_at: 2026-01-12T15:55:06Z
```

# Move external licenses to separate directory

---

_@charliermarsh_

I received some feedback from a friend at Google that our license structure was failing to get picked up as MIT when run against the license auto-detection systems that make it easier for Google employees to contribute. This was the other structure I considered initially (add licenses to a subdirectory), so lets run with that instead.


---

_Merged by @charliermarsh on 2023-01-05 01:07_

---

_Closed by @charliermarsh on 2023-01-05 01:07_

---

_Branch deleted on 2023-01-05 01:07_

---

_@charliermarsh reviewed on 2023-01-25 12:27_

---

_Review comment by @charliermarsh on `pyproject.toml`:44 on 2023-01-25 12:27_

@messense - Do you know if there's any way for Maturin to include these if I'm willing to manually enumerate them? (Even if they're not identified as "licenses" in any special way, but rather, just included in the distributed wheel in some form?)

---

_@messense reviewed on 2023-01-25 12:30_

---

_Review comment by @messense on `pyproject.toml`:44 on 2023-01-25 12:30_

You can use the `tool.maturin.include` option, see https://maturin.rs/metadata.html#add-maturin-build-options

---

_@charliermarsh reviewed on 2023-01-25 18:25_

---

_Review comment by @charliermarsh on `pyproject.toml`:44 on 2023-01-25 18:25_

Thank you :)

I'm finding that I have to do `include = ["../licenses/*"]` given our structure. Which gives me this slightly strange wheel:

![Screen Shot 2023-01-25 at 1 24 49 PM](https://user-images.githubusercontent.com/1309177/214650260-bad86fce-4f86-4ffc-8743-d1b406dd61c6.png)

Would it be too weird of me to symlink `licenses` into `python`, to get this?
 
![Screen Shot 2023-01-25 at 1 24 24 PM](https://user-images.githubusercontent.com/1309177/214650333-75a7e478-5fb7-4c45-8512-5d6182242ada.png)


---

_Review comment by @charliermarsh on `pyproject.toml`:44 on 2023-01-26 00:52_

@messense - Definitely not urgent, I decided to merge the licenses back into a single file (and was able to fix the broken license detector via some licensee hacks, I think).

---

_@charliermarsh reviewed on 2023-01-26 00:52_

---

_@messense reviewed on 2023-01-28 09:20_

---

_Review comment by @messense on `pyproject.toml`:44 on 2023-01-28 09:20_

> Would it be too weird of me to symlink `licenses` into `python`, to get this?

I think it's fine for now, in the future `ruff-X.Y.Z.dist-info/license_files` folder is the proper place to put them. `[tool.maturin.include]` doesn't support changing the path in wheel/sdist at the moment.

---
