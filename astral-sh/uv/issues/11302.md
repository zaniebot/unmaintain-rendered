```yaml
number: 11302
title: Start search for projects and virtual environments at script directory (instead of cwd)
type: issue
state: open
author: jdumas
labels:
  - breaking
assignees: []
created_at: 2025-02-07T00:19:32Z
updated_at: 2025-10-12T21:16:50Z
url: https://github.com/astral-sh/uv/issues/11302
synced_at: 2026-01-10T03:23:53Z
```

# Start search for projects and virtual environments at script directory (instead of cwd)

---

_Issue opened by @jdumas on 2025-02-07 00:19_

### Question

Consider a project with the following folder structure:

```
.
├── foo
│   ├── hello.py
│   └── pyproject.toml
└── pyproject.toml
```

**pyproject.toml**
```toml
[project]
name = "uv-nested"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[tool.uv.workspace]
members = ["foo"]
```

**foo/pyproject.toml**
```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = ["rich"]
```

**foo/hello.py**
```python
from rich import print


def main():
    print("Hello from foo!")


if __name__ == "__main__":
    main()
```

1. Now try to run `foo/hello.py`:
   ```
   ❯ uv run foo/hello.py
   Using CPython 3.11.11
   Creating virtual environment at: .venv
   Traceback (most recent call last):
     File "/Users/jedumas/sandbox/uv_nested/foo/hello.py", line 1, in <module>
       from rich import print
   ModuleNotFoundError: No module named 'rich'
   ```

2. If you cd in the subfolder, it runs correctly:
   ```
   ❯ pushd foo; uv run foo/hello.py; popd
   Hello from foo!
   ```

3. And now that we have a `foo/.venv`, calling `uv run` from the root folder will work!
   ```
   ❯ uv run foo/hello.py
   Hello from foo!
   ```

So my question is: is this expected behavior? I would expect that when calling `uv run` on a script, `uv` would use the most nested `pyproject.toml` to setup the venv and execute a given script.

### Platform

macOS 14.5

### Version

uv 5.29.0

---

_Label `question` added by @jdumas on 2025-02-07 00:19_

---

_Comment by @zanieb on 2025-02-07 00:38_

So I can reproduce this

```
❯ uv init example --bare
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv init foo
Adding `foo` as member of workspace `/Users/zb/workspace/uv/example`
Initialized project `foo` at `/Users/zb/workspace/uv/example/foo`
❯ uv add --package foo rich
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 6 packages in 187ms
Prepared 4 packages in 197ms
Installed 4 packages in 6ms
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + pygments==2.19.1
 + rich==13.9.4
❯ echo "import rich" > foo/hello.py

# Remove the implicit sync of `foo` from `uv add`
❯ rm -rf .venv
❯ uv run foo/hello.py
Using CPython 3.13.0
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "/Users/zb/workspace/uv/example/foo/hello.py", line 1, in <module>
    import rich
ModuleNotFoundError: No module named 'rich'
```

What you're missing is a dependency on `foo` from your workspace root project

```
❯ uv add ./foo
Resolved 6 packages in 2ms
Installed 4 packages in 10ms
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + pygments==2.19.1
 + rich==13.9.4
❯ uv run foo/hello.py
❯ cat pyproject.toml
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "foo",
]

[tool.uv.workspace]
members = ["foo"]

[tool.uv.sources]
foo = { workspace = true }
```

If you want `foo` to be available by default. Otherwise, you'd need to target that workspace member, e.g.:

```
❯ uv run --package foo foo/hello.py
```


---

_Comment by @jdumas on 2025-02-07 01:01_

Ok but my root project does not depend on `foo`. In fact the two are pretty much independent. I would argue that pyproject lookup should be done from the bottom-up, so if the script I want to run is in `foo/`, then `uv run` should pick `foo/pyproject.toml` rather than `./pyproject.toml` to setup the .venv I need.

Also, why would the second `uv run foo/hello.py` work? It seems uv is picking up the `foo/.venv` environment to run `foo/hello.py`, despite being run from the root folder.

---

_Comment by @jdumas on 2025-03-06 23:58_

Hi. I'd like to bump this topic again. I still do think it's a bug in uv, or at least a behavior that is worth changing.

To be a bit more concrete, in my root repository I have a `pyproject.toml` that builds Python bindings for a C++ project. This means long build time on a fresh clone, etc. Then I have a subfolder called `scripts/` with a bunch of Python files used for CI/CD (e.g. apply formatting/linting on my C++ files, update licensing headers, etc.). I would like to maintain a single `scripts/pyproject.toml` file for all my CI/CD files, without having to build the C++ project itself (which is time consuming).

For this reason, I would like `uv run scripts/foo.py` to pick-up the most nested `pyproject.toml` to setup and run the venv for my script files. I am currently using inline metadata in my various scripts. But since I have a lot of entry points, it creates a lot of redundant information.

Finally, I think that `uv run foo/hello.py` picking the `foo/.venv` and not the root `.venv` (on the 2nd try), but creating a `.venv` in the root folder (on the 1st try) still doesn't make sense to me.

---

_Comment by @andersk on 2025-03-23 01:16_

- Related: #12193

---

_Label `breaking` added by @zanieb on 2025-04-01 17:39_

---

_Renamed from "Pyproject and venv lookup for nested packages." to "Start search for projects and virtual environments at script directory (instead of cwd)" by @zanieb on 2025-04-01 17:39_

---

_Label `question` removed by @zanieb on 2025-04-01 17:39_

---

_Comment by @zanieb on 2025-04-01 17:42_

We can consider this. I think it's probably reasonable, but it is breaking and it will be hard to assess how many people rely on the existing behavior.

---

