```yaml
number: 10712
title: Pytorch for rocm needs explicit installation of python-triton-rom
type: issue
state: closed
author: bgs4free
labels:
  - needs-mre
assignees: []
created_at: 2025-01-17T14:48:07Z
updated_at: 2025-04-24T09:34:06Z
url: https://github.com/astral-sh/uv/issues/10712
synced_at: 2026-01-12T16:00:19Z
```

# Pytorch for rocm needs explicit installation of python-triton-rom

---

_@bgs4free_

For some reason, if you want to install `pytorch` from https://download.pytorch.org/whl/rocm6.2 index, it seems necessary to also explicitly install `python-triton-rocm` from the same index. Otherwise I run into

```
× No solution found when resolving dependencies for split (python_full_version ==
│ '3.11.*'):
╰─▶ Because there is no version of pytorch-triton-rocm{python_full_version < '3.13'
    and platform_machine == 'x86_64' and platform_system == 'Linux'}==3.1.0 and
    torch==2.5.1+rocm6.2 depends on pytorch-triton-rocm{python_full_version < '3.13' and
    platform_machine == 'x86_64' and platform_system == 'Linux'}==3.1.0, we can conclude
    that torch==2.5.1+rocm6.2 cannot be used.
    And because only the following versions of torch are available:
        torch<2.5.1
        torch==2.5.1+rocm6.2
    we can conclude that torch>=2.5.1 cannot be used
  ...
```

Though I don't think this is a uv issue, it might be good to mention it in the docs. Could prevent some headaches.

---

_Comment by @charliermarsh on 2025-01-17 15:19_

Can you share the configuration you're using to produce this?

---

_Label `needs-mre` added by @charliermarsh on 2025-01-17 15:19_

---

_Comment by @bgs4free on 2025-01-17 20:07_

```
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "pytorch-triton-rocm>=3.1.0", 
    "torch>=2.5.1"
]

[tool.uv.sources]
torch = [{ index = "pytorch-rocm" }]
pytorch-triton-rocm = [{ index = "pytorch-rocm" }]

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
explicit = true
```

This way everything works fine. Once I remove `pytorch-triton-rocm` I get the error.
Somewhat unfortunate, that they named it `pytorch-triton-rocm`. No idea, why. I guess, if they just had named it `pytorch-triton`, it would have worked implicitly.

---

_Comment by @charliermarsh on 2025-01-17 20:22_

This looks right to me? You need `pytorch-triton-rocm` to come from the PyTorch index (the version on PyPI is out-dated -- it's definitely not `3.1.0`), and you made that index explicit. So you have to mark that dependency as "sourced from the PyTorch index".

---

_Comment by @bgs4free on 2025-01-17 21:40_

I know that torch for rocm needs a special variant of triton and it's not an issue with uv. But since the docs contain a section dedicated to pytorch and specifically mention rocm (though not consistently throughout), I thought it be helpful to have an example that highlights this "asymmetry". Took me a moment to figure out the problem myself.

---

_Comment by @bgs4free on 2025-01-18 10:44_

I compared the behavior with pip:

```
pip install torch --index-url https://download.pytorch.org/whl/rocm6.2
```

Pip is able to resolve the transitive dependency on `pytorch-triton-rocm` with no issues. No need to install it explicitly.

Now I'm wondering if this might be a uv issue after all. At least pip behaves as I would have expected intuitively: look for transitive dependencies in the custom index first, then fall back on the default.

Btw, I'm not exactly sure, what you meant by 

> So you have to mark that dependency as "sourced from the PyTorch index"

---

_Comment by @charliermarsh on 2025-01-18 15:27_

@bgs4free -- This:

```toml
[tool.uv.sources]
torch = [{ index = "pytorch-rocm" }]

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
explicit = true
```

The use of `explicit = true` tells uv that the PyTorch index should _only_ be used for `torch`, and not for any other dependencies. It is _not_ the same as pip, which does not have this behavior.

If you want pip's behavior, you need to use different configuration. Specifically, you'd want:

```toml
[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"

[tool.uv]
index-strategy = "unsafe-best-match"
```


---

_Closed by @charliermarsh on 2025-01-18 15:27_

---

_Comment by @charliermarsh on 2025-01-18 15:28_

If you have more questions, it might help to read through https://docs.astral.sh/uv/configuration/indexes/ which goes through how we manage multiple indexes.

---

_Comment by @bgs4free on 2025-01-18 20:55_

Ok, I think, I understand now. Thanks a lot.

So in my case, if I don't set it to `explicit`, I'd run into (in my case):
```
hint: `requests` was found on https://download.pytorch.org/whl/rocm6.2, but not
      at the requested version (requests>=2.32.2)
```

because it doesn't look for `requests` in the pypi index. That is to prevent dependency confusion attacks, because of the assumption, that a custom index is used internally by organizations, in which case I would definitely want that behavior. Correct? 


---

_Comment by @charliermarsh on 2025-01-18 23:17_

No problem. That's right. If you want to use pip-like behavior where it _does_ check PyPI too, you'd add:

```toml
[tool.uv]
index-strategy = "unsafe-best-match"
```

---

_Comment by @theepicflyer on 2025-04-24 09:34_

Could the above be reflected in the uv guide on PyTorch? Similar to #11109

---
