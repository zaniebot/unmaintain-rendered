```yaml
number: 7165
title: "Opt-into / opt-out of automatic re-sync with `uv run`?"
type: issue
state: closed
author: pawamoy
labels:
  - needs-decision
assignees: []
created_at: 2024-09-07T11:42:27Z
updated_at: 2024-09-10T21:29:44Z
url: https://github.com/astral-sh/uv/issues/7165
synced_at: 2026-01-12T15:59:11Z
```

# Opt-into / opt-out of automatic re-sync with `uv run`?

---

_@pawamoy_

uv 0.4.5.

Hi, thanks for uv, it's hot! :volcano: 

I recently started using the higher-level commands of uv, like `uv sync` instead of `uv pip ...`. Working great :rocket: `uv sync` makes things much, much faster within my workflow :smile: 
 
I also started using `uv run`. And there's something about it that I both love and dislike: the fact that it automatically re-syncs the env each time I use it.

- I love it because it means I can clone my repo, enter it, and immediately run my `format` task (which uses Ruff :yum:) to format the code, and uv will automatically create the venv, lock and install the dependencies, and run the specified command. That is *awesome*.
- I dislike it because I often tinker with my venvs, installing different versions of dependencies to try things out. But now each time I use `uv run`, it resets these dependencies to the versions saved in the lock file.

I am aware of the `--with` option (which is *awesome* too!), but I would really prefer "persisting" my changes in the venv, so that I don't have to use `--with x==y` in each subsequent commands. So, ideally I would like to have a way to opt-out of this automatic re-sync, which would result in the following behavior:

1. if there's no venv, create one, lock, install deps
2. if there's a venv, don't touch it

(venv path is determined with `UV_PROJECT_ENVIRONMENT` and usual fallback(s))

Do you think you can consider such a thing :thinking:?

Happy to elaborate on my use-case, as maybe there's a different/better workflow that uv suggests.

---

_Comment by @pawamoy on 2024-09-07 11:52_

Ooooh it seems `--no-project` works for point 2 (if there's a venv, don't touch it), but not point 1 (if there's no venv, create it, lock, install). Could be good enough to me, as I don't mind continuing to tell my users to run `make setup` first to install deps.

---

_Comment by @charliermarsh on 2024-09-07 12:02_

I think it's gonna be hard for us to achieve both (1) and (2). We're trying to avoid reading state from disk like that. We _could_, it's just more of a departure from how things work today. I think `--no-project` will work if you've already activated the environment... Otherwise, I think you want something like `uv run --no-sync` (which doesn't exist IIRC).


---

_Label `needs-decision` added by @charliermarsh on 2024-09-07 12:02_

---

_Comment by @charliermarsh on 2024-09-07 12:02_

\cc @zanieb who may have opinions on point (1).

---

_Comment by @zanieb on 2024-09-07 16:04_

I think we're getting consistent feedback that `uv run --no-sync` should exist. 

But... I don't think an operating mode that touches the virtual environment if it doesn't exist but not otherwise makes a ton of sense for general use — it seems hard to teach. I guess this would be a variant like `uv run --only-initial-sync`(?). 

> I would really prefer "persisting" my changes in the venv, so that I don't have to use --with x==y in each subsequent commands.

I think that we can do better here. We don't want you to have to persist the changes to your environment nor should you have to specify `--with` on every invocation. The options are probably

1. Use `--with-requirements` and write them to a file
2. Add support for running commands with extra dependencies in #5903

Would (2) address your use case?


---

_Comment by @charliermarsh on 2024-09-07 17:29_

@zanieb -- Does `--no-sync` also not lock? (Should that just be the `--frozen` behavior perhaps?)

---

_Comment by @zanieb on 2024-09-07 17:43_

I would be surprised if it locked. I was also surprised `--frozen` syncs —  but it makes sense that we allow for syncing without updating the lock. 

Looking for consistency with other things... the `uv add` documention says:

> The lockfile and project environment will be updated to reflect the added dependencies. To skip updating the lockfile, use `--frozen`. To skip updating the environment, use `--no-sync`.

Which seems reasonable. For `uv run` though I feel like `--no-sync` could imply `--frozen` since the intent of the command is not to modify the project's dependencies as opposed to, e.g., `uv add`.



---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-08 16:34_

---

_Closed by @charliermarsh on 2024-09-10 21:29_

---

_Closed by @charliermarsh on 2024-09-10 21:29_

---
