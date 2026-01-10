```yaml
number: 2386
title: Selecting a Python interpreter
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2024-03-12T16:50:58Z
updated_at: 2024-05-21T19:37:25Z
url: https://github.com/astral-sh/uv/issues/2386
synced_at: 2026-01-10T05:31:37Z
```

# Selecting a Python interpreter

---

_Issue opened by @zanieb on 2024-03-12 16:50_

In #2338 some questions came up around Python interpreter selection. Here we'll summarize our expected behavior and have some discussion around it.

Interpreters can be selected with the following formats:

- Direct path e.g. `/bin/python3`
- Version e.g. `3.10`
- Implementation e.g. `pypy`
- Implementation and version e.g. `pypy3.10` or `cpython3.10`

Using `python` as the implementation should allow _any_ implementation. Its behavior should be equivalent to `X.Y`. 

Python interpreters may be discovered via:

- Path directly provided
- The spawning interpreter (i.e. `python -m uv`)
- An activated virtual environment
- A virtual environment in the current directory
- Executable found in `PATH`

When a user does not specify an interpreter, we should do our best to infer one in the order above.

When a user specifies an interpreter via a direct path:

- We should only use the exact provided interpreter
- We do not check the version or implementation, though they must be supported by uv

When a user specifies an interpreter via a version:

- We do not check the interpreter implementation
- We should follow the discovery order, but ignore interpreters that do not satisfy the version
- If we are compiling dependencies, we can fallback to a mismatched version if no matching version is found

When a user specifies an interpreter with an implementation (and version):

- If the implementation is unknown, we should error and recommend a direct path.
- We should follow the discovery order, but ignore interpreters that do not satisfy the implementation (and version)
- If we are compiling dependencies, we can fallback to a matching implementation with a mismatched version if no matching version is found

Interpreters can either belong to a virtual environment or the system.

When an interpreter belongs to the system, we will _not_ modify packages (i.e. install, uninstall) in the interpreter's environment without the `--system` flag or specification of the interpreter with a direct path. We use system interpreters for compiling dependencies without opt-in.

## Examples

Compile using system Python interpreter discovered by being the spawning intepreter:

```
python -m uv pip compile ...  # Ok: System Python used
```

Install using system Python interpreter discovered by being the spawning intepreter:

```
python -m uv pip install ...   # Error: System Python cannot be used without opt-in `--system`
```

Install using system Python interpreter discovered by direct path:

```
uv pip install --python /bin/python ...   # Ok: System can be used with direct path
```

Compile using system Python interpreter discovered by being the spawning intepreter with matching version:

```
python3.10 -m uv pip compile --python 3.10  # Ok: System Python used since version matches
```

Compile using Python interpreter discovered on path since spawning intepreter with mismatched version:

```
python3.10 -m uv pip compile --python 3.12  # Ok: Python 3.12 is discovered on path since version does not match spawning
```

Install using Python interpreter in the working directory:

```
uv venv
uv pip install ...  # Ok: Python is discovered in the working directory
```


Install using Python interpreter in the active virtual environment:

```
uv venv
source .venv/bin/activate
mkdir foo && cd foo
uv pip install ...  # Ok: Python is discovered from `VIRTUAL_ENV`
```

Install using Python interpreter in the active virtual environment with mismatched version:

```
uv venv --python 3.10
source .venv/bin/activate
uv pip install --python 3.12  # Error: Requested Python version does not match version from virtual environment
```


Compile using Python interpreter in the active virtual environment with mismatched version:

```
uv venv --python 3.10
source .venv/bin/activate
uv pip compile --python 3.12  # Ok: Either finds 3.12 on path or uses 3.10 for compilation with a warning
```

Install using Python interpreter via direct path with active virtual environment:

```
uv venv --python 3.10
source .venv/bin/activate
uv pip install --python ~/python/python3.12  # Ok: Uses direct path and ignores virtual environment entirely
```

Install using Python with an unknown implementation:

```
uv pip install --python foobar3.12  # Error: Unknown implementations are only allowed via direct paths
```

Install using Python with an unknown implementation via direct path:

```
uv pip install --python /bin/foobar3.12  # Ok: As long as we support the discovered implementation
```


---

_Comment by @konstin on 2024-03-12 16:56_

