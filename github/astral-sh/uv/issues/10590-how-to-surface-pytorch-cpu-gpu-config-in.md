---
number: 10590
title: How to surface Pytorch CPU/GPU config in workspace member to repo level
type: issue
state: closed
author: bepuca
labels:
  - bug
assignees: []
created_at: 2025-01-14T10:55:03Z
updated_at: 2025-01-27T14:05:43Z
url: https://github.com/astral-sh/uv/issues/10590
synced_at: 2026-01-07T13:12:18-06:00
---

# How to surface Pytorch CPU/GPU config in workspace member to repo level

---

_Issue opened by @bepuca on 2025-01-14 10:55_

## Context

I am working in a mono repo with workspaces. One of these workspace members requires PyTorch. To avoid massive environments when there is no GPU (so CI does not break), I was trying to set up PyTorch with extras as per defined in the [PyTorch integration docs](https://docs.astral.sh/uv/guides/integration/pytorch/).

## Problem/Question

Now, this works well at the package level. I can do something like `uv run --package my_package --extra gpu --isolated ...` and it does install what it should. The problem is that when I do `uv sync --extra gpu` at the top level, it does not cascade down and torch, an optional dependency, is not installed at all.

Now, I was thinking that maybe treating it the same way as any external package, I could do something like this:

```toml
[project.optional-dependencies]
cpu = [
    "my-package[cpu]"
]
gpu = [
    "my-package[gpu]"
]
```

At the top level `pyproject.toml`. But now, the `uv sync` fails on `error: Found conflicting extra 'cpu' unconditionally enabled in 'my-package[cpu] @ ... ; extra == 'cpu''`.

Even with the following at the top level `pyproject.toml` it does not work:

```
[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]
```

I am not sure if this is a bug or I just can't seem to figure it out. I understand that it may be undesirable to just cascade down extras, but then it would be awesome if you could help me figure out how to enable things so I install my package with an extra. I would really like to avoid details about torch at the top level, as the whole point is to keep things somewhat isolated.

Thank you very much!

---

_Label `bug` added by @charliermarsh on 2025-01-14 14:48_

---

_Comment by @charliermarsh on 2025-01-14 14:49_

I think there's a bug here unfortunately. We're looking into it.

---

_Comment by @BurntSushi on 2025-01-14 15:07_

This might be a duplicate of #9942, but there could be more than one bug lurking here. But some aspect of this is at least intended behavior for now (which we should try to fix). In particular, when `dependency[foo]` is seen where `foo` is an extra declared as conflicting, then uv _conservatively_ assumes that to be an unconditional enabling of a conflicting extra and thus returns an error:

https://github.com/astral-sh/uv/blob/faa4481ccc4af97b6043f824224c065da55a15c2/crates/uv-resolver/src/resolver/mod.rs#L3619-L3647

The comment in the code above shows why we do this.

Some thoughts on future direction here:

* If we could somehow prove that `dependency[foo]` is only enabled by another conflicting extra, then I think we could allow cases like that. But the logic for an iron clad proof here (that never permits installation of more than one version of the same package) seems a little tricky? At least, it's not immediately obvious to me. I'd have to sit down and think about it.
* The error reporting shown in the code above is very eager. That is, it actively seeks out a _possible_ conflict and hedges by returning an error. But maybe we don't need to do this eagerly and can be a bit more lazy about it? This also seems tricky to reason about.
* Very brainstorm-y thought: maybe we can do some workspace-level reasoning on the packages just in the workspace to do something smarter here?

---

_Comment by @bepuca on 2025-01-15 10:26_

Thank you guys for such a swift response.

To elaborate a bit more, to try to bypass the problem, I tried duplicating the `torch` duality as optional dependencies at the package level and at the repo level. This would be suboptimal but would unblock this approach. The problem I find when I do this is and `uv sync --extra gpu`:

```
error: Requirements contain conflicting indexes for package `torch` in all marker environments:
- https://download.pytorch.org/whl/cpu
- https://download.pytorch.org/whl/cu121
```

The error goes away when I remove the optional dependencies with the two extras CPU & GPU for torch. Now, I can also make it work if I just add `torch` as a normal dependency in the package level. The problem is that when I do `uv run` in these conditions I have no control over the version installed (just default PyPI) which does not cut it.

Do you guys have a workaround atm to be able to do `uv sync` at top level controlling CPU/GPU and also at package level?

Anyhow, the idea of this comment is to just give more observations of behaviors I can see. Let me know if I can help somehow.

---

_Comment by @sh1man999 on 2025-01-21 13:55_

> Спасибо вам, ребята, за такую оперативную реакцию.
> 
> Чтобы немного уточнить, чтобы попытаться обойти проблему, я попробовал дублировать двойственность в виде необязательных зависимостей на уровне пакета и на уровне репозитория. Это было бы неоптимально, но разблокировало бы этот подход. Проблема, с которой я сталкиваюсь, когда делаю это, заключается в том, что:`torch``uv sync --extra gpu`
> 
> ```
> error: Requirements contain conflicting indexes for package `torch` in all marker environments:
> - https://download.pytorch.org/whl/cpu
> - https://download.pytorch.org/whl/cu121
> ```
> 
> Ошибка исчезает, когда я удаляю необязательные зависимости с двумя дополнительными процессором и графическим процессором для torch. Теперь я также могу заставить его работать, если просто добавлю как обычную зависимость на уровне пакета. Проблема в том, что когда я делаю это в таких условиях, у меня нет контроля над установленной версией (только PyPI по умолчанию), которая ее не режет.`torch``uv run`
> 
> Есть ли у вас, ребята, обходной путь, чтобы иметь возможность управлять процессором/графическим процессором на верхнем уровне, а также на уровне пакета?`uv sync`
> 
> В любом случае, идея этого комментария состоит в том, чтобы просто дать больше наблюдений за поведением, которое я могу видеть. Дайте мне знать, если я могу как-то помочь.

I have the same error, it seems that the documentation is not written correctly

https://docs.astral.sh/uv/concepts/projects/dependencies/#optional-dependencies

---

_Referenced in [astral-sh/uv#9942](../../astral-sh/uv/issues/9942.md) on 2025-01-21 14:23_

---

_Comment by @reuben on 2025-01-21 14:40_

@bepuca you can try adapting my solution [here](https://github.com/astral-sh/uv/issues/9942#issuecomment-2549041505) but including a CPU extra which requires `torch=="X.Y.Z+cpu"` explicitly, all from the same index (so you control CPU vs. CUDA with the local version specifier rather than the index).

---

_Comment by @reuben on 2025-01-21 14:40_

That does mean that you have to depend on a specific patch version of Torch though, can't have any flexible resolution.

---

_Comment by @sh1man999 on 2025-01-22 09:10_

Yes, I did, but the documentation is misleading.

---

_Comment by @mchalecki on 2025-01-22 09:23_

I have the same use case: I'm using a submodule with cpu and gpu variants and want to forward these options to the main package.

Waiting on a bug fix as the current behavior makes it very complicated to integrate two such packages.

---

_Comment by @BurntSushi on 2025-01-22 17:04_

I think for now, I'd like to propose that we make the error checking for conflicting extras in `uv sync` more robust and remove the error checking in the resolver for "unconditional" enabling of conflicting extras. This will permit things like `foo[extra]` to work even if `extra` is declared as conflicting while still preserving the property that we never permit installation of multiple versions of the same package into the same environment. The downside of this is that we will sometimes produce "invalid" lock files that can never be installed (because running `uv sync` will always produce a conflicting extra error).

-----

OK, so I'm trying to distill this down into the essential thing we want to support that we don't support today. As mentioned above, uv will pretty conservatively fail whenever a conflicting extra is found "unconditionally" enabled in an optional dependency specification, even if the dependency specification itself is conditional. So trying to come up with a simpler example, consider this `pyproject.toml`:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.0"
dependencies = []

[tool.uv.workspace]
members = ["dependency"]

[tool.uv.sources]
dependency = { workspace = true }

[project.optional-dependencies]
x1 = [
    "dependency[nested-x1]"
]
x2 = [
    "dependency[nested-x2]"
]
```

And this `dependency/pyproject.toml`:

```toml
[project]
name = "dependency"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = []

[project.optional-dependencies]
nested-x1 = [
  "sortedcontainers==2.3.0",
]
nested-x2 = [
  "sortedcontainers==2.4.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "nested-x1" },
    { extra = "nested-x2" },
  ],
]
```

Running `uv lock` with the above gets you:

```
error: Found conflicting extra `nested-x1` unconditionally enabled in `dependency[nested-x1] @ file:///home/andrew/astral/uv/play/i10590-pytorch-shenanigans/simpler/dependency ; extra == 'x1'`
```

The conflicts are declared on `nested-x1` and `nested-x2`. And the dependent has its own `x1` and `x2` extras that each enable `nested-x1` and `nested-x2`, respectively. If `x1` and `x2` are enabled at the same time, then uv _should_ error since that configuration ultimately ends up enabling `nested-x1` and `nested-x2`. But that error checking (at install time) doesn't actually exist, so uv refuses to produce a resolution.

It seems like we could just add that error path such that `uv sync --extra x1 --extra x2` would just know to fail without adding anything else to `pyproject.toml` above.

I also want to consider my code comment from above as well:

https://github.com/astral-sh/uv/blob/028df07c192fc0ce023a1eacdfb6b1197c6fa390/crates/uv-resolver/src/resolver/mod.rs#L3617-L3646

Here, it's talking about a dependency graph like this:

```
root
|--> a
    |--> c[x1]
|--> b
    |--> c[x2]
```

where `x1` and `x2` are conflicting.

I think this was the original example that motivated the conservative error checking that uv is doing today. I think when I wrote this, what was on my mind is that since `c[x1]` and `c[x2]` are in different parts of the dependency tree, the necessary forks inside the resolver would never be created. But I think they actually are. And I think that if we make our error checking a bit smarter during `uv sync` (i.e., "does any pair of extras every get activated"), then it will cover the above case as well. **However, it will produce a lock file that will always fail to install.** Here's a setup that emulates the above tree.

First, `pyproject.toml`:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.0"
dependencies = ["a", "b"]

[tool.uv.workspace]
members = ["a", "b", "c"]

[tool.uv.sources]
a = { workspace = true }
b = { workspace = true }
c = { workspace = true }
```

Now `a/pyproject.toml`:

```toml
[project]
name = "a"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = ["c[x1]"]

[tool.uv.sources]
c = { workspace = true }
```

Now `b/pyproject.toml`:

```toml
[project]
name = "b"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = ["c[x2]"]

[tool.uv.sources]
c = { workspace = true }
```

And finally, `c/pyproject.toml`:

```toml
[project]
name = "c"
version = "0.1.0"
requires-python = ">=3.10.0"
dependencies = []

[project.optional-dependencies]
x1 = ["sortedcontainers==2.3.0"]
x2 = ["sortedcontainers==2.4.0"]

[tool.uv]
conflicts = [
  [
    { extra = "x1" },
    { extra = "x2" },
  ],
]
```

This setup also errors today for the same reason that the simpler example above does: there is an unconditional enabling of `x1` and `x2`. And indeed, in this example, there is no real valid installation as far as I can see. So this example would result in producing a lock file that would always fail at install time.

So... is there an alternative where we can correctly detect the second example above and thus fail to resolve, but simultaneously allow the first example with correct error checking at install time? I'm not quite sure to be honest. I think we would need to create a tree of reasoning with respect to the declared conflicts. i.e., From the first example above, in order to allow `dependency[nested-x1]` inside of the `x1` extra at the top-level, I think we'd basically have to behave _as if_ `x1` and `x2` were also declared as conflicting? And then I think we'd need to treat, e.g., `x2` as conflicting with `nested-x1` and `x1` as conflicting with `nested-x2` too as well.

---

_Referenced in [astral-sh/uv#10875](../../astral-sh/uv/pulls/10875.md) on 2025-01-22 19:55_

---

_Closed by @BurntSushi on 2025-01-22 23:52_

---

_Closed by @BurntSushi on 2025-01-22 23:52_

---

_Comment by @mchalecki on 2025-01-24 09:50_

This implemented solution installs actually both versions:

I'm running uv 0.5.23

```
#26 [base 22/29] RUN --mount=type=cache,target=/root/.cache/uv     uv sync --extra gpu 
#26 1.839 Prepared 3 packages in 1.66s
#26 2.863 Uninstalled 6 packages in 1.02s
#26 14.21 Installed 6 packages in 11.35s
#26 15.84 Bytecode compiled 29286 files in 1.62s
#26 15.84  ~ internal-package==0.1.0 (from file:///root/work/internal-package)
#26 15.84  + external-package==0.1.0 (from file:///root/work)
#26 15.84  ~ torch==2.4.1
#26 15.84  ~ torch==2.4.1+cu124
#26 15.84  ~ torchvision==0.19.1
#26 15.84  ~ torchvision==0.19.1+cu124
```

Running `uv pip list` I get both versions of torch and torchvision

```
...
torch                              2.4.1
torch                              2.4.1+cu124
torchvision                        0.19.1
torchvision                        0.19.1+cu124
...
```

---

_Comment by @BurntSushi on 2025-01-24 11:50_

@mchalecki Could you please include a full repro?

---

_Comment by @mchalecki on 2025-01-27 11:34_

I found the problem by trying to reproduce the problem on a new repo. The problem is with package torchmetrics. When it is in the workspace package/dependency this case exists. I have yet not inspect what can be wrong with that package configuration.

Minimal example:

Child:
```
[project]
name = "child-surface"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "torchmetrics>=1.1.0",
]


[project.optional-dependencies]
cpu = [
    "torch==2.4.1", 
    "torchvision==0.19.1"
]
gpu = [
    "torch==2.4.1", 
    "torchvision==0.19.1",
    "nvidia-dali-cuda120==1.30.0",
]
[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "gpu" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "gpu" },
]
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

Parent:
```
[project]
name = "parent-surface"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[tool.uv.sources]
child-surface = [{ workspace = true  },]

[tool.uv.workspace]
members = ["child-surface"]


[project.optional-dependencies]
cpu = [
    "child-surface[cpu]>=0.0.1", 
]
gpu = [
    "child-surface[gpu]>=0.0.1", 
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]
```
 
Full zipped example with lock file:
[parent-surface.zip](https://github.com/user-attachments/files/18557374/parent-surface.zip)

---
Edit: Looking at their repo and build system in [setup.py](https://github.com/Lightning-AI/torchmetrics/blob/master/setup.py#L162) I see that they define [base requirements](https://github.com/Lightning-AI/torchmetrics/blob/master/requirements/base.txt#L6C1-L6C22) which is just `torch >=2.0.0, <2.7.0`

So uv resolver has installed `torch==2.4.1+cu124` and then installs `torch==2.4.1` which is strange having two different versions of the same package

This behavior changed after introducing this change

---

_Referenced in [astral-sh/uv#10985](../../astral-sh/uv/issues/10985.md) on 2025-01-27 14:05_

---

_Comment by @BurntSushi on 2025-01-27 14:05_

@mchalecki Thank you! I can indeed reproduce it. It looks like a new bug. I opened https://github.com/astral-sh/uv/issues/10985 to track it.

---
