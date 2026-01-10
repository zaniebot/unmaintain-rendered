---
number: 12681
title: Altering editable sources when building a docker image
type: issue
state: open
author: gibsondan
labels:
  - question
assignees: []
created_at: 2025-04-04T20:32:13Z
updated_at: 2025-04-28T15:31:57Z
url: https://github.com/astral-sh/uv/issues/12681
synced_at: 2026-01-10T01:25:23Z
---

# Altering editable sources when building a docker image

---

_Issue opened by @gibsondan on 2025-04-04 20:32_

### Question

I'm trying to build a Docker image using uv that incorporates other packages from elsewhere in a monorepo, installing them editably. These packages are not part of the Docker context where the build command is being run.

The way that I would have done this before with pip is pretty gross, but worked:
- Copy in all the packages from elsewhere into the repo into the folder with the Dockerfile
- pip install them all together editably

I'm struggling to map that pattern onto the Dockerfile in the uv docker example: https://github.com/astral-sh/uv-docker-example which uses uv sync and relies on tool.uv.sources to locate the editable packages.

Specifically I have a pyproject.toml that looks like this:

```
[tool.uv.sources]
dagster-test = { path = '/Users/dgibson/dagster/python_modules/dagster-test', editable = true }
dagster-graphql = { path = '/Users/dgibson/dagster/python_modules/dagster-graphql', editable = true }
```

referencing paths outside of the Docker context.

The only option I can think of is modifying the contents of the pyproject.toml during the build to reference the new editable location after I copy it into the final folder with the Docker context.

Curious if this has come up before or if you have any other suggestions on how we might approach this. I could imagine heavy solutions like having different 'profiles' available for tool.uv.sources and being able to switch between them, but that seems like a lot.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @gibsondan on 2025-04-04 20:32_

---

_Comment by @gibsondan on 2025-04-04 20:33_

Altering the Dockerfile is also entirely fair game here, but ideally it would still be using uv sync under the hood. I wouldn't be opposed to manually passing in a long list of editable overrides to the tool.uv.sources values into the uv sync command, for example. 

---

_Comment by @zanieb on 2025-04-04 23:47_

Can you use relative paths instead of absolute paths?

Perhaps you could use a symlinked directory to the absolute paths outside the container then replace the symlink with a real directory with the packages copied in when inside the container?

Do you still need them to be editable?

---

_Comment by @heathkh-recursion on 2025-04-18 18:29_

I'm also struggling with this.  I was able to use relative paths for tools.uv.sources... and set the docker build context to include them. But I get back to a situation where a code change in the editable packages invalidates the uv sync layer meant to just install remote packages.   Seems we need a way to have uv sync filter down to just install the remote deps in one layer, the local / editable packages in a subsequent layer.  

---

_Comment by @charliermarsh on 2025-04-18 18:31_

You mean something like `--no-install-workspace`, but for `path` sources?

---

_Comment by @zanieb on 2025-04-18 20:48_

If what Charlie suggests, that'd be like https://github.com/astral-sh/uv/issues/12845, e.g., `--no-install-source path`?

---

_Referenced in [astral-sh/uv#9258](../../astral-sh/uv/issues/9258.md) on 2025-04-23 17:14_

---

_Referenced in [astral-sh/uv#13073](../../astral-sh/uv/issues/13073.md) on 2025-04-23 19:27_

---

_Comment by @JuR-0 on 2025-04-28 08:44_

Just to be sure I understood what you all said above. As of right now, the only way to build a dockerfile for a project referencing local packages (that are not part of the same workspace), would be to include all of these packages in the docker context when running export --frozen? 

---

_Comment by @charliermarsh on 2025-04-28 13:10_

No, if you're using `--frozen`, you don't need to include anything other than the root `pyproject.toml` and `uv.lock`.

---

_Comment by @JuR-0 on 2025-04-28 15:31_

> No, if you're using `--frozen`, you don't need to include anything other than the root `pyproject.toml` and `uv.lock`.

I was getting an error of distribution not found for the local dependency. 
I solved it by copying the pyproject.toml, lock of all the local dependencies with the same folder structure. Not sure if it's the best though?

---
