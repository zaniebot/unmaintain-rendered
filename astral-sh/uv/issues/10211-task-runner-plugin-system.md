```yaml
number: 10211
title: Task runner plugin system
type: issue
state: closed
author: rmorshea
labels: []
assignees: []
created_at: 2024-12-28T05:52:21Z
updated_at: 2025-01-02T18:04:55Z
url: https://github.com/astral-sh/uv/issues/10211
synced_at: 2026-01-12T16:00:08Z
```

# Task runner plugin system

---

_@rmorshea_

In my projects that use `uv` I've developed a pattern (which I've encoded in a [template repo](https://github.com/rmorshea/python-copier-template?tab=readme-ov-file#whats-in-the-box)) where I define a script called `dev.py` that uses `click` to define a CLI and automate common tasks (e.g. running tests, building docs and linting). With that in mind it would be nice if it were possible to define a "default script" so that, instead of typing `uv run dev.py ...`, I could avoid typing `dev.py` each time and use a shorter command a la `yarn run` or `npm run` which inherit commands from the `scripts` section of `package.json`. Since `uv run` wouldn't be suitable for this due to the fact that positional arguments would be confused with executables, an alternative command like `uv do ...` would be necessary. A hypothetical configuration could look something like:

```toml
[tool.uv.do]
command = "uv run dev.py"
```

I understand this might be considered relatively niche, however I could imagine a feature like this negating the need for UV to develop it's own system for defining and executing "user scripts" as other package managers in the Python space have (e.g. [Hatch's](https://hatch.pypa.io/1.12/config/environment/overview/#scripts) and [PDM's](https://pdm-project.org/en/latest/usage/scripts/#user-scripts) approaches). Instead, UV could defer to existing tools in the Python ecosystem for this like [Nox](https://nox.thea.codes/en/stable/) or [Invoke](https://www.pyinvoke.org/).

---

_Comment by @FishAlchemist on 2024-12-28 09:39_

Although the expected format is different, does this issue(https://github.com/astral-sh/uv/issues/5903) function as you expected?

---

_Comment by @zanieb on 2024-12-28 17:28_

Interesting, this sounds like an implementation of #5903 that just invokes a third-party tool? Sort of a plugin system?

---

_Comment by @rmorshea on 2024-12-28 20:36_

@zanieb I think it could function that way. Put another way, I'm proposing that UV provide a command for projects to alias in whatever way they might want.

Taking the initial [taskipy](https://github.com/taskipy/taskipy) example from #5903 you could configure:

```toml
[dependency-groups]
dev = ["taskipy"]

[tool.uv.do]
command = "uv run task"

[tool.taskipy.tasks]
hello = "echo Hello from taskipy!"
```

Any then run:

```
uv do hello
```

I see two main advantages of this approach has over #5903:

- If you choose to use another tool to manage your scripts you don't need to ensure you've copied them both under `[tool.uv.tasks]` and that tool's configuration location of choice. To me, having a bunch of "tasks" which look like `uv run uvx --from=rust-just just <the-task>` seems redundant and annoying to maintain. Instead, under this proposal you could simply configure:

  ```toml
  [tool.uv.do]
  command = "uv run uvx --from=rust-just just"
  ```

- As mentioned above, UV doesn't need to develop and maintain a task running system that will inevitably grow in scope and complexity. While choosing not to provide a task runner out of the box might not conform to people's expectations, I think it could be pretty easily addressed with a short and easily discoverable section of documentation that provides a recommended solution for people to copy-paste into their projects. 

---

_Comment by @rmorshea on 2024-12-28 20:55_

One downside of this proposal vs #5903 is that new contributors to a project that uses UV won't always be able to look for the same `tool.uv.tasks` configuration to find the set of common commands they might want to run.

---

_Renamed from "Consider option to define a "default script"" to "Task runner plugin system" by @rmorshea on 2025-01-02 00:10_

---

_Comment by @rmorshea on 2025-01-02 17:56_

@zanieb would you recommend closing this issue in favor of #5903?

---

_Closed by @zanieb on 2025-01-02 18:04_

---

_Closed by @zanieb on 2025-01-02 18:04_

---
