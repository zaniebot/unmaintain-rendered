```yaml
number: 6834
title: "Allow customizing the project environment path with `UV_PROJECT_ENVIRONMENT`"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
assignees: []
merged: true
base: main
head: zb/venv-path
created_at: 2024-08-29T22:20:59Z
updated_at: 2024-09-03T21:32:52Z
url: https://github.com/astral-sh/uv/pull/6834
synced_at: 2026-01-12T16:07:33Z
```

# Allow customizing the project environment path with `UV_PROJECT_ENVIRONMENT`

---

_@zanieb_

Allows configuration of the (currently hard-coded) path to the virtual environment in projects using the `UV_PROJECT_ENVIRONMENT` environment variable.

If empty, we'll ignore it. If a relative path, it will be resolved relative to the workspace root. If an absolute path, we'll use that.

This feature targets use in Docker images and CI. The variable is intended to be set once in an isolated system and used for all uv operations. 

We do not expose a CLI option or configuration file setting — we may pursue those later but I see them as lower priority. I think a system-level environment variable addresses the most pressing use-cases here.

This doesn't special-case the system environment. Which means that you can use this to write to the system Python environment. I would generally strongly recommend against doing so. The insightful comment from @edmorley at https://github.com/astral-sh/uv/issues/5229#issuecomment-2312702902 provides some context on why. More generally, `uv sync` will remove packages from the environment by default. This means that if the system environment contains any packages relevant to the operation of the system (that are not dependencies of your project), `uv sync` will break it. I'd only use this in Docker or CI, if anywhere. Virtual environments have lots of benefits, and it's only [one line to "activate" them](https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment).

If you are considering using this feature to use Docker bind mounts for developing in containers, I would highly recommend reading our [Docker container development documentation](https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container) first. If the solutions there do not work for you, please open an issue describing your use-case and why.

We do not read `VIRTUAL_ENV` and do not have plans to at this time. Reading `VIRTUAL_ENV` is high-risk, because users can easily leave an environment active and use the uv project interface today. Reading `VIRTUAL_ENV` would be a breaking change. Additionally, uv is intentionally moving away from the concept of "active environments" and I don't think syncing to an "active" environment is the right behavior while managing projects. I plan to add a warning if `VIRTUAL_ENV` is set, to avoid confusion in this area (see https://github.com/astral-sh/uv/pull/6864).

This does not directly enable centrally managed virtual environments. If you set `UV_PROJECT_ENVIRONMENT` to an absolute path and use it across multiple projects, they will clobber each other's environments. However, you could use this with something like `direnv` to achieve "centrally managed" environments. I intend to build a prototype of this eventually. See #1495 for more details on this use-case.

Lots of discussion about this feature in:

- https://github.com/astral-sh/rye/issues/371
- https://github.com/astral-sh/rye/pull/1222
- https://github.com/astral-sh/rye/issues/1211
- https://github.com/astral-sh/uv/issues/5229
- https://github.com/astral-sh/uv/issues/6669
- https://github.com/astral-sh/uv/issues/6612

Follow-ups:

- #6835 
- https://github.com/astral-sh/uv/pull/6864
- Document this in the project concept documentation (can probably re-use some of this post)

Closes https://github.com/astral-sh/uv/issues/6669
Closes https://github.com/astral-sh/uv/issues/5229
Closes https://github.com/astral-sh/uv/issues/6612


---

_Label `configuration` added by @zanieb on 2024-08-29 22:21_

---

_@zanieb reviewed on 2024-08-29 22:26_

---

_Review comment by @zanieb on `crates/uv/tests/venv.rs`:81 on 2024-08-29 22:26_

This will change in #6835 

---

_Comment by @zanieb on 2024-08-29 22:58_

I tested installation into a system environment with:

```dockerfile
FROM python:3.11.9-slim-bookworm

# Add my locally built binary
ADD uv /bin/uv
RUN chmod +x /bin/uv

# Create a project
RUN uv init example
WORKDIR example

# Use the system Python environment
ENV UV_PROJECT_ENVIRONMENT="/usr/local/"

# Add httpx to the project
RUN uv add httpx

# Import it using the system Python
RUN python -c "import httpx"
```

---

_Comment by @zanieb on 2024-08-29 23:09_