_Comment by @karnigen on 2025-04-01 20:46_

It is certainly important to have the ability to run scripts with `.venv` in the current working directory (cwd), at least for testing different versions and libraries, and this is the default behavior. From a user's perspective, it's also essential to have the option to use `.venv` from the directory where the script is installed, which is addressed by the `--project` flag. Both scenarios seem to be resolved, or am I missing something?

The only thing that could be improved is that the` --project <path>` flag is a bit cumbersome. It would be sufficient to introduce `--script-project` without specifying `<path>`, and it would automatically find where the script is located and use the appropriate `.venv`.

Searching for `.venv` directories from the bottom up is already done by `uv` now, or at least it's working correctly.

---

_Comment by @jdumas on 2025-04-01 22:18_

> Searching for .venv directories from the bottom up is already done by uv now, or at least it's working correctly.

That's not what I observe. In the example I shared above, `uv run foo/hello.py` is creating a `.venv` from the root `pyproject.toml`, not the `foo/pyproject.toml`.

EDIT: Ok so to be more clear, if the nested `foo/.venv` exists, then `uv run foo/hello.py` will work indeed. So it is surprising that when starting fresh, `uv run foo/hello.py` will first attempt to create the outermost `./.venv` instead of the nested `foo/.venv` environment. That seems inconsistent behavior.

---

_Comment by @karnigen on 2025-04-02 06:37_

Just to clarify what we want:

1. When there is a `.venv` in the current directory, we always want `uv run` to use the current one.
2. If `--project <path>` is used, we want it to look for `.venv` in `<path>` or directories upwards.

If I use `uv run <path>/script.py` and there is no `.venv` in the current directory (case 1), it automatically uses case 2, which `uv` doesn't normally do; the script cannot be run, which is probably correct.

If I use a workspace (your case), it seems the situation changes again. However, the question now is what has higher priority: to preserve steps 1 and 2 regardless of the workspace, or to create different rules for the workspace?

---

_Comment by @jdumas on 2025-04-02 14:38_

Actually it doesn't matter whether I have the `tool.uv.workspace` defined or not. If `foo/.venv` exists, then `uv run foo/hello.py` will use it.

What I'm trying to argue is that (1) the result of running `uv run <something>.py` should _not_ depend on the current working directory, and (2) search order should be from local to global, i.e. inline metadata > local folder > any parent folder.

Now, if we have inline metadata, it's pretty clear we should always use the associated tmp .venv to run the code. So why not have a similar non-cwd dependent behavior to find the closest pyproject.toml and associated .venv? There are two possible situations:
1. We are trying to call `uv run ../<something>.py`, where `<something>.py` is _outside_ of the cwd and its descendants. In this case this file is clearly not part of the current dir's `pyproject.toml`. Search for a matching `pyproject.toml` can proceed to parent folders. If no ascendant pyproject.toml is found then we should return an error (even if the cwd has a pyproject.toml).
2. We are trying to call `uv run foo/<something>.py` where both the cwd and `foo/` have their own `pyproject.toml`. In this case I'd expect `<something>.py` to be able to run in the immediate `foo/.venv` (otherwise why put it in `foo/` in the first place?). But should we expect `<something>.py` to run differently when executed in the context of the cwd `pyproject.toml`? To me it's not clear that it's ever the intended usage.

With the local-to-global search for `pyproject.toml`, I'm not sure we'd even need a `--project <>` flag to be honest. Ok I can see some use case where you'd want to be able to test a script in multiple environments where the `--project <>` flag would be needed.


In any case I agree it would be a breaking change, but since uv hasn't reached 1.0 yet I think it's worth discussing whether this makes sense as the default behavior.

---

_Comment by @karnigen on 2025-04-02 15:18_

It seems an old version of `uv` was used; it behaves differently now (version 0.6.11)
Same structure as above, and `uv run foo/hello.py`:

```
uv run foo/hello.py
Using CPython 3.13.2
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "foo/hello.py", line 1, in <module>
    from rich import print
ModuleNotFoundError: No module named 'rich'
```

```
uv self update 
info: Checking for updates...
success: You're on the latest version of uv (v0.6.11)
```

I would, of course, also welcome it if `uv` would by default search for `.venv` in the directory where the script is located, and if someone needed a different directory, they could use a flag. It also annoys me.
I personally use a script that reverses `uv`'s behavior, always defaulting to searching from the script's directory. But of course, it would be better if `uv` did this itself.

---

_Comment by @jdumas on 2025-04-02 15:27_

> It seems an old version of uv was used; it behaves differently now (version 0.6.11)
> Same structure as above, and uv run foo/hello.py:

If you run `uv run hello.py` in the `foo/` folder to create `foo/.venv`, and then later run `uv run foo/hello.py` from the parent directory, it will use `foo/.venv` to execute `hello.py`. (That's with v0.6.11.). I think that behavior should be considered a bug.

---

_Comment by @karnigen on 2025-04-03 22:39_

`uv` version 0.6.12 appears to be exhibiting different behavior. As a temporary solution, I've implemented a script that always resolves the script path and uses the corresponding virtual environment. The link is provided for those who may find it useful: [uvr](https://github.com/karnigen/uvr). Hopefully, it meets the expectations outlined here.

---

_Comment by @andersk on 2025-06-24 21:50_

This issue (specifically, its consequence #12193) is blocking Zulip from shedding its reliance on the system Python version and switching to uv’s Python binaries.

uvr has broken argument parsing that the author won’t fix (karnigen/uvr#4), plus it’s an obscure third-party tool that would need to be installed separately before any scripts can run, so it’s not a satisfactory solution.

---

_Added to milestone `v0.9.0` by @zanieb on 2025-08-07 13:21_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:16_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:16_

---
