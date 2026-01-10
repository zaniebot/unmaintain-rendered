```yaml
number: 9289
title: Multiple conflicting extras installed when using explicit index assignments
type: issue
state: closed
author: anaoum
labels:
  - bug
assignees: []
created_at: 2024-11-20T18:59:38Z
updated_at: 2024-12-10T15:57:23Z
url: https://github.com/astral-sh/uv/issues/9289
synced_at: 2026-01-10T04:36:20Z
```

# Multiple conflicting extras installed when using explicit index assignments

---

_Issue opened by @anaoum on 2024-11-20 18:59_

When conflicting extra groups share a dependant, both extra groups seem to be installed.

Consider the following example adapted from the [docs](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies):
```pyproject.toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11,<3.12"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]
metrics = [
  "torchmetrics>=1.2.0",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cu124", extra = "cu124" },
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

The key difference is the additional extra `metrics` that includes the `torchmetrics` package that depends on `torch`.

When I lock and sync both cpu and metrics extras, I get multiple versions of torch installed:
```bash
$ uv sync --extra cpu --extra metrics
Resolved 34 packages in 1ms
Uninstalled 3 packages in 199ms
Installed 3 packages in 1.10s
 ~ torch==2.5.1
 ~ torch==2.5.1+cpu
 ~ torch==2.5.1+cu124
```

---

_Comment by @charliermarsh on 2024-11-20 19:03_

Interesting, thanks. I don't think the extra indexes are relevant, since you can reproduce with:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11,<3.12"
dependencies = []

[project.optional-dependencies]
foo = [
  "idna==3.10",
]
bar = [
  "idna==3.9",
]
baz = [
  "anyio",
]

[tool.uv]
conflicts = [
  [
    { extra = "foo" },
    { extra = "bar" },
  ],
]
```

From there, `uv tree` gives you:

```
project v0.1.0
├── idna v3.9 (extra: bar)
├── anyio v4.6.2.post1 (extra: baz)
│   ├── idna v3.9
│   ├── idna v3.10
│   └── sniffio v1.3.1
└── idna v3.10 (extra: foo)
```

---

_Label `bug` added by @charliermarsh on 2024-11-20 19:03_

---

_Comment by @anaoum on 2024-11-20 19:39_

Thanks. It also happens if the two conflicting extra groups share the common dependant:
```pyproject.toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.11,<3.12"
dependencies = []

[project.optional-dependencies]
foo = [
  "idna==3.10",
  "anyio",
]
bar = [
  "idna==3.9",
  "anyio",
]

[tool.uv]
conflicts = [
  [
    { extra = "foo" },
    { extra = "bar" },
  ],
]
```

---

_Assigned to @BurntSushi by @charliermarsh on 2024-11-20 20:20_

---

_Comment by @BurntSushi on 2024-11-21 12:17_

I think i see a path to fixing this. I think it has two components:

* In `ResolverOutput::from_state` (and possibly also `marker_reachability`), generate `extra = "..."` markers from each `Resolution`'s conflicting groups, if they have them. Then apply those markers via intersection to each of the edges in that `Resolution`. So in Charlie's example above, that means the `idna==3.9` dependency would have a `extra == "bar"` marker and the `idna==3.10` dependency would have a `extra == "foo" or extra == "baz"` marker. Or something similar.
* Update `InstallTarget::to_resolution` in `uv-resolver/lock` to pass in extras when doing marker evaluation.

One hiccup here is that this doesn't take groups into account, so we might not be able to reuse markers for this. We'll instead need an additional field to encode this extra/group inclusion logic.

---

_Comment by @BurntSushi on 2024-11-21 13:16_

Working it out by hand, I think the conditional logic for each idna dependency in each of the three forks present in this example is:

```
 idna==3.9: extra != "foo" and (extra == "bar" or extra == "baz")
idna==3.10: extra != "bar" and (extra == "foo" or extra == "baz")
idna==3.10: (extra != "foo" and extra != "bar") and extra == "baz"
```

Which combines to:

```
 idna==3.9: extra != "foo" and (extra == "bar" or extra == "baz")
idna==3.10: (extra != "bar" and (extra == "foo" or extra == "baz"))
            or ((extra != "foo" and extra != "bar") and extra == "baz")
```

And simplifies to:

```
 idna==3.9: extra != "foo" and (extra == "bar" or extra == "baz")
idna==3.10: (extra != "bar" and extra == "baz") or (extra != "bar" and extra == "foo")
```

Note that because of how extras work, the above notation is quite misleading. It's more clearly written like this:

```
 idna==3.9: "foo" not in extras and ("bar" in extras or "baz" in extras)
idna==3.10: ("bar" not in extras and "baz" in extras) or ("bar" not in extras and "foo" in extras)
```

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---
