---
number: 9174
title: Force uv to skip certain (transitive) dependencies
type: issue
state: closed
author: chrisfougner
labels: []
assignees: []
created_at: 2024-11-17T23:00:11Z
updated_at: 2025-11-08T20:01:55Z
url: https://github.com/astral-sh/uv/issues/9174
synced_at: 2026-01-10T01:24:37Z
---

# Force uv to skip certain (transitive) dependencies

---

_Issue opened by @chrisfougner on 2024-11-17 23:00_

Is there a way to force uv to skip certain dependencies? Specifically we use docker containers [maintained by Nvidia](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch), which come pre-installed with various python ML packages on the system level (torch, apex, amp_C, flash-attn). If I use `uv venv --system-site-packages` then I can access those dependencies, the problem is that if any package that I install has a transitive dependency on one of these packages (primarily torch), then the one installed in venv will be chosen over the system one, and things break. I'd like to be able to specify that torch for example should not be installed as a transitive dependency, but I'm not sure how to do this.  

---

_Comment by @danielhollas on 2024-11-18 00:39_

It would be great to have a proper solution for this, not just a hack. We have a similar situation in our container setup, but the number of packages much larger.

See also discussion in #2500

---

_Comment by @charliermarsh on 2024-11-18 01:14_

I think using overrides with an "impossible" marker is actually kind of a fine solution? The alternative is that we introduce something non-standard.

---

_Comment by @charliermarsh on 2024-11-18 01:18_

I think this should be tracked in #4422 though.

---

_Closed by @charliermarsh on 2024-11-18 01:18_

---

_Comment by @chrisfougner on 2024-11-18 11:04_

Thank you both. Super helpful. The solution seems fine to me.

For anyone else that stubmless upon this issue. Add the following to your pyproject.toml. If you have a top-level pyproject.toml and any additional package specific ones, you should add it to the top-level pyproject.toml

```
[tool.uv]
override-dependencies = [
    "torch; sys_platform == 'never'",
]
```

---

_Comment by @danielhollas on 2024-11-18 11:07_

I would just note that this is quite a  dangerous solution to the original problem, since if a package depends on a torch version that is not satisfied by the underlying environment this will result in a broken installation.

---

_Referenced in [astral-sh/uv#11012](../../astral-sh/uv/issues/11012.md) on 2025-01-29 18:11_

---

_Referenced in [vllm-project/vllm-spyre#61](../../vllm-project/vllm-spyre/issues/61.md) on 2025-03-27 22:01_

---

_Referenced in [prefix-dev/pixi#4569](../../prefix-dev/pixi/issues/4569.md) on 2025-09-15 20:41_

---

_Comment by @ahakanbaba on 2025-10-03 04:23_

> Thank you both. Super helpful. The solution seems fine to me.
> 
> For anyone else that stubmless upon this issue. Add the following to your pyproject.toml. If you have a top-level pyproject.toml and any additional package specific ones, you should add it to the top-level pyproject.toml
> 
> ```
> [tool.uv]
> override-dependencies = [
>     "torch; sys_platform == 'never'",
> ]
> ```


This is a very reasonable workaround. 
BUT, this does not work with `--universal` parameter :( 


Instead I have written an obviously false another constraint. For example 

```
[tool.uv]
override-dependencies = [
    "torch; python_version < '3.0'",
]
```

---

_Comment by @atiladeokegab on 2025-11-08 20:01_

thank you

---
