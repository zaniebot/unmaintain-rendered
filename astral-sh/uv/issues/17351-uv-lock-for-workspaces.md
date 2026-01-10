```yaml
number: 17351
title: uv.lock for workspaces
type: issue
state: open
author: JuanFMontesinos
labels:
  - question
assignees: []
created_at: 2026-01-07T21:19:22Z
updated_at: 2026-01-08T02:28:55Z
url: https://github.com/astral-sh/uv/issues/17351
synced_at: 2026-01-10T03:11:36Z
```

# uv.lock for workspaces

---

_Issue opened by @JuanFMontesinos on 2026-01-07 21:19_

### Question

Hi,
there is an obvious case which not sure was never considered.

When u have a workspace with several libraries and u call something like `uv sync --all-packages --all-groups`, you obtain a uv.lock file for the overall build. However, you no longer obtain a uv.lock file for each specific library in the workspace. This causes that, in case you would like to reuse a library in the workspace somewhere else, you don't have a lockfile with the versions used and have to rebuild taking the risk of having different versions.

Is this intentional?
I think there would be somewhat a workaround doing `uv sync --package the_package` and then `uv export` but it's not very nice.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @JuanFMontesinos on 2026-01-07 21:19_

---

_Comment by @zanieb on 2026-01-07 22:32_

Yes it's intentional that we create a coherent lockfile for the entire workspace. The subsets selected during `uv sync` have no effect on the lockfile, the lockfile is always for all of the packages. If you need a subset of the lockfile, you can use `uv export --package ...`.

---

_Comment by @JuanFMontesinos on 2026-01-07 22:40_

Thanks for your answer. I understand your point, however, my understanding is that reproducibility is only guaranteed with `uv run --frozen`

So I would find interesting to be able to produce the lock file of just that package as of `uv export --format uv.lock --package ...` 

And then be able to take that package standalone, the resulting uv lock file and execute `uv run --frozen ...`

With the current exporter, uv only allows to use `uv pip install`, which feels like a bad practice to me. 

Does my understanding make sense? 
@zanieb 

---

_Comment by @zanieb on 2026-01-08 02:28_

You can use `uv run --package foo --frozen ...` and we'll just use the relevant subset of the lockfile. You don't need to create a subsetted lockfile.

---

_Comment by @zanieb on 2026-01-08 02:28_

Maybe I'm not quite understanding what you're trying to do?

---
