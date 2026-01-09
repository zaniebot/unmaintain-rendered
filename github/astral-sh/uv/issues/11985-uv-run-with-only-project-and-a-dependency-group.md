---
number: 11985
title: "`uv run` with only project and a dependency group"
type: issue
state: closed
author: nbaju1
labels:
  - question
assignees: []
created_at: 2025-03-05T17:51:33Z
updated_at: 2025-03-05T18:38:55Z
url: https://github.com/astral-sh/uv/issues/11985
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv run` with only project and a dependency group

---

_Issue opened by @nbaju1 on 2025-03-05 17:51_

### Question

I have a project with project dependencies and dependency groups. Is it possible to use `uv run` only installing the project it self (not its dependencies) and the dependencies from a dependency group? `uv run --only-group some-deps` will not install the project, as far as I know.

I want to be able to run a script with a subset of the main dependencies. Most other parts of the project requires the project dependencies, so really don't want to inverse the configuration and end up having to run 95% of the scripts with a `--with` flag.

### Platform

Not relevant

### Version

0.6.3

---

_Label `question` added by @nbaju1 on 2025-03-05 17:51_

---

_Comment by @zanieb on 2025-03-05 18:05_

This isn't supported. It sort of sounds like this is a use-case for https://peps.python.org/pep-0771/ where your other dependencies should be declared as optional (but still installed by default).

---

_Comment by @zanieb on 2025-03-05 18:06_

We do support `uv sync --no-install-project` and could support `uv sync --only-install-project`, but I'm a little hesitant.

---

_Comment by @nbaju1 on 2025-03-05 18:11_

Poetry's `--only` flag has this behavior, i.e. installs a dependency group and the project it self. Has been very useful for us, so would appreciate if you would consider implementing such a feature in uv as well.

---

_Comment by @charliermarsh on 2025-03-05 18:12_

Only the project and not its dependencies is a bit strange. Doesn't the project need its own dependencies...?

---

_Comment by @nbaju1 on 2025-03-05 18:16_

This particular project is various scripts we run in a CI pipeline. A couple of the scripts only require a subset of the dependencies. Since some of the main dependencies are quite large, it has been quite beneficial for us to be able to only install this subset along side the project. I guess the massively reduced installation time with uv negates much of the need for this for us though, but still find it generally useful.

---

_Comment by @nbaju1 on 2025-03-05 18:25_

> We do support `uv sync --no-install-project` and could support `uv sync --only-install-project`, but I'm a little hesitant.

Not that Poetry necessarily is a source of truth for this, but they have a `--only-root` flag for this behavior.

---

_Comment by @zanieb on 2025-03-05 18:26_

I would recommend using `dependency-groups` and `default-groups` instead of `project.dependencies` Then `--only-group` will do what you want. You can have a dependency on the project in dependency groups (just use the name).

---

_Comment by @nbaju1 on 2025-03-05 18:38_

Will have to check if that has any unwanted effects on our dependency scanning tools, but thanks for the suggestion!

---

_Closed by @nbaju1 on 2025-03-05 18:38_

---
