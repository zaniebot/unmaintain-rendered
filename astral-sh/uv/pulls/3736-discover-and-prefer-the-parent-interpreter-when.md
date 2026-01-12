```yaml
number: 3736
title: "Discover and prefer the parent interpreter when invoked with `python -m uv`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/parent-interp
created_at: 2024-05-22T14:12:33Z
updated_at: 2024-05-22T16:34:25Z
url: https://github.com/astral-sh/uv/pull/3736
synced_at: 2026-01-12T16:05:49Z
```

# Discover and prefer the parent interpreter when invoked with `python -m uv`

---

_@zanieb_

Closes #2222
Closes https://github.com/astral-sh/uv/issues/2058
Replaces https://github.com/astral-sh/uv/pull/2338
See also https://github.com/astral-sh/uv/issues/2649

We use an environment variable (`UV_INTERNAL__PARENT_INTERPRETER`) to track the invoking interpreter when `python -m uv` is used. The parent interpreter is preferred over all other sources (though it will be skipped if it does not meet a `--python` request or if `--system` is used and it belongs to a virtual environment). We warn if `--system` is not provided and this interpreter would mutate system packages, but allow it.

---

_Label `enhancement` added by @zanieb on 2024-05-22 14:12_

---

_Renamed from "Discover and prefer the parent interpreter when invoked with `python -m uv ...`" to "Discover and prefer the parent interpreter when invoked with `python -m uv`" by @zanieb on 2024-05-22 14:13_

---

_@zanieb reviewed on 2024-05-22 14:18_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:43 on 2024-05-22 14:18_

Should we do a real `warn_user_once` here?

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:43 on 2024-05-22 14:18_

(I think it may be overkill)

---

_@zanieb reviewed on 2024-05-22 14:18_

---

_Marked ready for review by @zanieb on 2024-05-22 14:18_

---

_@zanieb reviewed on 2024-05-22 14:20_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 14:20_

This is in the `SystemPython::Required` clause. Maybe I should move it out? This may be a bit weird if you do `./.venv/bin/python -m uv pip install --system` as we'll still find this Python... but also a bit weird if you do `/sys/python -m uv --system` and we find a different Python?

---

_Review comment by @konstin on `python/uv/__main__.py`:35 on 2024-05-22 15:07_

Can we use something other than the double underscore, like a name that is so long that no one likes to use it outside that script?

---

_@konstin approved on 2024-05-22 15:09_

---

_@zanieb reviewed on 2024-05-22 15:12_

---

_Review comment by @zanieb on `python/uv/__main__.py`:35 on 2024-05-22 15:12_

I feel like the double underscore is sufficient for saying it's not user facing and we're not going to document or support it elsewhere. Idk about making the name arbitrarily longer.... I guess I could do `UV_PRIVATE__...`?

---

_@zanieb reviewed on 2024-05-22 15:20_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 15:20_

I think I can fix this with some changes to discovery

---

_@charliermarsh reviewed on 2024-05-22 15:25_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/environment.rs`:43 on 2024-05-22 15:25_

Can you use `env.interpreter().is_virtualenv()`?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 15:30_

Is the ideal behavior that:

- `./.venv/bin/python -m uv pip install --system` does not find `.venv/bin/python`
- `/sys/python -m uv --system` does find `/sys/python`

?

---

_@charliermarsh reviewed on 2024-05-22 15:30_

---

_@zanieb reviewed on 2024-05-22 15:56_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:43 on 2024-05-22 15:56_

Yeah I copied this from below and I don't know why it's not using that either. Will do.

---

_@zanieb reviewed on 2024-05-22 15:56_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 15:56_

I think so yea. With #3739 this will be possible

---

_@zanieb reviewed on 2024-05-22 15:56_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 15:56_

(rebasing this onto that now)

---

_@zanieb reviewed on 2024-05-22 16:18_

---

_Review comment by @zanieb on `python/uv/__main__.py`:35 on 2024-05-22 16:18_

Added more suffix

---

_@zanieb reviewed on 2024-05-22 16:19_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:1117 on 2024-05-22 16:19_

The behavior should be as described now, I'll do some QA.

---

_@zanieb reviewed on 2024-05-22 16:23_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/discovery.rs`:69 on 2024-05-22 16:23_

