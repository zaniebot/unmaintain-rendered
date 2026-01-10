---
number: 8717
title: "`find-links` should be automatically added to `pyproject.toml` after `uv add -f`"
type: issue
state: open
author: sheey11
labels:
  - enhancement
assignees: []
created_at: 2024-10-31T08:49:58Z
updated_at: 2025-11-28T08:25:51Z
url: https://github.com/astral-sh/uv/issues/8717
synced_at: 2026-01-10T01:24:31Z
---

# `find-links` should be automatically added to `pyproject.toml` after `uv add -f`

---

_Issue opened by @sheey11 on 2024-10-31 08:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

After installed a package with `--find-links`, the next installation will fail because `uv` is unable to find package without find-links.

Reproduction:

```sh
uname -a
# Linux GOOSE 5.15.153.1-microsoft-standard-WSL2 #1 SMP Fri Mar 29 23:14:13 UTC 2024 x86_64 GNU/Linux

uv --version
# uv 0.4.28 (debe67ffd 2024-10-28)

uv init find-links-test && cd find-links-test

# see https://www.dgl.ai/pages/start.html
# the highest version of `dgl` on PyPI is 2.2.1, and the find-link
# below provides version 2.4.0.
uv add dgl -f https://data.dgl.ai/wheels/torch-2.4/cu124/repo.html

# `find-links` are not recorded
cat pyproject.toml
# [project]
# name = "find-links-test"
# version = "0.1.0"
# description = "Add your description here"
# readme = "README.md"
# requires-python = ">=3.11"
# dependencies = [
#     "dgl>=2.4.0",
# ]

uv add datasets
#  × No solution found when resolving dependencies for split (python_full_version == '3.11.*'):
#  ╰─▶ Because only dgl<=2.2.1 is available and your project depends on dgl>=2.4.0, we can conclude that your project's requirements are unsatisfiable.
#  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.

# manually adding it to pyproject.toml
cat <<EOF >>pyproject.toml
[tool.uv]
find-links = ["https://data.dgl.ai/wheels/torch-2.4/cu124/repo.html"]
EOF

uv add datasets
# Installed 15 packages in 35ms
```


---

_Comment by @FishAlchemist on 2024-10-31 09:29_

I prefer manual control over find-links placement to ensure the correct search order. Automating this process could lead to unintended consequences due to the array's sensitivity to sequence.
While this approach is convenient, I don't think it's a good idea from a security standpoint.
Therefore, I personally disagree with automatically writing find-links into the configuration file.

**Note:** I'm just sharing my thoughts on this. Ultimately, the decision of whether to support this feature in UV is out of my hands.


---

_Assigned to @charliermarsh by @zanieb on 2024-10-31 13:21_

---

_Comment by @sheey11 on 2024-11-01 02:34_

> I prefer manual control over find-links placement to ensure the correct search order. Automating this process could lead to unintended consequences due to the array's sensitivity to sequence. While this approach is convenient, I don't think it's a good idea from a security standpoint. Therefore, I personally disagree with automatically writing find-links into the configuration file.
> 
> **Note:** I'm just sharing my thoughts on this. Ultimately, the decision of whether to support this feature in UV is out of my hands.

Reasonable, instead of record `find-links` project wide, there should be another way to check the version of that package, I find that the `find-links` actually been recorded on `uv.lock`:

```toml
[[package]]
name = "dgl"
version = "2.4.0+cu124"
source = { registry = "https://data.dgl.ai/wheels/torch-2.4/cu124/repo.html" }
```

The `source` recorded here should be used to find the package, but seems `uv` use the `find-links` in `pyproject.toml` only.

---

_Comment by @FishAlchemist on 2024-11-01 03:12_

@sheey11 
In uv.lock, the source is indeed written directly, and the closest feature to explicitly specifying a source is currently ``Package indexes``, but it seems that it doesn't support find-link.
**Package indexes** https://docs.astral.sh/uv/configuration/indexes/#package-indexes
**uv.lock(lockfile)** https://docs.astral.sh/uv/concepts/projects/#project-lockfile

However, I'm not sure if there's a better way to do this, since I don't use ``find-links`` myself.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-11-01 13:29_

---

_Label `enhancement` added by @charliermarsh on 2024-11-01 13:29_

---

_Comment by @charliermarsh on 2024-11-01 13:30_

We probably won't write `find-links`, but I did leave space open for `[[tool.uv.index]]` entries to have a `find-links` "kind", so we'd add a `[[tool.uv.index]]` entry instead. Somewhat low priority but it's a TODO.

---

_Referenced in [astral-sh/uv#9079](../../astral-sh/uv/issues/9079.md) on 2024-11-13 09:13_

---

_Renamed from "`file-links` should be automatically added to `pyproject.toml` after `uv add -f`" to "`find-links` should be automatically added to `pyproject.toml` after `uv add -f`" by @zanieb on 2025-01-06 22:22_

---

_Comment by @FishAlchemist on 2025-03-26 12:53_

@charliermarsh Is there any consideration to ``uv add -f`` support for ``[[tool.uv.index]]``?



---

_Comment by @charliermarsh on 2025-03-26 13:12_

Good call, we should do it.

---

_Comment by @sanbei101 on 2025-11-22 12:29_

squat

---
