---
number: 6242
title: uv sync fails with relative packages
type: issue
state: closed
author: Eyal-Shalev
labels:
  - bug
assignees: []
created_at: 2024-08-20T02:41:47Z
updated_at: 2024-08-20T05:02:29Z
url: https://github.com/astral-sh/uv/issues/6242
synced_at: 2026-01-07T13:12:17-06:00
---

# uv sync fails with relative packages

---

_Issue opened by @Eyal-Shalev on 2024-08-20 02:41_

UV Version: `0.2.37`
Platform: MacOS `14.3.1`

## TL;DR
When running `uv sync` with a lockfile and a relatively installed package the command panics with:

```
thread 'main' panicked at crates/uv-resolver/src/lock.rs:2800:76:
called `Result::unwrap()` on an `Err` value: Custom { kind: Other, error: "Trivial strip failed:  vs. /tmp/package-a.arc8vD0sMy" }
```

## Files

### `/tmp/package-a.arc8vD0sMy/pyproject.toml`

```toml
[project]
name = "package-a"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

### `/tmp/package-b.it5jIqKRdX/pyproject.toml`

```toml
[project]
name = "package-b"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "package-a @ file://${PROJECT_ROOT}/../package-a.arc8vD0sMy",
]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"
```

### `/tmp/package-b.it5jIqKRdX/uv.lock`

```toml
version = 1
requires-python = ">=3.12"

[[package]]
name = "package-a"
version = "0.1.0"
source = { directory = "/tmp/package-a.arc8vD0sMy" }

[[package]]
name = "package-b"
version = "0.1.0"
source = { editable = "." }
dependencies = [
    { name = "package-a" },
]

[package.metadata]
requires-dist = [{ name = "package-a", directory = "/tmp/package-a.arc8vD0sMy" }]
```

## Commands

```sh
cd /tmp/package-b.it5jIqKRdX
uv lock
RUST_BACKTRACE=full uv sync -vvv
```

## Output

```
    0.000103s DEBUG uv uv 0.2.37
warning: `uv sync` is experimental and may change without warning
    0.001765s DEBUG uv_workspace::workspace Found project root: `/tmp/package-b.it5jIqKRdX`
    0.001899s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.004487s DEBUG uv::commands::project The virtual environment's Python version satisfies `Python >=3.12`
 uv_client::linehaul::linehaul
    0.010079s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=package-b @ file:///tmp/package-b.it5jIqKRdX
    0.013988s DEBUG uv_fs Acquired lock for `$HOME/Library/Caches/uv/built-wheels-v3/editable/7490a518e7226923`
    0.015293s   2ms DEBUG uv_distribution::source Using cached metadata for: package-b @ file:///tmp/package-b.it5jIqKRdX
    0.015519s   2ms DEBUG uv_workspace::workspace No workspace root found, using project root
thread 'main' panicked at crates/uv-resolver/src/lock.rs:2800:76:
called `Result::unwrap()` on an `Err` value: Custom { kind: Other, error: "Trivial strip failed:  vs. /tmp/package-a.arc8vD0sMy" }
stack backtrace:
   0:        0x101031978 - __mh_execute_header
   1:        0x100d38238 - __mh_execute_header
   2:        0x101002818 - __mh_execute_header
   3:        0x1010347ac - __mh_execute_header
   4:        0x1010340c4 - __mh_execute_header
   5:        0x101035b80 - __mh_execute_header
   6:        0x101034ac4 - __mh_execute_header
   7:        0x101034a2c - __mh_execute_header
   8:        0x101034a20 - __mh_execute_header
   9:        0x101a59828 - __mh_execute_header
  10:        0x101a59bb4 - __mh_execute_header
  11:        0x101868ff4 - __mh_execute_header
  12:        0x1011e3020 - __mh_execute_header
  13:        0x1011d8258 - __mh_execute_header
  14:        0x1011cacac - __mh_execute_header
  15:        0x10117b3d0 - __mh_execute_header
  16:        0x10112dce0 - __mh_execute_header
  17:        0x10112ccf4 - __mh_execute_header
  18:        0x10124dcc0 - __mh_execute_header
  19:        0x101520c78 - __mh_execute_header
  20:        0x1014d7d20 - __mh_execute_header
  21:        0x101520710 - __mh_execute_header
