```yaml
number: 10059
title: "`module-import-not-at-top-of-file` ignore after `os.environ`"
type: issue
state: closed
author: Pastells
labels:
  - rule
assignees: []
created_at: 2024-02-20T10:49:01Z
updated_at: 2024-11-06T19:13:01Z
url: https://github.com/astral-sh/ruff/issues/10059
synced_at: 2026-01-12T15:54:49Z
```

# `module-import-not-at-top-of-file` ignore after `os.environ`

---

_@Pastells_

Sometimes you need to set an environment variable before importing a package, for example:

```python
import os
os.environ["CUDA_VISIBLE_DEVICES"] = "4" 
os.environ["WORLD_SIZE"] = "1"
import torch
```

See: https://discuss.pytorch.org/t/runtimeerror-device-0-device-num-gpus-internal-assert-failed/178118/6

My suggestion is to ignore E402 `module-import-not-at-top-of-file` if it comes after a `os.environ` statement, similar to https://github.com/astral-sh/ruff/pull/9047

---

_Label `rule` added by @charliermarsh on 2024-02-20 16:26_

---

_Comment by @charliermarsh on 2024-02-20 16:27_

I think I agree with this. We already ignore `sys.path` manipulations, and for me this is similar.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 16:43_

---

_Comment by @charliermarsh on 2024-02-20 18:03_

Added in https://github.com/astral-sh/ruff/pull/10066, though it'll be `--preview`-only until we release v0.3.0, per our versioning policy.

---

_Closed by @Pastells on 2024-02-20 18:07_

---

_Comment by @ion-elgreco on 2024-04-22 09:18_

@charliermarsh it might make sense to allow warnings.filterwarnings in between as well. I sometimes have to add that in between imports to silence the warnings

---

_Comment by @Cjkjvfnby on 2024-11-06 19:13_

I just use `extend-per-file-ignores` in my Ruff config, to suppress this rule for that specific file.  


In my case it's custom logger configuration.  I need to setup logger, before calling any getLogger. Since I use logger in other files, I have to do it before imports.

---
