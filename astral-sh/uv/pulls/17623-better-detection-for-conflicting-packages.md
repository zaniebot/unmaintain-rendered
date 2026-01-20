```yaml
number: 17623
title: Better detection for conflicting packages
type: pull_request
state: open
author: konstin
labels:
  - preview
assignees: []
base: main
head: konsti/better-conflict-detection
created_at: 2026-01-20T13:56:12Z
updated_at: 2026-01-20T23:25:45Z
url: https://github.com/astral-sh/uv/pull/17623
synced_at: 2026-01-20T23:51:56Z
```

# Better detection for conflicting packages

---

_@konstin_

In the previous iteration, conflict detection was based on top level modules. This would work if all namespace packages correctly omitted the `__init__.py`, but e.g. the nvidia packages include an empty `nvida/__init__.py`.

Instead, we track overlapping top level modules during installation and perform conflict analysis after installation, recursing only as far as necessary.

See https://github.com/astral-sh/uv/pull/13437 for a list of reported conflicts.

Before:
```
$ uv venv -c -q && uv pip install --preview nvidia-nvjitlink-cu12==12.8.93 nvidia-nvtx-cu12==12.8.90
  Resolved 2 packages in 0.99ms
  ░░░░░░░░░░░░░░░░░░░░ [0/2] Installing wheels...                                                    warning: The module `nvidia` is provided by more than one package, which causes an install race condition and can result in a broken module. Consider removing your dependency on either `nvidia-nvtx-cu12` (v12.8.90) or `nvidia-nvjitlink-cu12` (v12.8.93).
  Installed 2 packages in 3ms
   + nvidia-nvjitlink-cu12==12.8.93
   + nvidia-nvtx-cu12==12.8.90
```

After:
```
$ uv venv -c -q && cargo run -q pip install --preview nvidia-nvjitlink-cu12==12.8.93 nvidia-nvtx-cu12==12.8.90
  Resolved 2 packages in 3ms
  Installed 2 packages in 4ms
   + nvidia-nvjitlink-cu12==12.8.93
   + nvidia-nvtx-cu12==12.8.90
```

Still detected true positive:
```
$ uv venv -c -q && cargo run -q pip install --no-progress opencv-python opencv-contrib-python --no-build --no-deps --preview
  Resolved 2 packages in 5ms
warning: The file `cv2/__init__.pyi` is provided by more than one package, which causes an install race condition and can result in a broken module. Packages containing the file:
* opencv-contrib-python (opencv_contrib_python-4.13.0.90-cp37-abi3-manylinux_2_28_x86_64.whl)
* opencv-python (opencv_python-4.13.0.90-cp37-abi3-manylinux_2_28_x86_64.whl)
Installed 2 packages in 6ms
 + opencv-contrib-python==4.13.0.90
 + opencv-python==4.13.0.90
```

---

_Review requested from @EliteTK by @konstin on 2026-01-20 13:56_

---

_Label `preview` added by @konstin on 2026-01-20 13:56_

---

_Review comment by @EliteTK on `crates/uv/tests/it/pip_install.rs`:13402 on 2026-01-20 13:59_

This comment seems to contradict the output now.

---

_Review comment by @EliteTK on `crates/uv/tests/it/pip_install.rs`:13362 on 2026-01-20 13:59_

```suggestion
    // This file collides too, but we always show the first file.
```

Typo I think?

---

_Review comment by @EliteTK on `crates/uv/tests/it/pip_install.rs`:13394 on 2026-01-20 13:59_

```suggestion
    // This file collides too, but we always show the first file.
```

ditto

---

_Review comment by @EliteTK on `crates/uv-installer/src/installer.rs`:202 on 2026-01-20 14:04_

I think we should print the error chain here. Similarly to this:

https://github.com/astral-sh/uv/blob/f793b182edab2fea4a9d37fcef3cb4546de583b1/crates/uv-python/src/discovery.rs#L1565-L1574

I was going to say we could centralise the implementation... But there's no direct way to do that.

Maybe for now just merge as is, or copy the code and adjust it, and then I will propose something as a PR later...

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:28 on 2026-01-20 18:54_

Maybe the `struct` should itself also be renamed to more closely reflect its use... Although I am not sure what that name should be.

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:62 on 2026-01-20 22:18_

Since this ends up being turned into a `Path` on the other side, I think it makes sense to make the key type here a `PathBuf` and avoid the noise.

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:156 on 2026-01-20 22:43_

We never pass `files` outside of this function so it can hold references.

```suggestion
                        .insert((wheel, dir_entry.metadata()?.len()));
```

You'll need to edit the type signature above too (although you can switch the key types to placeholders as they can be inferred).

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:161 on 2026-01-20 22:43_

```suggestion
                        .insert((wheel.clone(), dir_entry.path()));
```

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:161 on 2026-01-20 22:46_

Technically the wheel clone could be dropped here too if you feel like it. But it would require some reshuffling so not sure it's totally worth it.

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:124 on 2026-01-20 23:03_

It seems justifiable to compare contents lazily if the sizes match and aren't zero.

It would only happen when there are overlapping files (rare) with identical sizes (rarer). But it would ensure that we're not under-reporting in extremely rare cases which definitely _will_ cause a problem.

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:57 on 2026-01-20 23:16_

Given the doc comment, it's kind of confusing for it to take "relative" (`&Path`) at all.

I think one of these may be preferable:

* It could take an absolute path and a module name (i.e. `Component`) and the responsibility on passing just the module name should fall on the parent.
* The function could just be renamed and the doc comment reworded?

---

_Review comment by @EliteTK on `crates/uv-install-wheel/src/linker.rs`:148 on 2026-01-20 23:23_

```suggestion
```

Sorting here doesn't affect the iteration order for `BTreeMap`.

---

_@EliteTK approved on 2026-01-20 23:25_

---
