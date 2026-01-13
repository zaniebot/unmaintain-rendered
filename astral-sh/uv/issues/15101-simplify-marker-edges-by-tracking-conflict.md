```yaml
number: 15101
title: Simplify marker edges by tracking conflict reachability on nodes
type: issue
state: open
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2025-08-06T09:07:33Z
updated_at: 2026-01-13T16:56:02Z
url: https://github.com/astral-sh/uv/issues/15101
synced_at: 2026-01-13T17:25:45Z
```

# Simplify marker edges by tracking conflict reachability on nodes

---

_@konstin_

Currently, we track conflict inferences in `simplify_conflict_markers` independently of the markers, and we don't track conflict markers in reachability. This limits our power in simplifying edges. Instead, we should compute the reachability for each node including the conflict markers that need to be accessed to reach a node, than simplify each edge using the reachability marker.

Additionally, we should remove all parts of markers not captured by conflicts more effectively. For example, we currently see `(extra == 'extra-3-pkg-x1' and extra == 'extra-3-pkg-x2')` markers in the lockfile, even though `x1` and `x2` conflict. The current imbibe function does not seem to remove those. One approach is to remove them using the DNF, but we should avoid the transformation to DNF. In the DNF, we can remove any clause where two (positive) variables refer to two extra which conflict.

**Example**

```toml
[project]
name = "debug"
version = "0.0.1"
requires-python = "==3.12.*"

[project.optional-dependencies]
a = [
  "python-dateutil==2.8.0",
]
b = [
  "python-dateutil==2.8.1; platform_machine != 'inapplicable'",
  "python-dateutil==2.8.0; platform_machine == 'inapplicable'",
]

[tool.uv]
conflicts = [
  [
    { extra = "a" },
    { extra = "b" },
  ],
]
```

```toml
[[package]]
name = "python-dateutil"
version = "2.8.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_machine == 'inapplicable' and extra != 'extra-5-debug-a' and extra == 'extra-5-debug-b'",
    "extra == 'extra-5-debug-a' and extra != 'extra-5-debug-b'",
]
dependencies = [
    { name = "six", marker = "(platform_machine == 'inapplicable' and extra == 'extra-5-debug-b') or extra == 'extra-5-debug-a'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/ad/99/5b2e99737edeb28c71bcbec5b5dda19d0d9ef3ca3e92e3e925e7c0bb364c/python-dateutil-2.8.0.tar.gz", hash = "sha256:c89805f6f4d64db21ed966fda138f8a5ed7a4fdbc1a8ee329ce1b74e3c74da9e", size = 327134, upload-time = "2019-02-05T14:12:37.493Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/41/17/c62faccbfbd163c7f57f3844689e3a78bae1f403648a6afb1d0866d87fbb/python_dateutil-2.8.0-py2.py3-none-any.whl", hash = "sha256:7e6584c74aeed623791615e26efd690f29817a27c73085b78e4bad02493df2fb", size = 226803, upload-time = "2019-02-05T14:12:35.322Z" },
]
```

The `six` marker is exactly the reachability of python-dateutil 2.8.0, we should be able to elide it.

---

_Label `enhancement` added by @konstin on 2025-08-06 09:07_

---

_Comment by @graipher on 2026-01-13 16:54_

I was about to post a similar issue, which occurs when following the [docs regarding choosing a pytorch backend using extra markers](https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies):

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = "==3.13.*"
dependencies = []

[project.optional-dependencies]
cpu = [
  "torch>=2.7.1",
  "torchvision>=0.22.1",
]
cu130 = [
  "torch>=2.7.1",
  "torchvision>=0.22.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu130" },
  ],
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://download.pytorch.org/whl/cu130"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu130", extra = "cu130" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu130", extra = "cu130" },
]
```

This also leads to impossible marker branches (thankfully also as branches that do not matter):

```
...
[[package]]
name = "foo"
version = "0.1.0"
source = { virtual = "." }

[package.optional-dependencies]
cpu = [
    { name = "torch", version = "2.9.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(sys_platform == 'darwin' and extra == 'extra-3-foo-cpu') or (extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
    { name = "torch", version = "2.9.1+cpu", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(sys_platform != 'darwin' and extra == 'extra-3-foo-cpu') or (extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
    { name = "torchvision", version = "0.24.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(sys_platform == 'darwin' and extra == 'extra-3-foo-cpu') or (sys_platform == 'linux' and extra == 'extra-3-foo-cpu') or (extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
    { name = "torchvision", version = "0.24.1+d801a34", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(sys_platform != 'darwin' and sys_platform != 'linux' and extra == 'extra-3-foo-cpu') or (sys_platform == 'darwin' and extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130') or (sys_platform == 'linux' and extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
]
cu130 = [
    { name = "torch", version = "2.9.1+cu130", source = { registry = "https://download.pytorch.org/whl/cu130" } },
    { name = "torchvision", version = "0.24.1", source = { registry = "https://download.pytorch.org/whl/cu130" }, marker = "(platform_machine == 'aarch64' and platform_python_implementation == 'CPython' and sys_platform == 'linux' and extra == 'extra-3-foo-cu130') or (platform_machine != 'aarch64' and extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130') or (platform_python_implementation != 'CPython' and extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130') or (sys_platform != 'linux' and extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
    { name = "torchvision", version = "0.24.1+cu130", source = { registry = "https://download.pytorch.org/whl/cu130" }, marker = "(platform_machine != 'aarch64' and extra == 'extra-3-foo-cu130') or (platform_python_implementation != 'CPython' and extra == 'extra-3-foo-cu130') or (sys_platform != 'linux' and extra == 'extra-3-foo-cu130') or (extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')" },
]
...
```

Here e.g. `(sys_platform == 'darwin' and extra == 'extra-3-foo-cpu') or (extra == 'extra-3-foo-cpu' and extra == 'extra-3-foo-cu130')` is equivalent to `sys_platform == 'darwin' and extra == 'extra-3-foo-cpu'` because of the configured `conflicts`.

---
