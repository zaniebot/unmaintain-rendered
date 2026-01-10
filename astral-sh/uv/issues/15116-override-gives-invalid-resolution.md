---
number: 15116
title: "`override` gives invalid resolution?"
type: issue
state: closed
author: DanielYang59
labels:
  - question
assignees: []
created_at: 2025-08-06T19:23:00Z
updated_at: 2025-08-09T20:49:54Z
url: https://github.com/astral-sh/uv/issues/15116
synced_at: 2026-01-10T01:25:52Z
---

# `override` gives invalid resolution?

---

_Issue opened by @DanielYang59 on 2025-08-06 19:23_

### Summary

We (credit to @blueraft) have a [complex dependency structure](https://gitlab.mpcdf.mpg.de/nomad-lab/nomad-distro/-/merge_requests/199) but I'm able to recreate with the following dummy packages:

```
dummy-distro
├── dummy-lab
│   └── numpy (no version constraint)
└── dummy-xrd
    ├── dummy-lab         ← shared dependency
    └── tensorflow >= 2.16
```

Basically we have a wrapper package `dummy-distro` which depends on both `dummy-lab` (the core package) and `dummy-xrd` (a plugin). `dummy-lab` depends on `numpy` without version constraint. `dummy-xrd` depends on `dummy-lab` and `tensorflow`.

Enclosed a copy of this dummy package structure: [uv-override-recreate.tgz](https://github.com/user-attachments/files/21632104/uv-override-recreate.tgz)

---

In `dummy-distro` we used `override` to test numpy 2:
```toml
[tool.uv]
required-version = ">=0.5.15"
override-dependencies = [
  "numpy>=2",
  "tensorflow>=2.16",  # assist numpy version resolving
]
```

And used the following to generate `requirements.txt`:
```sh
uv pip compile --universal -p 3.10 --annotation-style=line --output-file=requirements.txt pyproject.toml
```

But the generated `requirements.txt` seems to contain incompatible `numpy` and `tensorflow` (when compatible resolution exists):
```
tensorflow==2.19.0        # via dummy-xrd, --override (workspace)

numpy==2.2.6 ; python_full_version < '3.11'  # via dummy-lab, h5py, keras, ml-dtypes, tensorboard, tensorflow, --override (workspace)
numpy==2.3.2 ; python_full_version >= '3.11'  # via dummy-lab, h5py, keras, ml-dtypes, tensorboard, tensorflow, --override (workspace)
```

```console
➜ tensorflow-2.19.0.dist-info: grep numpy METADATA 
Requires-Dist: numpy <2.2.0,>=1.26.0
```



### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.5 (Homebrew 2025-08-05)

### Python version

_No response_

---

_Label `bug` added by @DanielYang59 on 2025-08-06 19:23_

---

_Renamed from "`override` leads to invalid resolution?" to "`override` gives invalid resolution?" by @DanielYang59 on 2025-08-06 19:23_

---

_Comment by @charliermarsh on 2025-08-06 20:23_

I think that's "correct" based on how overrides work. Overrides override the dependency definition for any package that requires the overridden package. So from uv's perspective, the `Requires-Dist: numpy <2.2.0,>=1.26.0` is ignored and replaced with `numpy>=2`.

---

_Comment by @DanielYang59 on 2025-08-06 20:36_

Ok thanks for the quick reply and explanation ;) 

This is a bit unexpected to me (I don't quite know how the resolver works so likely I'm wrong) because I was expecting dependencies in the `override` group would still try to find a valid resolution _within_ the `override` group, which would then be forced on the default group regardless of compatibility. I.e. the numpy requirement for tensorflow (as a member of the override group) would still be considered together with the override `numpy>=2`

---

_Comment by @charliermarsh on 2025-08-06 20:44_

Could the `numpy` override instead be a `constraint-dependencies`?

---

_Label `bug` removed by @zanieb on 2025-08-07 00:30_

---

_Label `question` added by @zanieb on 2025-08-07 00:30_

---

_Comment by @DanielYang59 on 2025-08-07 09:37_

Unfortunately I don't think we could use `constraint-dependencies` because some other plugins (not shown in this dummy structure) still haven't dropped `numpy<2` pin, but we want to test `numpy>=2` compatibility so I assume we have to use override here?

---

_Comment by @charliermarsh on 2025-08-08 09:31_

Ah yeah, I think this is roughly working as expected then. It's kind of important that overrides are a "hammer" like this, since adding constraints like (the overrides have to resolve together) would mean that you couldn't actually override dependencies that are dependencies of other overrides.

---

_Closed by @charliermarsh on 2025-08-08 09:31_

---

_Comment by @DanielYang59 on 2025-08-09 20:49_

Ok thanks for the input, so here is not a right or wrong thing then (just decision by `uv`), but probably the documentation could be clarified as I personally found this behaviour quite surprising

---
