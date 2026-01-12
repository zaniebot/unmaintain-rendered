```yaml
number: 14153
title: "Prevent concurrent updates of the environment in `uv run`"
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: main
head: jack/run_locking
created_at: 2025-06-20T14:07:02Z
updated_at: 2025-06-21T01:11:51Z
url: https://github.com/astral-sh/uv/pull/14153
synced_at: 2026-01-12T16:11:03Z
```

# Prevent concurrent updates of the environment in `uv run`

---

_@oconnor663_

This is very similar to the locking added for `uv sync`, `uv add`, and `uv remove` in https://github.com/astral-sh/uv/pull/13869. Improving our (f)locking in general is tracked in
https://github.com/astral-sh/uv/issues/13883.

This is a minimal change that un-breaks parallel invocations of `uv run`. It's too easy to confuse locking as in "lockfile generation" and locking as in "file locking to prevent race conditions", so we probably need a broader overhaul of how these things are named. It's also too easy to miss a callsite here, or to accidentally retain one of these locks across `run_to_completion`, so ideally we could move this file locking down to a lower-level function that's shared by all these callers, possibly `pip::operations::install`. I want to get this minimal change through testing and review before I tackle either of those.

Here's a repro of the current race condition in `uv run`, which this PR fixes. Add a bunch of dependencies to provoke it, as suggested in #12751:

```
$ cd `mktemp -d`
$ uv init --package --name scratch
Initialized project `scratch`
$ uv add boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter
...
$ rm -r .venv && uv run -q scratch & uv run -q scratch
[1] 1451463
error: Failed to install: jupyterlab_widgets-3.0.15-py3-none-any.whl (jupyterlab-widgets==3.0.15)
  Caused by: failed to copy file from /home/jacko/.cache/uv/archive-v0/zXFcriPVQ1wOXqf_UUK1u/jupyterlab_widgets-3.0.15.data/data/share/jupyter/labextensions/@jupyter-widgets/jupyterlab-manager/schemas/@jupyter-widgets/jupyterlab-manager/plugin.json to /tmp/tmp.NFEGYgsQnF/.venv/lib/python3.13/site-packages/jupyterlab_widgets-3.0.15.data/data/share/jupyter/labextensions/@jupyter-widgets/jupyterlab-manager/schemas/@jupyter-widgets/jupyterlab-manager/plugin.json: No such file or directory (os error 2)
```

With this change:

```
$ rm -r .venv && ~/uv/target/fast-build/uv run -q scratch & ~/uv/target/fast-build/uv run -q scratch
[1] 1452335
Hello from scratch!
[1]  + done       ~/uv/target/fast-build/uv run -q scratch
Hello from scratch!

# Run 100 invocations in parallel.
$ rm -r .venv && for i in `seq 100` ; do ~/uv/target/fast-build/uv run -q scratch & done
[lots of job output, no failures]
```

---

_Review requested from @zanieb by @oconnor663 on 2025-06-20 14:07_

---

_Review requested from @konstin by @oconnor663 on 2025-06-20 14:07_

---

_Assigned to @zanieb by @zanieb on 2025-06-20 14:08_

---

_Comment by @oconnor663 on 2025-06-20 15:22_

I wonder if this is a flake? Reruning it. (Edit: Yep: https://github.com/astral-sh/uv/issues/14160.)
```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: run_groups_requires_python-4
    Source: crates/uv/tests/it/run.rs:4690
    ────────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
        1     1 │ exit_code: 0
        2     2 │ ----- stdout -----
        3     3 │ 
        4     4 │ ----- stderr -----
              5 │+Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
              6 │+Removed virtual environment at: .venv
              7 │+Creating virtual environment at: .venv
        5     8 │ Resolved 6 packages in [TIME]
        6       │-Audited 2 packages in [TIME]
              9 │+Installed 2 packages in [TIME]
             10 │+ + sniffio==1.3.1
             11 │+ + typing-extensions==4.10.0
    ────────────┴───────────────────────────────────────────────────────────────────
    Stopped on the first failure. Run `cargo insta test` to run all snapshots.
    test run::run_groups_requires_python ... FAILED
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:368 on 2025-06-20 16:04_

nit: we can more one statement later, just before `update_environment`

---

_@konstin approved on 2025-06-20 16:16_

---

_Comment by @oconnor663 on 2025-06-20 20:23_

Manual testing for the two script cases. First unlocked:

```
$ cd `mktemp -d`
$ uv init --script myscript.py
Initialized script at `myscript.py`
$ uv add --script myscript.py boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter
Updated `myscript.py`
$ for i in `seq 10` ; do
    uv run myscript.py &
done
[...lots of job output...]
error: Failed to install: widgetsnbextension-4.0.14-py3-none-any.whl (widgetsnbextension==4.0.14)
  Caused by: failed to rename file from /home/jacko/.cache/uv/environments-v2/myscript-e99e98cc3e25a2bf/lib/python3.13/site-packages/.tmpRU8hBM/widgetsnbextension.json to /home/jacko/.cache/uv/environments-v2/mys
cript-e99e98cc3e25a2bf/lib/python3.13/site-packages/widgetsnbextension-4.0.14.data/data/etc/jupyter/nbconfig/notebook.d/widgetsnbextension.json: No such file or directory (os error 2)
[...lots more errors...]
```

Fixed with this PR:

```
$ uv init --script myscript.py
Initialized script at `myscript.py`
$ uv add --script myscript.py boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter
Updated `myscript.py`
$ for i in `seq 10` ; do
    ~/uv/target/fast-build/uv run myscript.py &
done
[...job output, no errors...]
```

----

And now the locked case:

```
$ uv init --script myscript.py
Initialized script at `myscript.py`
$ uv add --script myscript.py boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter
Updated `myscript.py`
$ uv lock --script myscript.py
Resolved 124 packages in 74ms
$ for i in `seq 10` ; do
    uv run myscript.py &
done
[...lots of job output...]
error: Failed to install: jupyterlab_widgets-3.0.15-py3-none-any.whl (jupyterlab-widgets==3.0.15)
  Caused by: failed to remove directory `/home/jacko/.cache/uv/environments-v2/myscript-ae497719a2d6056a/lib/python3.13/site-packages/jupyterlab_widgets-3.0.15.data`: Directory not empty (os error 39)
[...lots more errors...]
```

Fixed with this PR:

```
$ uv init --script myscript.py
Initialized script at `myscript.py`
$ uv add --script myscript.py boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter
Updated `myscript.py`
$ uv lock --script myscript.py
Resolved 124 packages in 12ms
$ for i in `seq 10` ; do
    ~/uv/target/fast-build/uv run myscript.py &
done
[...job output, no errors...]
```

---

_Merged by @oconnor663 on 2025-06-20 20:34_

---

_Closed by @oconnor663 on 2025-06-20 20:34_

---

_Branch deleted on 2025-06-20 20:34_

---

_Comment by @zanieb on 2025-06-20 20:38_

As a minor note, I'd title this to be user-facing so it makes sense in the changelog

What's the actual user-facing change? "Lock the Python environment during updates in `uv run`"? or "Prevent concurrent changes to the environment during `uv run`"?

---

_Comment by @oconnor663 on 2025-06-20 20:43_

Got it. Yes, I'd go with either of your descriptions above, or maybe "lock the virtual environment when `uv run` does an implicit sync, to prevent concurrent modifications".

---

_Renamed from "(f)lock during `uv run`" to "Prevent concurrent updates of the environment in `uv run`" by @zanieb on 2025-06-21 01:11_

---
