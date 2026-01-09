---
number: 14248
title: "Cannot resolve github dependency with `uv tool install <package>` from pypi"
type: issue
state: closed
author: noamteyssier
labels:
  - bug
assignees: []
created_at: 2025-06-25T00:06:39Z
updated_at: 2025-06-25T16:47:02Z
url: https://github.com/astral-sh/uv/issues/14248
synced_at: 2026-01-07T13:12:18-06:00
---

# Cannot resolve github dependency with `uv tool install <package>` from pypi

---

_Issue opened by @noamteyssier on 2025-06-25 00:06_

### Summary

Hello! 

This may be an oversight on my end but I'm having issues installing a package from pypi because a github dependency cannot be resolved. 

This is the [python package](https://pypi.org/project/arc-state/) and [github repo](https://github.com/arcinstitute/state)

Here is a command to replicate the error:
```bash
# Will error
uv tool install -U arc-state==0.8.6
```

with the error:
```text
error: Because zclip was not found in the package registry and arc-state==0.8.6 depends on zclip, we can conclude that arc-state==0.8.6 cannot be used.
And because you require arc-state==0.8.6, we can conclude that your requirements are unsatisfiable.
```

Notably this error does not occur when installing locally or from github:
```bash
# Works fine
uv tool install git+https://github.com/arcinstitute/state
```

My project dependencies are:
```
dependencies = [
  "anndata>=0.11.4",
  "cell-load>=0.7.1",
  "numpy>=2.2.6",
  "pandas>=2.2.3",
  "pyyaml>=6.0.2",
  "scanpy>=1.11.2",
  "scikit-learn>=1.6.1",
  "seaborn>=0.13.2",
  "torch>=2.7.0",
  "tqdm>=4.67.1",
  "wandb>=0.19.11",
  "hydra-core>=1.3.2",
  "geomloss>=0.2.6",
  "zclip",
  "transformers>=4.52.3",
  "cell-eval>=0.5.22",
]
```

and I include the source like this:
```toml
[tool.uv.sources]
zclip = { git = "https://github.com/bluorion-com/ZClip" }
```

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.14 (e7f596711 2025-06-23)

### Python version

Python 3.13.0

---

_Label `bug` added by @noamteyssier on 2025-06-25 00:06_

---

_Comment by @charliermarsh on 2025-06-25 16:41_

Once you publish to a registry, `[tool.uv.sources]` will no longer get picked up when you try to install a package. `[tool.uv.sources]` intentionally don't make it into the published metadata, which needs to follow a specific standard.

As a workaround, you could do this:

```
uv tool install --with git+https://github.com/bluorion-com/ZClip -U arc-state==0.8.6
```

---

_Comment by @noamteyssier on 2025-06-25 16:47_

gotcha - thanks @charliermarsh ! 

I'll use this command in the meantime and try to get them to publish their module on pypi.

Cheers 

---

_Closed by @noamteyssier on 2025-06-25 16:47_

---
