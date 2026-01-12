```yaml
number: 4687
title: Markers of splits in universal bluejay resolution are wrong
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-01T11:17:56Z
updated_at: 2024-07-15T17:09:02Z
url: https://github.com/astral-sh/uv/issues/4687
synced_at: 2026-01-12T15:58:51Z
```

# Markers of splits in universal bluejay resolution are wrong

---

_@konstin_

When resolving transformers, the marker expressions on the forks are non-sensical:

```
DEBUG Splitting resolution on opencv-python over numpy
DEBUG Pre-fork split universal took 0.017s
DEBUG Solving split python_version >= '3.12' and platform_machine == 'aarch64' and platform_system == 'Darwin' and platform_system == 'Linux'
DEBUG Split python_version >= '3.12' and platform_machine == 'aarch64' and platform_system == 'Darwin' and platform_system == 'Linux' took 1.075s
DEBUG Solving split python_version == '3.9' and platform_machine == 'arm64' and platform_system == 'Darwin'
DEBUG Split python_version == '3.9' and platform_machine == 'arm64' and platform_system == 'Darwin' took 0.270s
```

`platform_system == 'Darwin' and platform_system == 'Linux'` is ∅, i.e. those split can never be reached, and the union of the splits is not universal.

I've tried to minimize is a bit, but the problem did not occur when using `uv pip install --universal` or when using direct dependencies with `tool.uv.source, i.e. i think it needs some previous (dummy?) splits, and hence the half-done MRE: 

Use this [pyproject.toml](https://gist.github.com/konstin/c74566bbe4bd83be711f231ba894e5e7), edit the overrides to point to two dummy projects. `opencv-python` splits, `foo` is our reporter.

```toml
[project]
name = "opencv-python"
version = "4.10.0.84"
description = "Default template for PDM package"
authors = [
    {name = "konstin", email = "konstin@mailbox.org"},
]
dependencies = [
    "cmake>=3.1",
    'numpy >=1.13.3 ; python_version < "3.7"',
    'numpy >=1.21.0 ; python_version <= "3.9" and platform_system == "Darwin" and platform_machine == "arm64"',
    'numpy >=1.21.2 ; python_version >= "3.10"',
    'numpy >=1.21.4 ; python_version >= "3.10" and platform_system == "Darwin"',
    'numpy >=1.23.5 ; python_version >= "3.11"',
    'numpy >=1.26.0 ; python_version >= "3.12"',
    'numpy >=1.19.3 ; python_version >= "3.6" and platform_system == "Linux" and platform_machine == "aarch64"',
    'numpy >=1.17.0 ; python_version >= "3.7"',
    'numpy >=1.17.3 ; python_version >= "3.8"',
    'numpy >=1.19.3 ; python_version >= "3.9"',
    "foo"
]
requires-python = ">=3.9"
license = {text = "MIT"}
```

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Default template for PDM package"
authors = [
    {name = "konstin", email = "konstin@mailbox.org"},
]
dependencies = [
    "anyio==4.3.0; platform_machine == 'x86_64'",
    "maturin>=1,<2"
]
requires-python = ">=3.9"
license = {text = "MIT"}
```

Run this on a 64-bit x86 machine or edit the marker to something else that's your machine but excluded in the lock markers. Run `uv lock -v`. In the lockfile we find

```toml
[[distribution]]
name = "foo"
version = "0.1.0"
source = { directory = "/home/konsti/projects/transformers/foo" }
dependencies = [
    { name = "maturin" },
]
```
and anyio 4.4.0, missing our constraint! It looks like we're only solving for a subset of markers in this case.

The marker on opencv-python -> numpy also looks wrong:

```toml
[[distribution]]
name = "opencv-python"
version = "4.10.0.84"
source = { directory = "/home/konsti/projects/transformers/opencv-python" }
dependencies = [
    { name = "cmake" },
    { name = "foo" },
    { name = "numpy", marker = "python_version >= '3.7' or (python_version <= '3.9' and platform_machine == 'arm64' and platform_system == 'Darwin') or (python_version >= '3.6' and platform_machine == 'aarch64' and platform_system == 'Linux')" },
]
```

---

_Label `bug` added by @konstin on 2024-07-01 11:17_

---

_Label `preview` added by @konstin on 2024-07-01 11:17_

---

_Comment by @charliermarsh on 2024-07-01 11:50_

Can you break this down a bit? It seems like you’re reporting multiple issues here - or am I misreading?

---

_Comment by @konstin on 2024-07-01 11:51_

No i think this is one issue where we accumulate the wrong markers and then everything downstream is wrong

---

_Comment by @charliermarsh on 2024-07-01 12:19_

Do you want to look into it?

---

_Assigned to @konstin by @konstin on 2024-07-01 12:41_

---

_Comment by @charliermarsh on 2024-07-02 02:11_

My guess is... maybe we aren't checking if the fork is disjoint with the _parent_ fork? Like, after we fork, we do:

```rust
forked_state.markers.and(fork.markers);
forked_state.markers = normalize(forked_state.markers)
    .unwrap_or(MarkerTree::And(Vec::new()));
```

But _that_ expression could be `∅`.


---

_Comment by @charliermarsh on 2024-07-02 02:16_

Although in theory we filter out deps with `possible_to_satisfy_markers`...

---

_Unassigned @konstin by @konstin on 2024-07-08 11:49_

---

_Comment by @konstin on 2024-07-08 12:09_

Solving this should also fix the following entry in the transformers lockfile:

```toml
[[distribution]]
name = "orbax-checkpoint"
version = "0.5.16"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/05/f3/39988acde4fa04b0017b8dec1c2cc4429c4d54830f418c80574ff6f6ff9b/orbax_checkpoint-0.5.16.tar.gz", hash = "sha256:c64d2a3ba6dd7e41330c3efd2bb669743480e62ad242eac5517e67369c26a38e", size = 160749 }
dependencies = [
    { name = "absl-py" },
    { name = "etils", version = "1.5.2", source = { registry = "https://pypi.org/simple" } },
    { name = "etils", version = "1.5.2", source = { registry = "https://pypi.org/simple" }, extra = "epath" },
    { name = "etils", version = "1.5.2", source = { registry = "https://pypi.org/simple" }, extra = "epy" },
    { name = "etils", version = "1.9.2", source = { registry = "https://pypi.org/simple" } },
    { name = "etils", version = "1.9.2", source = { registry = "https://pypi.org/simple" }, extra = "epath" },
    { name = "etils", version = "1.9.2", source = { registry = "https://pypi.org/simple" }, extra = "epy" },
    { name = "jax" },
    { name = "jaxlib" },
    { name = "msgpack" },
    { name = "nest-asyncio" },
    { name = "numpy" },
    { name = "protobuf" },
    { name = "pyyaml" },
    { name = "tensorstore" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/50/00/95794fe517d705c06b1918c4d32372d7476f81a2bfc6ed44f850bbe71ee2/orbax_checkpoint-0.5.16-py3-none-any.whl", hash = "sha256:1b4329ff477c59e061ac15bc09eea34a248f2048b1c5b43eae9b530e068e5974", size = 217032 },
]
```

---

_Assigned to @BurntSushi by @BurntSushi on 2024-07-08 13:54_

---

_Closed by @BurntSushi on 2024-07-15 17:09_

---
