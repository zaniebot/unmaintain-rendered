```yaml
number: 16596
title: "Why are source includes not included in the wheel with `uv_build`?"
type: issue
state: closed
author: rzuckerm
labels:
  - question
assignees: []
created_at: 2025-11-04T19:18:19Z
updated_at: 2025-11-07T16:06:15Z
url: https://github.com/astral-sh/uv/issues/16596
synced_at: 2026-01-12T16:02:34Z
```

# Why are source includes not included in the wheel with `uv_build`?

---

_@rzuckerm_

### Question

I have a use-case where a project is including a configuration file (`config.yaml`) that is outside the package directory (`my_project/`). When I do `uv build`, the configuration file shows up in the package tarball but not in the wheel. In poetry, I had this, and the file showed up in both:
```toml
[project]
name = "my_project"
# ...

[tool.poetry]
packages = {include = "config.yaml"}
```
For `uv`, I have this:
```toml
[project]
name = "my_project"
# ...

[tool.uv.build-backend]
module-root = ""
source-include = ["config.yaml"]
```
I've even tried adding `workspace = true` to `tool.uv.build-backend` but to no avail. What am I doing wrong?

### Platform

Ubuntu 24.04 amd64

### Version

0.9.7

---

_Label `question` added by @rzuckerm on 2025-11-04 19:18_

---

_Comment by @konstin on 2025-11-05 17:44_

The `config.yaml` should go inside the package directory instead of into the top level of the wheel. This shouldn't really work in poetry either, as `config.yaml` is not a package. Another option for the configuration file are the data directories, which also work for arbitrary files.

---

_Comment by @rzuckerm on 2025-11-05 19:13_

Yeah, I agree with that. However, I'm just investigating our current poetry-based projects to see what problems we are going to have when we go to `uv` to minimize the disruption to our developer community. I'm tending to think that we will just use `hatchling` for the more exotic use-cases.

---

_Comment by @rzuckerm on 2025-11-07 13:39_

It seems that I cannot even use `hatchling` for these cases. If there are files in the wheel that are not in the source tarball, then `uv build` errors out with a stack dump in `hatchling` indicated that the wheel-only file does not exist. However, if I build with `hatch build`, I do not see this problem. I really do not want to have to include `hatch` since it has just as many dependencies as `poetry`, and our developer community does not have any familiarity with this. I will log a separate issue for this.

---

_Comment by @rzuckerm on 2025-11-07 16:02_

See #16634 

---

_Closed by @rzuckerm on 2025-11-07 16:06_

---
