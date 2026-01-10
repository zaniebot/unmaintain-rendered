---
number: 3836
title: "Virtual environment support for `uv run`"
type: issue
state: closed
author: Vigilans
labels:
  - preview
assignees: []
created_at: 2024-05-25T15:02:04Z
updated_at: 2024-06-26T21:33:59Z
url: https://github.com/astral-sh/uv/issues/3836
synced_at: 2026-01-10T01:23:31Z
---

# Virtual environment support for `uv run`

---

_Issue opened by @Vigilans on 2024-05-25 15:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

As mentioned in https://github.com/astral-sh/uv/issues/3097, `uv run` may support working with virtual environment, where:
* `uv run` can specify a venv folder to use that venv for running command
* `uv run` can work without depending on a python project under certain condition (e.g. when a venv folder is specified)

---

_Label `preview` added by @zanieb on 2024-05-25 15:09_

---

_Comment by @charliermarsh on 2024-05-26 12:05_

Thanks!

---

_Comment by @charliermarsh on 2024-06-26 12:55_

What about something like this:

- If we discover a workspace, we use the current behavior.
- If we can't discover a workspace, we use our standard Python interpreter discovery, and run in that interpreter. (So, e.g., run in the current venv, or a discovered venv, or whatever you pass in `--python`.)
- We add an option to mark a `pyproject.toml` as "not managed by uv" / "not part of a workspace" (like `managed = false` or `workspace = false`) so that you can use this in codebases that have a `pyproject.toml` but want to use the `pip` API.


---

_Comment by @Vigilans on 2024-06-26 13:59_

The first two rules are fairly reasonable! For the third rule, is https://github.com/astral-sh/uv/issues/3404 and https://github.com/astral-sh/uv/pull/3585 relevant regarding `workspace = false` option?

Though I haven't figured out how a `pyproject.toml` will impact `pip` API (the `uv pip` commands?) yet, in `uv run` it will cause `uv` to install current project as package, and disabling it may make not much difference once editable build is installed? (btw, I found in my project `uv run ls` always try to build and reinstall my project, is that intended?)

---

_Comment by @charliermarsh on 2024-06-26 14:06_

I am wondering if `workspace = false` and `managed = false` mean different things... You might have a project that's nested in a workspace, but shouldn't be considered _part of_ the workspace, but should _still_ be managed with uv.

> btw, I found in my project uv run ls always try to build and reinstall my project, is that intended?

(Do you have dynamic metadata in your project? If so, yes, but we may change that behavior.)

---

_Comment by @Vigilans on 2024-06-26 14:19_

> (Do you have dynamic metadata in your project? If so, yes, but we may change that behavior.)

Yes, we have `requirements.txt` (generated with `uv pip freeze | uv pip compile -`) used for `pyroject.toml` using
```
[tool.setuptools.dynamic]
dependencies = {file = ["requirements.txt"]}
```



---

_Comment by @charliermarsh on 2024-06-26 14:20_

Yeah, we have to rebuild your project every time if you have dynamic metadata right now, since we have no way of knowing if it's up-to-date.

---

_Referenced in [astral-sh/uv#4549](../../astral-sh/uv/pulls/4549.md) on 2024-06-26 14:26_

---

_Comment by @zanieb on 2024-06-26 14:45_

> We add an option to mark a pyproject.toml as "not managed by uv" / "not part of a workspace" (like managed = false or workspace = false) so that you can use this in codebases that have a pyproject.toml but want to use the pip AP

Would this be equivalent to `uv run --isolated`?

---

_Comment by @charliermarsh on 2024-06-26 14:46_

Nope! `uv run --isolated` runs a command in an isolated, ephemeral environment.

---

_Comment by @zanieb on 2024-06-26 14:50_

Ah hm, should `--isolated` being double duty of ignoring configuration _and_ forcing an ephemeral environment? I think the flag name makes sense for the latter in this command but I worry about expanding the concept / purpose. (I think this is a pretty minor concern)

---

_Comment by @charliermarsh on 2024-06-26 14:53_

We could remove it and add a dedicated flag (`--ephemeral` or whatever), but we still need a solution to running in a venv when a `pyproject.toml` is present.

---

_Comment by @charliermarsh on 2024-06-26 14:54_

> Ah hm, should `--isolated` being double duty of ignoring configuration and forcing an ephemeral environment?

(This is already how it behaves today, just to clarify.)

---

_Comment by @Vigilans on 2024-06-26 14:59_

> we still need a solution to running in a venv when a pyproject.toml is present.

Should any explicit flag of `python interpreter discovery` takes precedence over discovered workspace? That enables user to explicitly specify a venv to use it when in a folder with pyproject.toml (but makes the code in the PR more complicated though).

---

_Comment by @charliermarsh on 2024-06-26 15:01_

Unfortunately `--python` _does_ have a valid meaning and usage in workspaces. If you run `--python 3.8`, and the venv uses a mismatched version, we'll re-create it.

---

_Comment by @zanieb on 2024-06-26 15:03_

> we still need a solution to running in a venv when a pyproject.toml is present.

Perhaps the answer I'm looking for will fall out of that. 

Given the semantics in Ruff, I'd expect `--isolated` to ignore the `pyproject.toml` and other configuration files but not change other other behavior like ignoring existing environments. I designed the `uv run` isolated feature purely to do the latter but yeah it ended up doing both because it's the obvious flag name. I don't think the behavior is unreasonable though. I need to think a bit more about the `pyproject.toml`-not-a-workspace design :)



---

_Comment by @charliermarsh on 2024-06-26 15:04_

Yeah, if `--isolated` respected an existing venv, then we'd probably find ourselves wanting a flag to do what `--isolated` does now.

Let me know your thoughts when you can, this work is blocked on feedback on the ideas I proposed.


---

_Comment by @Vigilans on 2024-06-26 15:12_

> I'd expect --isolated to ignore the pyproject.toml and other configuration files but not change other other behavior like ignoring existing environments.

I think this will be a very useful behavior. For end-users, implicit would help, while for scripts explicitness helps much. 

For my use case, I majorly use `conda run`/`uv run` in automated shell scripts, especially in Dockerfile where activating an environment is not convenient, while explicitly specifying an environment to run is a more working solution.

So, for such scripts, I'd expect an explicit flag (`--isolated` or something) to tell `uv` to consistently run in project mode / venv mode, without taking care of what current directory `uv` is running in. 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-26 21:33_

---

_Comment by @charliermarsh on 2024-06-26 21:33_

We now fallback to virtual environments if you're not in a project.

---

_Closed by @charliermarsh on 2024-06-26 21:33_

---

_Referenced in [astral-sh/uv#4565](../../astral-sh/uv/pulls/4565.md) on 2024-06-27 11:05_

---
