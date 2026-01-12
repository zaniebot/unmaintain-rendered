```yaml
number: 7756
title: "Fill in `authors` filed during `uv init`"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: init-author
created_at: 2024-09-28T11:34:39Z
updated_at: 2024-10-13T09:14:05Z
url: https://github.com/astral-sh/uv/pull/7756
synced_at: 2026-01-12T16:07:59Z
```

# Fill in `authors` filed during `uv init`

---

_@j178_

## Summary

Fill in the `authors` field of `pyproject.toml` by fetching author info from Git.

Resolves #7718

---

_Marked ready for review by @j178 on 2024-09-28 12:54_

---

_Review comment by @edmorley on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-29 08:00_

Hi! For non-packaged application project types, I would probably end up deleting the author info most of the time.

What do you think about only adding it for libraries + package=true app types? ie: Putting this block inside an `if package` conditional?

---

_@edmorley reviewed on 2024-09-29 08:00_

---

_@zanieb reviewed on 2024-09-29 15:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-29 15:29_

That seems reasonable to me, I think. If we do this, we should add an `--authors` flag too and allow it to force them to be added.

---

_@zanieb reviewed on 2024-09-29 15:31_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:712 on 2024-09-29 15:31_

What if the VCS isn't Git?

---

_@j178 reviewed on 2024-09-30 07:19_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:712 on 2024-09-30 07:19_

We could just use Git as a source of information (a common one), it is valid information even if current VCS is not Git.

---

_@j178 reviewed on 2024-09-30 07:23_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:712 on 2024-09-30 07:23_