Do we support looking up `-p my_not_cpython3.10`, where `my_not_cpython3.10` is in `PATH` but `my_not_cpython` is not a known implementation (currently cpython, pypy and pyston)? If so, do we give the user indication whether a prefix was recognized or not? If not, should we remove the current PATH lookup support?

---

_Comment by @zanieb on 2024-03-12 16:58_

@konstin I think we should explicitly not support that and require a direct path if you want to use an unknown implementation.

(edited the post to reflect this)

---

_Comment by @charliermarsh on 2024-03-12 17:01_

(For clarity: we do support that today, I think. The behavior Zanie proposed above would be a change.)

---

_Assigned to @konstin by @konstin on 2024-03-12 17:24_

---

_Comment by @konstin on 2024-03-12 17:51_

Another question from #2388: With `-p 3.10`, would we pick pypy, or should we only do cpython by default and alternative implementations be an opt-in? Given how incompatible other implementations are, i'm in favor of cpython-by-default and leaving pypy/pyston for explicit opt-in.

---

_Comment by @zanieb on 2024-03-12 17:57_

We've got a couple discussions going about this elsewhere:

- https://github.com/astral-sh/uv/issues/2092#issuecomment-1987649391
- https://github.com/astral-sh/uv/issues/2316#issuecomment-1987088424

I think those make it clear that we should support `pypy` without opt-in and I think the top-level post here reflects that already.

---

_Comment by @konstin on 2024-03-12 18:00_

https://github.com/astral-sh/uv/issues/2316#issuecomment-1987088424: 

> Yes, if no other python is present.
> 
> The way I interpret it, python or the lack of the implementation basically translates to any implementation. However for historical reasons if multiple implementations are available cpython wins.

That would again we very different from what we currently, this requires us to collect all python implementations and rank!

---

_Comment by @zanieb on 2024-03-12 18:07_

I think it's fine to take the first one on the path and not support the historical discovery noted there. We can consider something more complicated if we hear it causes problems?

---

_Comment by @charliermarsh on 2024-03-12 18:45_

When a user provides, e.g., `cpython3.10`, how we will find the relevant executables in `PATH`?

---

_Comment by @zanieb on 2024-03-12 18:51_

I think when an implementation is provided we could

- First, scan for an exact match in the PATH
- If not found, scan for `pythonX.Y` and `python` in PATH and check if the implementation matches

We could also consider behavior specific to the implementation and the binaries we know they ship (this probably makes a lot of sense but I don't know what's common). Since we require you to match a known implementation we could definitely do this.

---

_Unassigned @konstin by @konstin on 2024-03-13 12:21_

---

_Comment by @mkleinbort-ic on 2024-03-14 13:59_

Just to add to this...

I was having issues today where `python -m uv pip install --system --prerelease=allow -r pyproject.toml` was not updating my dependencies as per the `pyproject.toml`

It seems the issue is that  `which  python` 
returns `/c/Users/mkleinbort/AppData/Local/Programs/Python/Python311/python`

but `which python3` returns `/c/Users/mkleinbort/AppData/Local/Microsoft/WindowsApps/python3`

Not hard to fix, but I would have expected the original command to install the dependencies in the python as referenced by `python`

---

_Comment by @haarisr on 2024-04-03 17:10_

Might not be exactly related

But how about writing to a file such as `pyproject.toml` under tool.uv or a uv specific file when the venv is created in the first place. 

If not uv writing to the file, let the user specify it under the toml so that uv can look it up and activate that venv/python on running a python/uv command.

This also might blend well with creating a default venv when there's no venv present in the current directory in #2665  and the path of the venv can be added/appended to the toml file.

---

_Label `enhancement` added by @charliermarsh on 2024-04-16 04:32_

---

_Comment by @charliermarsh on 2024-04-16 15:18_

A brief proposed addendum on top of the above logic, quoting https://github.com/astral-sh/uv/issues/3060. When looking at a `--python` value...

- If the --python value is a path to an existing file, use that as the interpreter.
- If it's a directory, assume it's a venv and use the venv interpreter.
- Otherwise, if it contains a path separator, fail with an error.
- Finally (a filename with no path part) try a search on PATH.


---

_Closed by @zanieb on 2024-05-21 19:37_

---