@edmorley please let me know if this will work for CNBs — I find that use-case particularly compelling.

I also plan to validate this approach works well for multi-stage Docker builds.

---

_@charliermarsh approved on 2024-08-29 23:39_

---

_Marked ready for review by @zanieb on 2024-08-30 03:24_

---

_Comment by @hynek on 2024-08-30 05:38_

To clarify, since my Rust is not quite up there: given that `uv pip install` is not part of the greater "project" topic, it will ignore the variable? To me it's not too bad to run it as `uv pip install --python=$UV_PROJECT_ENVIRONMENT` but it will 100% confuse people so it maybe should be called out?

---

_Comment by @zanieb on 2024-08-30 12:07_

Yes `uv pip install` will ignore this. It's not a part of the "project" interface. _Maybe_ it should support it in a project root (as I did for venv), but I'm hesitant.

---

_Comment by @inoa-jboliveira on 2024-08-30 12:31_

Hello! Since it is called UV_PROJECT_ENVIRONMENT (emphasis on project), would it be too much of a stretch to have it be also set in pyproject.toml or uv.toml?

I understand you are focusing on docker where it is standard to set environment variables, but there are much more wide use cases for this. 

Thank you for providing this. It is better than the VIRTUAL_ENV variable, but the reason that is an env variable is because of the activation script. Otherwise a config file might be extremely useful



---

_Comment by @hynek on 2024-08-30 12:40_

> Since it is called UV_PROJECT_ENVIRONMENT (emphasis on project), would it be too much of a stretch to have it be also set in pyproject.toml or uv.toml?
> 
> I understand you are focusing on docker where it is standard to set environment variables, but there are much more wide use cases for this.

I'm not sure this makes semantically: if you're cooperating with others on a project, you're dictating on them where to put their venv. If you're working on it yourself, it would be very repetitive and a better solution would be a global config that allows to set some template or something.

Env variables aren't just a Docker thing; people use them locally for exactly these kinds customizations using `.env` files or, even better, [Direnv](https://direnv.net).

> Yes `uv pip install` will ignore this. It's not a part of the "project" interface.

Yeah I figured, and I'm not arguing for the contrary. I just think it's a potential footgun to keep in mind.



---

_Comment by @zanieb on 2024-08-30 12:46_

I think it would make sense to respect `UV_PROJECT_ENVIRONMNET` when using `uv pip install` from a project root as in https://github.com/astral-sh/uv/pull/6835 — but there's a bunch places we'd need to add inference (e.g., in every `pip` command) and we'd also need `--no-project` support. I can't do it right now — but I think it's the right behavior. cc @charliermarsh 

---

_Comment by @inoa-jboliveira on 2024-08-30 13:14_

> if you're cooperating with others on a project, you're dictating on them where to put their venv.

Unfortunately this is a reality in some projects (think about python has been around for 25 years). This prevents uv add/sync from being the norm by not providing a way to migrating while being compatible.

Then it might be finally possible for projects just forget about this need when uv is the norm. 

Being in a transition stage we need features that might not make sense if you are just trying to be purist about what is right and wrong. 

---

_Comment by @ramarnat on 2024-08-30 13:24_

This solution works for me. Thanks for the responsive turnaround and the docs on the docker dev environment too. 

---

_Comment by @zanieb on 2024-08-30 13:28_

> > if you're cooperating with others on a project, you're dictating on them where to put their venv.
> 
> Unfortunately this is a reality in some projects (think about python has been around for 25 years). This prevents uv add/sync from being the norm by not providing a way to migrating while being compatible.

I think the suggestion here is to use `.env` or similar if this is important to your project. We'll consider config-level options, but not with the urgency in which we pursued this change.

---

_Comment by @ramarnat on 2024-08-30 13:37_

This won't change `uv tool` behavior of using its own place for deployment, will it? 

---

_Comment by @charliermarsh on 2024-08-30 13:39_

> This won't change uv tool behavior of using its own place for deployment, will it?

No, it won't have any effect on `uv tool`.

---

_Comment by @inoa-jboliveira on 2024-08-30 14:57_

> if you're cooperating with others on a project, you're dictating on them where to put their venv.

