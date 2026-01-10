```yaml
number: 7316
title: Add reachability markers to every lockfile node
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/marker-nodes
created_at: 2024-09-12T00:56:49Z
updated_at: 2024-09-23T23:31:40Z
url: https://github.com/astral-sh/uv/pull/7316
synced_at: 2026-01-10T12:53:44Z
```

# Add reachability markers to every lockfile node

---

_Pull request opened by @charliermarsh on 2024-09-12 00:56_

## Summary

This PR explores writing the fully-resolved markers to every node in the lockfile, rather than _just_ the node edges.


---

_Comment by @charliermarsh on 2024-09-12 01:01_

These are pretty small. I need to do more validation to understand whether they're _correct_. For example:

```toml
[[package]]
name = "datasets"
version = "2.20.0"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(python_full_version < '3.10' and platform_machine != 'aarch64' and platform_machine != 'arm64') or (python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system != 'Linux') or (python_full_version < '3.10' and platform_machine == 'arm64' and platform_system != 'Darwin')",
    "python_full_version == '3.10.*' and platform_system == 'Darwin'",
    "python_full_version == '3.10.*' and platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(python_full_version == '3.10.*' and platform_machine != 'aarch64' and platform_system != 'Darwin') or (python_full_version == '3.10.*' and platform_system != 'Darwin' and platform_system != 'Linux')",
    "python_full_version == '3.11.*' and platform_system == 'Darwin'",
    "python_full_version == '3.11.*' and platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(python_full_version == '3.11.*' and platform_machine != 'aarch64' and platform_system != 'Darwin') or (python_full_version == '3.11.*' and platform_system != 'Darwin' and platform_system != 'Linux')",
    "python_full_version == '3.12.*' and platform_system == 'Darwin'",
    "python_full_version == '3.12.*' and platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(python_full_version == '3.12.*' and platform_machine != 'aarch64' and platform_system != 'Darwin') or (python_full_version == '3.12.*' and platform_system != 'Darwin' and platform_system != 'Linux')",
    "python_full_version >= '3.13' and platform_system == 'Darwin'",
    "python_full_version >= '3.13' and platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(python_full_version >= '3.13' and platform_machine != 'aarch64' and platform_system != 'Darwin') or (python_full_version >= '3.13' and platform_system != 'Darwin' and platform_system != 'Linux')",
]
markers = "platform_machine != 'arm64' or platform_system != 'Darwin' or python_full_version >= '3.10'"
```

I _think_ `markers` should be the simplified `OR` of `resolution-markers`.

---

_Comment by @charliermarsh on 2024-09-12 01:01_

Just scanning visually, the largest I see is:

```toml
markers = "(python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')"
```

---

_Comment by @charliermarsh on 2024-09-12 01:06_

I think this doesn't _actually_ buy us much because we support multiple root nodes in the graph. So the simplified expression doesn't quite tell you exactly when a package will be included.

For example... say workspace member A depends on `black` when `sys_platform == 'win32'`. Meanwhile, member B depends on `black` for `sys_platform == 'darwin'`. The combined marker would be `sys_platform == 'win32' or sys_platform == 'darwin'`. But... the package would only be included on `sys_platform == 'darwin'` _if_ we're installing member B.


---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock/mod.rs`:1910 on 2024-09-12 13:50_

Ah interesting. I think I was naively expecting that the "full" markers would just be written in place of the markers we're writing already, instead of adding a new field.

---

_@BurntSushi reviewed on 2024-09-12 13:51_

> I think this doesn't actually buy us much because we support multiple root nodes in the graph. So the simplified expression doesn't quite tell you exactly when a package will be included.
>
> For example... say workspace member A depends on black when sys_platform == 'win32'. Meanwhile, member B depends on black for sys_platform == 'darwin'. The combined marker would be sys_platform == 'win32' or sys_platform == 'darwin'. But... the package would only be included on sys_platform == 'darwin' if we're installing member B.

Yeah I think I agree. If this doesn't enable us to do a "linear scan" then it might not be worth doing.

---

_Comment by @charliermarsh on 2024-09-23 23:31_

We decided this wasn't worthwhile right now.

---

_Closed by @charliermarsh on 2024-09-23 23:31_

---
