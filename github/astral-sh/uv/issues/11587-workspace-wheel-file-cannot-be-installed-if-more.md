---
number: 11587
title: Workspace wheel file cannot be installed if more than one workspace subpackage dependencies are specified
type: issue
state: open
author: boccileonardo
labels:
  - question
assignees: []
created_at: 2025-02-18T06:58:03Z
updated_at: 2025-02-19T13:57:13Z
url: https://github.com/astral-sh/uv/issues/11587
synced_at: 2026-01-07T13:12:18-06:00
---

# Workspace wheel file cannot be installed if more than one workspace subpackage dependencies are specified

---

_Issue opened by @boccileonardo on 2025-02-18 06:58_

### Summary

In a workspace package, the wheel resulting from uv build can be installed if only one sub-package is defined.
However, from 2 subpackage workspace dependencies, the wheel dependencies cannot be resolved, failing with error:

No solution found when resolving dependencies:
  ╰─▶ Because packtwo was not found in the package registry and uv-pack==0.1.0 depends on packtwo, we can conclude that
      uv-pack==0.1.0 cannot be used.
      And because only uv-pack==0.1.0 is available and your project depends on uv-pack, we can conclude that your project's
      requirements are unsatisfiable.

MRE:
```
~$ mkdir uv_pack
~$ cd uv_pack
~/uv_pack $ uv init --package
~/uv_pack $ mkdir packages && mkdir packages/packone && cd packages/packone
~/uv_pack/packages/packone $ uv init --package
~/uv_pack/packages/packone $ cd ../../
~/uv_pack $ mkdir packages/packtwo && cd packages/packtwo
~/uv_pack/packages/packtwo $ uv init --package
~/uv_pack/packages/packone $ cd ../../
```

```
#~uv_pack/pyproject.toml
...
dependencies = [
    "packone",
    "packtwo",
]

[tool.uv.sources]
packone = { workspace = true }
packtwo = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
...
```

```
~/uv_pack $ uv build --wheel
```

The resulting wheel installed in a different env causes the error above, but running this command in the same env produces no errors:
uv run --with uv_pack --no-project -- python -c "import uv_pack"

### Platform

Ubuntu on WSL

### Version

uv 0.6.1

### Python version

3.10.12

---

_Label `bug` added by @boccileonardo on 2025-02-18 06:58_

---

_Comment by @charliermarsh on 2025-02-18 14:26_

The wheel built by `uv build` does not include `tool.uv.sources` -- it only includes the standards-based metadata in the `[project]` table. So if you try to install that built wheel later, you would need to provide the command with some way to understand where it should get `packone`, `packtwo`, etc.

---

_Comment by @charliermarsh on 2025-02-18 14:28_

I believe it's a duplicate of https://github.com/astral-sh/uv/issues/10602 which we're tracking in https://github.com/astral-sh/uv/issues/8729.

---

_Label `bug` removed by @charliermarsh on 2025-02-18 14:28_

---

_Label `question` added by @charliermarsh on 2025-02-18 14:28_

---

_Comment by @boccileonardo on 2025-02-19 13:56_

I don't quite understand why it works with just one workspace package though, and not with 2.
If I understood the linked issue correctly, you would expect workspace packages to never be installed when a wheel is built? This doesn't seem right to me, as I would not see a point of ever building and distributing the top-level package then.

For this specific case, I just checked again in a clean env that as long as there is only one workspace dependency, it installs just fine:
```
leo@5CG2145M95-W10:~/exploration$ uv add uv_pack-0.1.0-py3-none-any.whl
Resolved 3 packages in 11ms
Installed 2 packages in 13ms
 + packone==0.1
 + uv-pack==0.1.0 (from file:///home/leo/exploration/uv_pack-0.1.0-py3-none-any.whl)
```

It only fails if the second workspace dependency is added, which is why this seems like a bug to me.
```
leo@5CG2145M95-W10:~/exploration$ uv add uv_pack-0.1.0-py3-none-any.whl
  × No solution found when resolving dependencies:
  ╰─▶ Because packtwo was not found in the package registry and uv-pack==0.1.0 depends on packtwo, we can conclude that
      uv-pack==0.1.0 cannot be used.
      And because only uv-pack==0.1.0 is available and your project depends on uv-pack, we can conclude that your project's
      requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

---

_Comment by @boccileonardo on 2025-02-19 13:57_

Or, if you meant that the installation is expected to fail if one tries to use other installation methods that are not UV, then I understand, but it's then not related to this case, since I'm adding the wheel using uv

---