Is it worthwhile to add a new dependency like [whoami](https://crates.io/crates/whoami) or [users](https://crates.io/crates/users) to fetch the OS username as a fallabck method?

---

_Comment by @j178 on 2024-09-30 07:26_

The windows failure seems to be a flaky test:
```console
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: uv_run_isolate
Source: E:\uv:538
───────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────
    0       │-success: true
    1       │-exit_code: 0
          0 │+success: false
          1 │+exit_code: 2
    2     2 │ ----- stdout -----
    3       │-Success
    4     3 │ 
    5     4 │ ----- stderr -----
    6     5 │ warning: `VIRTUAL_ENV=[VENV]/` does not match the project environment path `.venv` and will be ignored
    7     6 │ Using CPython 3.12.[X] interpreter at: [PYTHON-3.12]
    8     7 │ Creating virtual environment at: .venv
    9     8 │ Resolved 8 packages in [TIME]
   10       │-Prepared 7 packages in [TIME]
   11       │-Installed 7 packages in [TIME]
   12       │- + albatross==0.1.0 (from file://[TEMP_DIR]/albatross-root-workspace)
   13       │- + anyio==4.3.0
   14       │- + bird-feeder==1.0.0 (from file://[TEMP_DIR]/albatross-root-workspace/packages/bird-feeder)
   15       │- + idna==3.6
   16       │- + seeds==1.0.0 (from file://[TEMP_DIR]/albatross-root-workspace/packages/seeds)
   17       │- + sniffio==1.3.1
   18       │- + tqdm==4.66.2
          9 │+error: Failed to prepare distributions
         10 │+  Caused by: Failed to fetch wheel: seeds @ file://[TEMP_DIR]/albatross-root-workspace/packages/seeds
         11 │+  Caused by: Build backend failed to build editable through `build_editable()` with exit code: 1
         12 │+--- stdout:
         13 │+
         14 │+--- stderr:
         15 │+Traceback (most recent call last):
         16 │+  File "<string>", line 11, in <module>
         17 │+  File "[CACHE_DIR]/builds-v0/[TMP]/build.py", line 83, in build_editable
         18 │+    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
         19 │+                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         20 │+  File "[CACHE_DIR]/builds-v0/[TMP]/interface.py", line 155, in build
         21 │+    artifact = version_api[version](directory, **build_data)
         22 │+               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         23 │+  File "[CACHE_DIR]/builds-v0/[TMP]/wheel.py", line 434, in build_editable
         24 │+    return self.build_editable_detection(directory, **build_data)
         25 │+           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         26 │+  File "[CACHE_DIR]/builds-v0/[TMP]/wheel.py", line 437, in build_editable_detection
         27 │+    from editables import EditableProject
         28 │+  File "<frozen importlib._bootstrap>", line 1360, in _find_and_load
         29 │+  File "<frozen importlib._bootstrap>", line 1331, in _find_and_load_unlocked
         30 │+  File "<frozen importlib._bootstrap>", line 935, in _load_unlocked
         31 │+  File "<frozen importlib._bootstrap_external>", line 991, in exec_module
         32 │+  File "<frozen importlib._bootstrap_external>", line 1128, in get_code
         33 │+  File "<frozen importlib._bootstrap_external>", line 1186, in get_data
         34 │+PermissionError: [Errno 13] Permission denied: 'E:\/uv-tmp\/[TMP]/__init__.py'
         35 │+---
────────────┴──────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test test_uv_run_isolate ... FAILED
```

---

_@j178 reviewed on 2024-09-30 07:32_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-30 07:32_

Regarding `--authors`, shoule we enable it to accept a value? For example: `uv init --authors 'Alice <alice@exmaple.org>'` or `uv init --authors Alice`.

---

_@zanieb reviewed on 2024-09-30 16:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-30 16:49_

That's... a good question. It makes sense to allow providing the authors, but also it breaks our typical `--no-<flag>` / `--<flag>` pattern. I think we'd need like... `--authors git` as a special value that says fetch from git?

---

_@j178 reviewed on 2024-09-30 17:02_

---

_Review comment by @j178 on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-30 17:02_

or rename this flag as something like `--fill-authors` / `--no-fill-authors` and keep `--authors` for future usage?

---

_@zanieb reviewed on 2024-09-30 19:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:712 on 2024-09-30 19:48_

I think it's valuable to use the information from the relevant VCS — so if you say not to use Git we shouldn't fetch information from it or if you say to use Mercurial the author information should come from there.

We could consider adding OS-level author information fetches, but let's do that separately than this.

---

_@zanieb reviewed on 2024-09-30 19:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:612 on 2024-09-30 19:57_

I'm hesitant to introduce a bunch of flags for this. I'll do some thinking out loud. Typically, we use singular names for options that can be repeated so... `--author <name> --author <another name>` makes sense to me. Then... `--no-authors` with a `--no-author` alias to disable detection? Then we're left with "I want the author to be derived from somewhere even if it's an app" which could be like `--force-author` or we can reuse `--author` and add a special key like `auto`. This is sort of nice because we can add more special keys like `git`, `os`, etc. for various author sources? This would let you force read from a source e.g. `uv init --vcs git --author os` which would otherwise be another new flag like `--author-from`? I think collisions are pretty unlikely but it does make the flag less self-documenting.

I shall seek another opinion here. cc @BurntSushi 



---

_@BurntSushi reviewed on 2024-10-01 12:46_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/init.rs`:612 on 2024-10-01 12:46_

I guess my initial reaction here is that it's not worth changing this flag's behavior based on library versus application. It's not immediately obvious to me why we would make that distinction by default. If folks don't want author information, then instead of deleting it after-the-fact, they can do `--no-authors`.

Otherwise, how far do we want to go with `uv init` being an interface for writing metadata values to `pyproject.toml`? If we just want it to be something that tries to infer things with reasonable defaults, then we might not need to expose functionality like `--author BurntSushi`.

Failing all of that, I like your approach @zanieb. I might use `infer-git` instead of just `git` for example. Or something like that.

---

_@zanieb reviewed on 2024-10-01 12:52_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:612 on 2024-10-01 12:52_

> I guess my initial reaction here is that it's not worth changing this flag's behavior based on library versus application. It's not immediately obvious to me why we would make that distinction by default

I guess I like sensible defaults. I don't think the default has much bearing on the flag name though, we still need both toggles for consistency with the rest of our CLI.

Right now we don't really support passing metadata directly, but I think we generally should in the future so I want to design in a way that we won't regret.

---

_@edmorley reviewed on 2024-10-01 13:16_

---

_Review comment by @edmorley on `crates/uv/src/commands/project/init.rs`:612 on 2024-10-01 13:16_

> I guess my initial reaction here is that it's not worth changing this flag's behavior based on library versus application. It's not immediately obvious to me why we would make that distinction by default. If folks don't want author information, then instead of deleting it after-the-fact, they can do `--no-authors`.

For me, the reason why I wouldn't want authors (or any other optional fields related to packaging) defined in a `pyproject.toml` for an app project type, is that if the package doesn't have a build backend and isn't being published to PyPI, then it's extra clutter/something to understand/keep up to date that isn't adding any value. (So really this is more about build backend/publishing vs not, rather than app vs library.)

Users could of course delete those fields afterwards, but they'd have to know that they were optional or else be willing to try deleting them and seeing if something breaks (and hoping that the tool's schema validation is solid enough that it gives me a suitable upfront error message rather than something silently breaking later).

Would a compromise that could keep `uv init` simpler (and avoid the need to support multiple VCSes and OS username lookup etc) be to instead not populate authors at all in `uv init` and instead have `uv publish` handle making the package "publish ready" wrt best practices etc (eg prompting if metadata not filled out).

---

_Comment by @j178 on 2024-10-03 08:50_

I'll go with `--author-from auto|git|none`:

```rust
#[derive(Debug, Default, Copy, Clone, clap::ValueEnum)]
pub enum AuthorFrom {
    /// Fetch the author information from some sources (e.g., Git) automatically.
    #[default]
    Auto,
    /// Fetch the author information for Git configuration only.
    Git,
    /// Do not infer the author information.
    None,
}
```


---

_Review requested from @zanieb by @j178 on 2024-10-03 09:01_

---

_Assigned to @zanieb by @zanieb on 2024-10-03 16:43_

---

_@zanieb approved on 2024-10-08 19:06_

Thank you!

---

_Merged by @zanieb on 2024-10-08 19:06_

---

_Closed by @zanieb on 2024-10-08 19:06_

---

_Comment by @dpprdan on 2024-10-10 15:23_

I am trying to get the `authors` field written by `uv init`, but without success so far. What am I doing wrong?

```sh
dan@DP:~/projects/test-uv$ uv version
uv 0.4.20
dan@DP:~/projects/test-uv$ git config --get user.name
Daniel Possenriede
dan@DP:~/projects/test-uv$ ls -a
.  ..
dan@DP:~/projects/test-uv$ uv init --author-from auto
Initialized project `test-uv`
dan@DP:~/projects/test-uv$ cat pyproject.toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
dan@DP:~/projects/test-uv$ rm -r {*,.*}
dan@DP:~/projects/test-uv$ uv init --lib
Initialized project `test-uv`
dan@DP:~/projects/test-uv$ cat pyproject.toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Comment by @j178 on 2024-10-10 15:57_

@dpprdan Can you show the output of `uv init -v --lib` and `git --version`?

---

_Comment by @dpprdan on 2024-10-10 16:18_

@j178 Sure

```sh
dan@DP:~/projects/test-uv$ uv init -v --lib
DEBUG uv 0.4.20
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/home/dan/.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
WARN Failed to get author from git: No author information found
DEBUG Writing Python versions to `/home/dan/projects/test-uv/.python-version`
Initialized project `test-uv`
dan@DP:~/projects/test-uv$ git --version
git version 2.43.0

dan@DP:~/projects/test-uv$ git config --global user.name
Daniel Possenriede
dan@DP:~/projects/test-uv$ git config --global user.email
me@example.com
```

---

_Comment by @dpprdan on 2024-10-10 16:25_

FWIW this is on Ubuntu 24.04 on WSL.

---

_Comment by @bluss on 2024-10-10 16:36_

Seems like the git config --get/git config get command is not a stable interface - or `--get` is the variant that has been there longer.

Compare

- https://git-scm.com/docs/git-config/2.47.0
- https://git-scm.com/docs/git-config/2.32.3

---

_Comment by @dpprdan on 2024-10-10 16:54_

I think that's it. `git config get` was apparently introduced in [v2.46.0](https://git-scm.com/docs/git-config/2.46.0) (vs [v.2.45.0](https://git-scm.com/docs/git-config/2.45.0)).

With v2.43.0 I see

```sh 
dan@DP:~/projects/test-uv$ git config get user.name
fatal: not in a git directory    # well of course, it is not yet initialised.
dan@DP:~/projects/test-uv$ git config --get user.name
Daniel Possenriede
```

So better use `git config --get` for backwards compatibility? 

---

_Comment by @zanieb on 2024-10-10 17:10_

On it!

---

_Branch deleted on 2024-10-13 09:14_

---