```



---

_Label `bug` added by @zanieb on 2024-08-20 02:43_

---

_Comment by @charliermarsh on 2024-08-20 02:44_

I think this might already be fixed on main #6157.

---

_Comment by @zanieb on 2024-08-20 02:45_

Yeah this line of code (`let lock_path = relative_to(workspace.lock_path(), &lock_path).unwrap()`) no longer unwraps on `main`.

---

_Comment by @zanieb on 2024-08-20 02:45_

Thanks for the clear report!

---

_Comment by @Eyal-Shalev on 2024-08-20 03:06_

Is there an easy way to install uv from main to verify this ineed fixes the issue?

---

_Comment by @zanieb on 2024-08-20 03:23_

You could clone and `cargo build` or download a built artifact from the latest commit to `main`, e.g.  at the bottom of https://github.com/astral-sh/uv/actions/runs/10462793660

---

_Comment by @charliermarsh on 2024-08-20 03:27_

I can also test it out real quick.

---

_Comment by @charliermarsh on 2024-08-20 03:40_

```
❯ uv sync
Using Python 3.12.4
Creating virtualenv at: .venv
Resolved 2 packages in 1ms
   Built package-a @ file:///private/tmp/package-a.arc8vD0sMy
   Built package-b @ file:///private/tmp/package-b.it5jIqKRdX
Prepared 2 packages in 528ms
Installed 2 packages in 1ms
 + package-a==0.1.0 (from file:///private/tmp/package-a.arc8vD0sMy)
 + package-b==0.1.0 (from file:///private/tmp/package-b.it5jIqKRdX)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-20 03:40_

---

_Comment by @charliermarsh on 2024-08-20 03:40_

And confirmed that latest on PyPI fails:

```
❯ python -m uv sync
warning: `uv sync` is experimental and may change without warning
thread 'main' panicked at crates/uv-resolver/src/lock.rs:2800:76:
called `Result::unwrap()` on an `Err` value: Custom { kind: Other, error: "Trivial strip failed:  vs. /private/tmp/package-a.arc8vD0sMy" }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Closed by @charliermarsh on 2024-08-20 03:40_

---

_Comment by @Eyal-Shalev on 2024-08-20 04:21_

Yes. I confirmed it as well.

Though I encountered a different issue with `uv lock` that causes `uv sync` to fail when one of the dependencies (package-b) uses poetry and relies relatively on a different package (package-c). 🫠

I'll open another issue later though.

---

_Comment by @charliermarsh on 2024-08-20 04:25_

Feel free! Relative path dependencies are explicitly not standardized, and the use of `${PROJECT_ROOT}` above is also non-standard. In general I'd probably suggest using `tool.uv.sources` to express these relationships?

---

_Comment by @eth3lbert on 2024-08-20 04:38_

> You could clone and `cargo build` or download a built artifact from the latest commit to `main`, e.g. at the bottom of https://github.com/astral-sh/uv/actions/runs/10462793660

It might be interesting to provide a nightly built artifact.

---

_Comment by @zanieb on 2024-08-20 04:51_

@eth3lbert we've considered it, but we release a few times a week already

---

_Comment by @Eyal-Shalev on 2024-08-20 04:54_

> Feel free! Relative path dependencies are explicitly not standardized, and the use of `${PROJECT_ROOT}` above is also non-standard. In general I'd probably suggest using `tool.uv.sources` to express these relationships?

These bugs happen when I use `tool.uv.sources` as well.

---

_Comment by @eth3lbert on 2024-08-20 04:59_

> @eth3lbert we've considered it, but we release a few times a week already

Yeah, that makes sense!
I think for those who want to debug or test the latest successful build, a link that redirects to the artifacts from the latest successfully merged action would be sufficient.

---

_Comment by @zanieb on 2024-08-20 05:02_

Yeah unfortunately those builds aren't release-optimized so I'd be hesitant to make them easy to access. Idk. Maybe we'll add nightly builds — seems better than doing a release build for every commit to `main`.

---
