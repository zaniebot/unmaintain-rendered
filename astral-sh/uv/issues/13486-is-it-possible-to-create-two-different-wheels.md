```yaml
number: 13486
title: Is it possible to create two different wheels using the same pyproject.toml?
type: issue
state: closed
author: Garrus990
labels:
  - question
assignees: []
created_at: 2025-05-16T09:35:48Z
updated_at: 2025-05-16T10:47:02Z
url: https://github.com/astral-sh/uv/issues/13486
synced_at: 2026-01-12T16:01:30Z
```

# Is it possible to create two different wheels using the same pyproject.toml?

---

_@Garrus990_

### Question

*Context*
I'm using `uv` to run jobs on Databricks. The jobs are wheel-based, so I am building a wheel file and this file is used by DBX clusters to install the required packages in specified versions. My job comprises two tasks, each of which could have its own wheel delivered, which would be beneficial for cluster set-up time as I wouldn't have to install unnecessary packages (that are required in one task and not in the other), some of which can be quite big (pytorch).

*Question*
Is it possible now or would it be possible to have a way to build two different wheels from the same pyproject.toml by using some parameters? I know I can specify packages in optional-dependencies, but the DBX runner uses (I assume) `pip install package` and I have no way of requesting installation of `pip install package[pytorch]` which could save me some time for the tasks where pytorch is not required. If that would be possible I could build two wheels, one slim and one with all the bells and whistles and supply them to each task independently.

### Platform

Ubuntu 22.04

### Version

0.5.24

---

_Label `question` added by @Garrus990 on 2025-05-16 09:35_

---

_Comment by @konstin on 2025-05-16 09:45_

The decision what a wheel contains is up to the build backend (e.g., hatchling), but it is expected that the built wheel is consistent, as other tool might break if it isn't. uv assumes that optional dependencies are used for this case, so we don't have a solution for this.

---

_Comment by @Garrus990 on 2025-05-16 10:47_

Makes sense, I was just hoping that in the future there may be an option like `--make-group-non-optional` that would, for the time of wheel creation, mark one of the groups as "core" published dependencies. But I guess that wouldn't be very pythonic. Thanks for help!

---

_Closed by @Garrus990 on 2025-05-16 10:47_

---