I've been thinking more and more about this statement. UV does dictate where the venv is located. As if it prevents a "project maintainer" from deciding it because it was already decided and enforced for all users. Seems contradictory.



> We'll consider config-level options, but not with the urgency in which we pursued this change.

Sure, thank you! I'm in favor of any command line option or env variable being configurable in .toml files because it allows me to give users a project ready to use where they just need to learn the minimal commands and not care about extra steps. 

I used to work only in mac/linux environments, but for the past 2 years I moved to windows and there are way more challenges than in a unix based system to make things simply work

---

_Comment by @FBen3 on 2024-08-31 23:46_

First off, thank you for putting so much effort into this project, and keeping up with all the requests & issues. `uv` is awesome.

Couple of stupid questions:

1. 
> This does not directly enable centrally managed virtual environments. If you set UV_PROJECT_ENVIRONMENT to an absolute path and use it across multiple projects, they will clobber each other's environments.

~~So does this mean I can't do something like `UV_PROJECT_ENVIRONMENT=VIRTUAL_ENV` in my `.zshrc` file, and whenever I activate a `venv` it would resolve to my active virtual environment (and, e.g. `uv add requests`, would not re-create `.venv` in the project)?~~

I guess I can do something like `alias activate='source venv/bin/activate && export UV_PROJECT_ENVIRONMENT=$VIRTUAL_ENV'` to my `.zshrc` file so every time I activate / switch to a new virtual environment `UV_PROJECT_ENVIRONMENT` is updated.


2.
> We do not expose a CLI option or configuration file setting — we may pursue those later but I see them as lower priority.

Would it be easy / How does the dev team feel about having a `--sync` CLI option (that's like the opposite of `--no-sync`), which updates the active virtual environment? So users who run into issues like #6849 could just do `uv add ruff --sync`)

3. 
> Additionally, uv is intentionally moving away from the concept of "active environments" and I don't think syncing to an "active" environment is the right behavior while managing projects

What is the reason behind this? Isn't it advantageous to explicitly define which environment a user is using? Otherwise how can I feel safe I'm not modifying my global environment? 

---

_Comment by @zanieb on 2024-09-03 17:52_

Responding to the above

1. Yes, you could do that. I wouldn't recommend it, but it seems okay to use this to match your active environment.
2. I don't think we'll add a CLI option to sync the active environment. Yes, it'd be easy to implement but it comes with a lot of complexity.
3. The problem of "global environments" is something we're trying to avoid — uv's project interface doesn't ever modify a global environment so you just don't need to worry about that. You easily _could_ mutate an environment outside your current project if we just synced the active environment — this is the kind of problem we're trying to avoid. Generally speaking, activating environments shouldn't be necessary when using uv. Instead, use `uvx` and `uv run` and you'll always have the correct environment for your current context.

---

_Merged by @zanieb on 2024-09-03 17:52_

---

_Closed by @zanieb on 2024-09-03 17:52_

---

_Branch deleted on 2024-09-03 17:52_

---

_Comment by @edmorley on 2024-09-03 21:26_

@zanieb Sorry for the delayed reply. This new env var will work well for our use-case (https://github.com/astral-sh/uv/issues/5229#issuecomment-2312702902) - thank you! We'll likely also set `VIRTUAL_ENV` too (to the same location) for parity with how we set up pip/Poetry venvs, in case some other tooling checks for it (rather than checking for `sys.prefix != sys.base_prefix`), but this will be no different to pip, where we set both `PIP_PYTHON` and `VIRTUAL_ENV` already.

> Instead, use uvx and uv run and you'll always have the correct environment for your current context.

So this probably something you've already considered/are aware of (and were instead talking mainly about the host-machine development use-case), but in a container context, it will be reasonably common to exclude `uv` from the final app image (in the case of `Dockerfile` via multi-stage builds, or in the case of CNBs via build-only layers), in which case the final app image can't wrap commands with `uv run` in eg `CMD` / `ENTRYPOINT`, and instead containers that are using venvs (for the reasons in https://github.com/astral-sh/uv/issues/5229#issuecomment-2312702902) will need to have "activated" the venv by adding it to `PATH` etc instead.

---

_Comment by @zanieb on 2024-09-03 21:32_

Yeah totally agree — but that use-case doesn't require uv to read and mutate an "active" virtual environment.

---
