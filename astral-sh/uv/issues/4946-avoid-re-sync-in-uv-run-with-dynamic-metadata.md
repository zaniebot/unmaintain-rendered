```yaml
number: 4946
title: "Avoid re-sync in `uv run` with dynamic metadata"
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
  - preview
assignees: []
created_at: 2024-07-09T21:57:49Z
updated_at: 2024-07-24T20:45:00Z
url: https://github.com/astral-sh/uv/issues/4946
synced_at: 2026-01-12T15:58:53Z
```

# Avoid re-sync in `uv run` with dynamic metadata

---

_@charliermarsh_

_No description provided._

---

_Label `needs-decision` added by @charliermarsh on 2024-07-09 21:57_

---

_Label `preview` added by @charliermarsh on 2024-07-09 21:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-18 00:57_

---

_Closed by @charliermarsh on 2024-07-22 00:04_

---

_Comment by @blueraft on 2024-07-24 19:54_

I think in `uv 0.2.28` it still re-syncs on every `uv run` invocation.

Seems like 600ms is spent on resolution every time:
```
❯ uv run black
warning: `uv run` is experimental and may change without warning
Resolved 38 packages in 610ms
Audited 6 packages in 0.01ms
```

Some verbose logs:

```
    0.304107s  84ms DEBUG uv_resolver::resolver Split specific environment resolution took 0.082s
       uv_dispatch::install resolution="hatch-fancy-pypi-readme==24.1.0, hatch-vcs==0.4.0, hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, setuptools==71.1.0, setuptools-scm==8.1.0, trove-classifiers==2024.7.2", venv="/Users/ahmedilyas/Library/Caches/uv/builds-v0/.tmp16wlC3"
          0.307448s   0ms DEBUG uv_dispatch Installing in hatch-fancy-pypi-readme==24.1.0, hatch-vcs==0.4.0, hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, setuptools==71.1.0, setuptools-scm==8.1.0, trove-classifiers==2024.7.2 in /Users/ahmedilyas/Library/Caches/uv/builds-v0/.tmp16wlC3
          0.322034s  14ms DEBUG uv_installer::plan Requirement already cached: hatch-fancy-pypi-readme==24.1.0
          0.329887s  22ms DEBUG uv_installer::plan Requirement already cached: hatch-vcs==0.4.0
          0.334509s  27ms DEBUG uv_installer::plan Requirement already cached: hatchling==1.25.0
          0.339634s  32ms DEBUG uv_installer::plan Requirement already cached: packaging==24.1
          0.344357s  36ms DEBUG uv_installer::plan Requirement already cached: pathspec==0.12.1
          0.345665s  38ms DEBUG uv_installer::plan Requirement already cached: pluggy==1.5.0
          0.354954s  47ms DEBUG uv_installer::plan Requirement already cached: setuptools==71.1.0
          0.355957s  48ms DEBUG uv_installer::plan Requirement already cached: setuptools-scm==8.1.0
          0.361379s  53ms DEBUG uv_installer::plan Requirement already cached: trove-classifiers==2024.7.2
          0.361406s  53ms DEBUG uv_dispatch Installing build requirements: hatch-fancy-pypi-readme==24.1.0, hatch-vcs==0.4.0, hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, setuptools==71.1.0, setuptools-scm==8.1.0, trove-classifiers==2024.7.2
         uv_installer::installer::install num_wheels=9
 uv_installer::installer::install num_wheels=9
        0.429133s 235ms DEBUG uv_build Calling `hatchling.build.get_requires_for_build_editable()`
       uv_build::run_python_script script="get_requires_for_build_editable", python_version=3.12.1
        1.152189s 958ms DEBUG uv_build Installing extra requirements for build backend
 uv_resolver::resolver::solve
    1.152336s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.12.1
```

---

_Comment by @charliermarsh on 2024-07-24 19:57_

Can you share any more details about your project? Is there anything else in the verbose logs?

---

_Comment by @blueraft on 2024-07-24 19:58_

This was just using black: https://github.com/psf/black

But I think the same is true for flask too.

---

_Comment by @charliermarsh on 2024-07-24 20:00_

But what's the project itself that you're running the command from?

---

_Comment by @charliermarsh on 2024-07-24 20:00_

Like cloning Black?

---

_Comment by @blueraft on 2024-07-24 20:00_

Yes

---

_Comment by @charliermarsh on 2024-07-24 20:02_

Ok thank you.

---

_Comment by @charliermarsh on 2024-07-24 20:29_

Ok after #5423 it goes down to 23ms for me on Black.

---

_Comment by @blueraft on 2024-07-24 20:44_

Awesome

---
