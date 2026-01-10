```yaml
number: 9167
title: "Modules override `[project.scripts]` for `uv run`"
type: issue
state: closed
author: JimDabell
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-16T17:01:44Z
updated_at: 2025-02-12T18:08:58Z
url: https://github.com/astral-sh/uv/issues/9167
synced_at: 2026-01-10T03:50:30Z
```

# Modules override `[project.scripts]` for `uv run`

---

_Issue opened by @JimDabell on 2024-11-16 17:01_

If my project has a module called `foo` and a section in `pyproject.toml` with:

```toml
[project.scripts]
foo = "foo.cli:app"
```

…then it’s not possible to call the script with `uv run foo`:

```python
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/app/foo/__main__.py", line 3, in <module>
    from . import cli
ImportError: attempted relative import with no known parent package
```

However if I rename the script from `foo` to `bar`, it works:

```toml
[project.scripts]
bar = "foo.cli:app"
```

If I try to run the module with `uv run --module foo`, this also works.

Shouldn’t the script definition take precedence if there’s a `--module` flag to select the module instead?

This is with uv 0.5.2.


---

_Comment by @raqbit on 2024-12-03 10:56_

I've created a reproduction repo here: https://github.com/raqbit/uv-scripts-repro/

The `run_all.sh` script attempts to run the project in three ways:
- Using `python -m foo`
- Using `./.venv/bin/foo`
- Using `uv run foo`

Only the last case fails, probably because it's running the `__main__` file (and not as a module), and not the script installed in `./.venv/bin`.

Tested with `uv 0.5.5`

---

_Comment by @zanieb on 2024-12-03 15:21_

Thanks for the reproduction!

The problem is https://github.com/astral-sh/uv/blob/02c105c3cbc53e291632dc43dbef8777410656c8/crates/uv/src/commands/project/run.rs#L1414-L1415

We shouldn't do that logic if the given command exists in the `bin` directory.

---

_Label `bug` added by @zanieb on 2024-12-03 15:21_

---

_Comment by @zanieb on 2024-12-03 15:35_

This is sort of a pain because we resolve the `RunCommand` _before_ we've discovered the Python environment. We'll need to move some things around.

---

_Label `help wanted` added by @zanieb on 2024-12-03 15:35_

---

_Comment by @2022tgoel on 2024-12-07 17:13_

Hi, 
I was thinking of working on this. 
Do you think a reasonable solution would be to instead of returning a `PythonPackage`, return an `Undetermined` type, and then in `as_command()`, for undetermined types, check if the command is in `bin` before checking if it has a `__main__.py` ? (and then lastly, try executing as external)

---

_Comment by @zanieb on 2024-12-07 18:25_

So.. it looks like we call `RunCommand::from_args` early so that we can read (and apply) settings from PEP 723 scripts. We need to preserve that, so it's mostly a matter of refactoring the types in a nice way to represent the deferred resolution of some commands.

I'd maybe introduce a new type that we resolve earlier then cast to `RunCommand` in the `run` implementation once we have information about the available entry points, e.g. (with some made up names):

```
enum RunTarget {
    Resolved(ResolvedRunTarget)
    Unresolved(UnresolvedRunTarget)
} 

enum ResolvedRunTarget {
   ... enumerate the script kinds?
}

struct UnresolvedRunTarget {
    ... preserve the arguments needed to resolve `RunCommand`?
} 

impl From<RunTarget> for RunCommand { ... }
```


I think this roughly matches your suggestion with `Undetermined`, just with more types? This is the sort of thing that I'd have to play with concretely to find the right pattern for though.
        

---

_Comment by @2022tgoel on 2024-12-08 16:02_

Hi, 
I liked your idea, so I implemented something along those lines in https://github.com/astral-sh/uv/pull/9722. Could you take a look at this? Thanks.

---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Closed by @zanieb on 2025-02-12 18:08_

---