I decided to add an `Explicit` variant to avoid confusing internal behavior where `SystemPython::Disallowed` could still get a system Python from this environment variable.

---

_Comment by @zanieb on 2024-05-22 16:28_

e.g.

```
❯ RUST_LOG=uv=trace ./uv/.venv/bin/python -m uv -v pip install anyio
TRACE Cached interpreter info for Python 3.12.3, skipping probing: uv/.venv/bin/python
TRACE Found Python interpreter cpython 3.12.3 at /Users/zb/workspace/uv/.venv/bin/python from parent interpreter
DEBUG Using Python 3.12.3 environment at uv/.venv/bin/python
DEBUG Trying to lock if free: uv/.venv/.lock
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("anyio"), version: "4.3.0", path: "/Users/zb/workspace/uv/.venv/lib/python3.12/site-packages/anyio-4.3.0.dist-info" }) Registry { specifier: VersionSpecifiers([]), index: None }
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("sniffio"), version: "1.3.1", path: "/Users/zb/workspace/uv/.venv/lib/python3.12/site-packages/sniffio-1.3.1.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.1" }]), index: None }
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("idna"), version: "3.7", path: "/Users/zb/workspace/uv/.venv/lib/python3.12/site-packages/idna-3.7.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "2.8" }]), index: None }
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio>=1.1
DEBUG All editables satisfied: 
Audited 1 package in 1ms
```

```
❯ RUST_LOG=uv=trace ./uv/.venv/bin/python -m uv -v pip install anyio --system
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /Users/zb/bin
TRACE Checking `PATH` directory for interpreters: /Users/zb/.local/bin
TRACE Checking `PATH` directory for interpreters: /Users/zb/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /opt/homebrew/bin/python3
TRACE Found Python interpreter cpython 3.12.3 at /opt/homebrew/bin/python3 from search path
DEBUG Using Python 3.12.3 environment at /opt/homebrew/opt/python@3.12/bin/python3.12
error: The interpreter at /opt/homebrew/opt/python@3.12/Frameworks/Python.framework/Versions/3.12 is externally managed, and indicates the following:
```

```
❯ ./uv/.venv/bin/python -m uv -v pip install anyio --python 3.10
INFO Found active virtual environment (via VIRTUAL_ENV) at: /Users/zb/workspace/uv/.venv
DEBUG Ignoring Python interpreter at `/Users/zb/workspace/uv/bin/cpython-3.10.13-macos-aarch64-none/install/bin/python3`: system intepreter not explicit
DEBUG Ignoring Python interpreter at `/opt/homebrew/opt/python@3.12/bin/python3.12`: system intepreter not explicit
DEBUG Ignoring Python interpreter at `/Library/Developer/CommandLineTools/usr/bin/python3`: system intepreter not explicit
DEBUG Ignoring Python interpreter at `/opt/homebrew/opt/python@3.12/bin/python3.12`: system intepreter not explicit
error: No interpreter found for Python 3.10 in all sources
```

```
❯ /opt/homebrew/bin/python3 -m uv -v pip install anyio
DEBUG Allowing system Python interpreter at `/opt/homebrew/opt/python@3.12/bin/python3.12`
DEBUG Using Python 3.12.3 environment at /opt/homebrew/opt/python@3.12/bin/python3.12
```

---

_@charliermarsh approved on 2024-05-22 16:30_

---

_Merged by @zanieb on 2024-05-22 16:34_

---

_Closed by @zanieb on 2024-05-22 16:34_

---

_Branch deleted on 2024-05-22 16:34_

---
