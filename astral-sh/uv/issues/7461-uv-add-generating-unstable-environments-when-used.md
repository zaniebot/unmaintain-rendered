---
number: 7461
title: uv add generating unstable environments when used with local version identifiers
type: issue
state: closed
author: itrase
labels:
  - question
assignees: []
created_at: 2024-09-17T14:08:32Z
updated_at: 2025-07-16T13:54:14Z
url: https://github.com/astral-sh/uv/issues/7461
synced_at: 2026-01-10T01:24:15Z
---

# uv add generating unstable environments when used with local version identifiers

---

_Issue opened by @itrase on 2024-09-17 14:08_

On uv version 0.4.10

To reproduce:
`mkdir project_a`
`cd project_a`
`uv init`
`uv add torch==2.4.1+cu124 --index-url https://download.pytorch.org/whl/cu124`
`uv add numpy`

The resulting environment technically works (torch is available for import and uses the correct version of CUDA), but trying to add numpy (or any other package) will give the following error:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.4.1+cu124 and your project depends on torch==2.4.1+cu124, we can conclude that
      your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```

My expectation with `uv add` is that it is intended to result in a reproducible environment, but it doesn't feel like it is the case here. I know that there is significant weirdness around local version identifiers, so perhaps this is intended behavior - but if so, having uv emit a warning would be good.

We can solve this by adding the following to the pyproject.toml:
```
[tool.uv]
extra-index-url = [
    "https://download.pytorch.org/whl/cu124",
]
```

This allows us to continue to `add` packages as normal. However, if I then initialize another project and try to `add` `project_a`, things break again:

`mkdir project_b`
`cd project_b`
`uv init`
`uv add ../project_a --editable`

We get the same error:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.4.1+cu124 and project-a==0.1.0 depends on torch==2.4.1+cu124, we can conclude
      that project-a==0.1.0 cannot be used.
      And because only project-a==0.1.0 is available and your project depends on project-a, we can conclude that your
      project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```
Once again, we can solve this by adding the extra-index-url to `project_b`'s `pyproject.toml`, but my expectation is that the extra index should be inherited automatically.

I believe a similar problem exists for things like torch-scatter, which want to use find-links to be installed:
`uv add torch_scatter  -f https://data.pyg.org/whl/torch-2.4.0+cu124.html`
adding a find-links section to the [tool.uv] allows it to proceed, but results in all downstream projects needing that line as well.

---

_Comment by @charliermarsh on 2024-09-17 14:30_

I think `uv add` should add the index, and we're going to do that as part of some larger changes to how indexes are handled. (I don't think that `project_b` should implicitly use the PyTorch index, but this will become easier to get right when we make the aforementioned changes.)


---

_Comment by @JackismyShephard on 2024-11-07 17:39_

@charliermarsh I am experiencing what I believe may be a related issue. I have a distributable cli application that uses pytorch 2.5.1+cuda124 as a dependency and is built and published (to pypi) using uv. If I try to test the cli application using `uv tool install` (in another project) I get: `error: Because there is no version of torch==2.5.1+cu124 and ultimate-rvc==0.1.5 depends on torch==2.5.1+cu124, we can conclude that ultimate-rvc==0.1.5 cannot be used.` Is a fix on the horizon?

---

_Comment by @itrase on 2024-11-07 18:45_

@JackismyShephard I'm installing torch in a normal package (not something intended as a tool), but with the updated version of uv (0.4.30), the syntax in the toml that I am using is:

```
requires-python = ">=3.11,<3.12"
dependencies = [
    "torch==2.5.1+cu124",
]

[tool.uv.sources]
torch = { index = "pytorch" }

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

This has been working for me - does your cli application specify the index like this? 

---

_Comment by @JackismyShephard on 2024-11-07 20:14_

@itrase Yes I specify the index just like that. I don't think it should make a difference whether the application exposes a cli entry point or not. I have tried both using `uv tool install` as well as `pip install` (in a regular virtual environment with no uv installed) . Neither works. However, it should be noted that the package can be installed (from PyPI) as long as I first explicitly run `pip install torch==2.5.1+cu124 torchaudio==2.5.1+cu124 --index-url https://download.pytorch.org/whl/cu124`.

---

_Comment by @charliermarsh on 2024-12-27 18:42_

@itrase -- Is this solved by using `uv add --default-index`? That writes the index to `pyproject.toml`.

---

_Label `question` added by @charliermarsh on 2024-12-27 18:42_

---

_Comment by @itrase on 2024-12-28 18:54_

yes, thank you!

---

_Comment by @JackismyShephard on 2025-03-02 16:50_

@itrase @charliermarsh I am still having problems installing my package which is packaged with uv and depending on torch using pip. It only works if I explicitly install torch first using `pip install torch==2.4.1+cu124 torchaudio==2.4.1+cu124 --index-url https://download.pytorch.org/whl/cu124`. But I guess this might be a pip specific issue due to torch not being hosted on pypi?

---

_Closed by @charliermarsh on 2025-07-16 13:54_

---
