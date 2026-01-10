---
number: 6612
title: "`uv add`/`uv sync`/... do not respect the active virtualenv, should they?"
type: issue
state: closed
author: akx
labels: []
assignees: []
created_at: 2024-08-25T16:40:19Z
updated_at: 2025-05-18T10:12:38Z
url: https://github.com/astral-sh/uv/issues/6612
synced_at: 2026-01-10T01:24:03Z
---

# `uv add`/`uv sync`/... do not respect the active virtualenv, should they?

---

_Issue opened by @akx on 2024-08-25 16:40_

Related to #1495, #5229, but not exactly the same IMO.

Like #1495, I prefer to have my virtualenvs located centrally instead of in project directories. I have a Fish shell function called `v <NAME>` that activates a virtualenv `~/envs/<NAME>` if available and cds to the related project directory `~/build/<NAME>` if available.

I wanted to try out `uv add` and `uv sync` and was initially somewhat confused when they didn't seem to do anything, only to realize that they currently always work on the `~/.venv` virtualenv, and don't even stop to consider a currently active virtualenv to work on.

There's apparently a workaround to symlink `.venv` to the centrally-located venv, but that also seems a bit messy too.

I think it should be at least an (envvar or config-file) option to have the "project venv" tools (`run`, `add`, `remove`, `sync`, etc.) consider the currently active virtualenv, if any.

Pretty minimal Docker container repro:

```
~ $ docker run -it python:3.12 bash
root@7ab1d7a6054d:/# pip install uv
Successfully installed uv-0.3.3
root@7ab1d7a6054d:/# mkdir ~/envs
root@7ab1d7a6054d:/# uv venv ~/envs/myenv
Using Python 3.12.4 interpreter at: usr/local/bin/python3
Creating virtualenv at: root/envs/myenv
root@7ab1d7a6054d:/# source ~/envs/myenv/bin/activate
(myenv) root@7ab1d7a6054d:/# echo $VIRTUAL_ENV
/root/envs/myenv
(myenv) root@7ab1d7a6054d:/# mkdir -p ~/projects/myproject
(myenv) root@7ab1d7a6054d:/# cd ~/projects/myproject
(myenv) root@7ab1d7a6054d:~/projects/myproject# uv init
Initialized project `myproject`
(myenv) root@7ab1d7a6054d:~/projects/myproject# uv add ruff
Using Python 3.12.4 interpreter at: /usr/local/bin/python3
Creating virtualenv at: .venv
Resolved 2 packages in 275ms
   Built myproject @ file:///root/projects/myproject
Prepared 2 packages in 983ms
Installed 2 packages in 1ms
 + myproject==0.1.0 (from file:///root/projects/myproject)
 + ruff==0.6.2
(myenv) root@7ab1d7a6054d:~/projects/myproject#
(myenv) root@7ab1d7a6054d:~/projects/myproject# uv pip list
(myenv) root@7ab1d7a6054d:~/projects/myproject#
```


---

_Comment by @MithicSpirit on 2024-08-25 17:24_

I'm a bit of a configuration junkie, so my immediate opinion is "yes—under a configuration option". If a configuration option is deemed too complex for this, I think it'd still be nice to make it the default behavior to support the "external venv" workflow, though a message should be printed if the active venv seems to be outside the current project. If the current project has a venv, but the active venv is different, I'm not sure which should be used by uv; maybe it should just result in an error.

---

_Assigned to @zanieb by @charliermarsh on 2024-08-26 14:33_

---

_Comment by @zanieb on 2024-08-26 17:39_

At the very least, I think we should warn if there's an active virtual environment that isn't the target.

We're very hesitant to sync to active virtual environments. I'll be considering this as we hear more use cases.

---

_Comment by @ramarnat on 2024-08-26 19:13_

This is confusing to me, If I do:
```
uv venv /opt/venv && \
uv sync
```
Shouldnt the `uv sync` respect that I created the venv in the first step, it does not seem to do so. What is the use case for `uv venv`, just `uv pip`?




---

_Comment by @zanieb on 2024-08-26 19:21_

Yeah, `uv venv` isn't really for use with the project interface. It's for manual management of virtual environments, which is generally the purview of `uv pip`.

---

_Comment by @ramarnat on 2024-08-26 19:26_

here's our use case that is tripping me up. We have a docker image that gets built and we use an activated venv in there. During development/testing, this docker image is also used and we mount the local development directory `./app:/app`

I dont want the dev/test to use the local `./app/.venv` because that was built for osx, and it should use the activated venv. 

For now I have worked around it by setting up the build to use a link from `app/.venv` to `/opt/venv` and it seems to work.

---

_Comment by @zanieb on 2024-08-26 19:31_

Yep! We agree the Docker volume mount is a big problem and we are working on designing a recommended solution.

Does adding `.venv` to `.dockerignore` not work?

---

_Comment by @ramarnat on 2024-08-26 19:34_

`.dockerignore` does not impact a mount. So for dev we are doing `-v ./app:/app` when running the docker image created with the activated venv.

---

_Comment by @zanieb on 2024-08-26 20:15_

Yep, just struggled with that myself.

Have you tried [docker compose watch](https://docs.docker.com/compose/file-watch/#compose-watch-versus-bind-mounts)? Is that an option for you?

---

_Comment by @zanieb on 2024-08-26 20:22_

Also, have you tried `--volume .:/app --volume /app/.venv`? This appears to be working for me.

---

_Comment by @ramarnat on 2024-08-26 20:26_

ah, watch is something is new, and did not know about. I'll try both options. 

---

_Comment by @zanieb on 2024-08-26 20:45_

Thanks! Let me know if you have any problems — I'm writing some examples up with those.

---

_Comment by @inoa-jboliveira on 2024-08-26 20:47_

Hi, I was going to add a similar issue here. I think more than respecting current active virtualenv, they should also respect the `VIRTUAL_ENV` environment variable.

I would prefer if we could select the virtualenv folder in uv.toml or pyproject.toml 

For historical reasons, on some projects the name of the env folder is relevant (and .venv is also an arbitrary name)

---

_Comment by @ramarnat on 2024-08-26 21:11_

> Also, have you tried `--volume .:/app --volume /app/.venv`? This appears to be working for me.

that one did not work for me for one of the cases I am trying, because then I cant do `uv sync` to add the dev dependencies, without it trying to add everything again, and not use what's already there in `/opt/venv`. If I didnt have to add the development deps, then I think that would work. 

the `watch` with this stanza seems to be working. Have to go back and change the flow that calls docker directly to use docker compose to see if this will round out the existing cases to use `uv` instead of pip
```
    develop:
      watch:
        - action: sync
          path: ./app
          target: /app
          ignore:
            - .venv/  
```
 


---

_Comment by @zanieb on 2024-08-26 21:14_

> that one did not work for me for one of the cases I am trying, because then I cant do uv sync to add the dev dependencies, without it trying to add everything again, and not use what's already there in /opt/venv. If I didnt have to add the development deps, then I think that would work.

Can you share a concrete example? I'm not quite following and am happy to debug.

---

_Comment by @ramarnat on 2024-08-26 21:29_

assuming `mydockerimage` was created in two steps, first a `build` stage with `/opt/venv` as the activated venv, with `/app/.venv` symlinked to `/opt/env` and uses `uv sync --locked -no-dev` to build out the venv. Then in the second stage, looks like this:

```
FROM python:3.12.3-slim AS runtime

ARG APP_VERSION=default
# Make sure we use the virtualenv:
ENV PATH="/opt/venv/bin:$PATH"
ENV APP_VERSION=${APP_VERSION}
RUN echo "Building image on version: ${APP_VERSION}"

# needed if used make test-docker etc.
ENV VIRTUAL_ENV="/opt/venv"
COPY --from=build /bin/uv /bin/uv

WORKDIR /app
COPY --from=build /opt/venv /opt/venv
COPY . /app

EXPOSE 8000

CMD ["python", "-m", "app.main"]
```

Then if I try the `docker run`:
```
docker run  -v .:/app -v /app/.venv -w /app -it mydockerimage bash -c "uv sync --locked"
```
which if I am understanding your suggestion correctly, it will override `/app/.venv` with an empty directory, because /app/.venv does not exist locally, It fails with:
```
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3
Using Python 3.12.3 interpreter at: /usr/local/bin/python3
error: failed to remove directory `/app/.venv`
  Caused by: Resource busy (os error 16)
```

---

_Comment by @zanieb on 2024-08-26 21:39_

Sorry I think I'll need to see a complete minimal example. Perhaps you could derive a small multi-stage image as you describe from this (work in progress) example: https://github.com/astral-sh/uv-docker-example

---

_Comment by @ramarnat on 2024-08-26 21:43_

ok, let me adapt that example in a fork, and push it in a few

---

_Comment by @ramarnat on 2024-08-26 22:59_

ok, I see the bit that I was missing. I need to copy the `/app` folder from the build over to the runtime stage, and that will bring the `/app/.venv` volume over for it to be mounted. Let me play around with this a bit more. 

---

_Comment by @JanMurmann on 2024-08-27 09:32_

This would be very helpful for us, too.

Our use case is that we run tox in our pipeline and the first step is to verify and install the requirements for the unit tests. So basically we would like to do `uv sync --extra test --locked` with the current tox virtualenv being the target of the sync.

---

_Comment by @akx on 2024-08-27 10:46_

> We're very hesitant to sync to active virtual environments.

I kind of understand (though not exactly fully).

I think I would be pretty happy if the sync/add/... commands accepted e.g. `--allow-active-environment` (or more broadly, I guess, `--use-environment=default/active`..?) which could be set via envvar (`UV_ALLOW_ACTIVE_ENVIRONMENT=1`, `UV_USE_ENVIRONMENT=active`?) or other system-wide config, so I could set that in my usual dev environment.

(I suppose `--use-environment=active` should also imply requiring an active virtualenv, and the tools wouldn't then create the `.venv` default venv.)


---

_Comment by @FBen3 on 2024-08-27 19:07_

> We're very hesitant to sync to active virtual environments.

Can you explain the reasoning behind this? I thought most users activate a virtual environment to ensure they're not modifying anything globally.

similar to @akx I keep my virtual environments outside of the repo to save space. Then have an alias to activate whichever environment I need or want to test. It would be nice to have an `--active-env` flag that can be used with `sync`/`add`/... commands, or set some config, to allow for this. 

---

_Comment by @DanCardin on 2024-08-27 23:56_

It seems kind of wild to not respect VIRTUAL_ENV, for any command honestly. I'm not sure I can imagine a drawback. in the happy path, no one is setting it, or otherwise activating the virtualenv. but in every other case, it's the path of least surprise to me.

I frequently use quick swapping of virutalenvs under different names to fast test across multiple versions of python. I personally do so with my own tool (currently, would love to stop using it) `prp -n p39; pytest`, `prp -n p310; pytest`, where my venvs live somewhere else entirely....Granted with `uv`, `uv sync --python 3.9` takes all of 1s to swap, it does unnecessarily kill and recreate the venv each time, and output 2 pages of text.

Somewhat tangentially related, I submitted https://github.com/astral-sh/uv/issues/1495 a while back basically to facilitate this workflow. Where ideally i wouldn't even necessarily be activating any of the venvs, if i could be doing something like `uv -p 3.9 sync`, `uv -p 3.9 run pytest`(then swap to `-p 3.10`); and if under the hood it was just reading/writing to different venv paths external to the project folder (ideally based on `pwd` and the python version, since `./.venv` clobbers itself, and any other local path starts creating clutter). Then in **that** world, I might no longer have a need or reason to activate a venv in the first place.

---

_Comment by @charliermarsh on 2024-08-27 23:58_

> ... if under the hood it was just reading/writing to different venv paths external to the project folder (ideally based on pwd and the python version, since ./.venv clobbers itself, and any other local path starts creating clutter). Then in that world, I might no longer have a need or reason to activate a venv in the first place.

Just a brief drive-by comment: this is something we've considered but cut from scope for last week's release :)


---

_Comment by @blast-hardcheese on 2024-08-28 05:10_

👋 Big +1 for the principle of least surprise when it comes to not just using the active `VIRTUAL_ENV`.

I'm sympathetic towards not wanting to try and heal a broken virtualenv (if indeed this is the motivation), and there are at least a few places in the documentation where I've seen "unexpected behavior" if uv ends up executing a different python binary than the one living in the virtualenv for one reason or another.

As a workaround, I've discovered that `.venv` being a symlink does not seem to hinder operation, so `prp -n p39` can be replaced with `ln` (for example, or in my case attempting to adapt `.venv` to `.pythonlibs` which is the standard venv location on Replit)

Considering venvs are already roughly ephemeral, even if `uv` completely wrecks an existing virtualenv I think this'll strengthen the ecosystem in the long run due to increased expectation of interoperability.

(New user here, I'd like to take the opportunity to express my gratitude both to the team as well as all contributors for the fantastic tool 🙂)

---

_Comment by @jamesharris-garmin on 2024-08-28 20:32_

I'm really interested in how this effects the tox/nox workflow since, prior to this release I have been using nox for most of my projects/script management.

Do we have any suggestions on how to handle similar workflows with the project/workspace interface?

---

_Comment by @JonathanRayner on 2024-08-29 11:07_

I would prefer it if e.g. `uv sync` would respect `VIRTUAL_ENV`.

It was the most surprising thing I found when starting to use `uv` - I commonly have active virtual environments that I want to be obeyed by `uv` commands. I think the fact that the environment is active is good indication that the user intends to use it and if they don't, that's a user error.

It also is a surprising difference from `uv pip install`, which does respect `VIRTUAL_ENV`. I think it would be great if `uv sync` had similar/consistent behavior.

---

_Comment by @inoa-jboliveira on 2024-08-29 16:32_

I don't know if anyone got my suggestion above, but it would be great if there was a config in pyproject.toml to refer to the virtual env path besides the VIRTUAL_ENV variable (of course an active env would take priority). 
This makes it possible to run uv sync/add as intended without activating the env manually.

```
[tool.uv]
virtual_env = '.my_venv'
```
or 
```
[tool.uv]
virtual_env = '/usr/local/my_global_env'
```

I'm very eager to use this new add/sync interface! Thank you guys for such cool project

---

_Comment by @DanCardin on 2024-08-29 17:23_

If that was going to be a feature, `uv`-specific (i.e. not project-specific, i.e. not pyproject.toml) config would make a lot more sense to me. The virtualenv usage/location is definitely in the realm of developer-specific concern imo, not project-specific concern.

---

_Comment by @inoa-jboliveira on 2024-08-29 17:42_

Maybe you are thinking about libraries, but it is common for a project meant to run directly to have several strong opinions on where things are located.

According to the [latest 0.4.0 release notes](https://github.com/astral-sh/uv/releases/tag/0.4.0), it seems to be a goal of uv to cater to theses projects.

Direct quotes from the maintainer:

> Most users are not developing libraries that need to be packaged and published to PyPI. Instead, they're building applications using web frameworks, or running collections of Python scripts in the project's root directory. 

> This release adds first-class support for Python projects that are not designed as Python packages (e.g., web applications, data science projects, etc.).

---

_Comment by @DanCardin on 2024-08-29 18:32_

i mean, it's ultimately up to them, but it seems weird to me for the project (versus myself or the tool) to have control over where my venv goes, regardless of the usecase. it totally makes sense for **uv** to have strong opinions or for the user. but i just really dont see a reason for the project itself to care (**particularly** with a tool like `uv`, where you can write your project tooling to use `uv run` or whatever and expect it to do the right thing).

---

_Referenced in [astral-sh/uv#6834](../../astral-sh/uv/pulls/6834.md) on 2024-08-29 22:39_

---

_Comment by @zanieb on 2024-08-29 23:17_

Take a look at https://github.com/astral-sh/uv/pull/6834 and let me know if it is sufficient for you. Please remember to be considerate.

---

_Comment by @DanCardin on 2024-08-30 00:32_

I certainly dont think there's anything wrong with uv also having it's own env var that takes precedence over VIRTUAL_ENV, whether or not you respected it, the PR still seems like a net improvement in the mean time.

But in my mind, all of the reasons you state in the PR for being reasons against supporting `VIRTUAL_ENV` also sort of apply to this new env var.

* If someone has `UV_PROJECT_ENVIRONMENT` set, but not `VIRTUAL_ENV`: `uv` essentially has an "active" environment but all the preexisting python tooling wont work properly
* If someone has `VIRTUAL_ENV` set but not `UV_PROJECT_ENVIRONMENT`: all other tooling operates on that venv, but `uv sync` wont

So if someone's going to use this env var for any of the local-development usecases (docker/ci both seem addressed by the PR either way) either forces one to set both or prefix everything with one runs interactively with `uv run`. Which is maybe fine but i'm mostly saying to point out i dont know that it necessarily solves the problems you're describing as why you'd prefer to not read VIRTUAL_ENV.

For my own purposes of fast swapping between python versions interactively, which is how i landed here in the first place, this env var is inconvenient as a solution relative to the standard env var for the reasons stated above, but i suppose perhaps #1495 will have to be the solution.

---

_Referenced in [astral-sh/uv#6849](../../astral-sh/uv/issues/6849.md) on 2024-08-30 03:32_

---

_Comment by @pawamoy on 2024-08-30 16:19_

I have a use-case similar to @DanCardin's.

Basically, I don't use tox/nox, and instead prepare one venv per Python version I support. Then I install deps in each one of them, and use scripts to run the same commands in all of them, one after the other.

```bash
# Create default .venv, as well as .venvs/3.8, .venvs/3.9, etc., and install deps in each
make setup

# Run tests in all venvs
make multirun duty test  # shortcut: make test

# Serve docs from default env
make run duty docs  # shortcut: make docs

# Install something in all venvs, including default env
make allrun uv pip install ...
```

Currently my `make setup` command loops on `.venv` and the venvs in `.venvs`, and each time activate the venv then calls `uv pip compile ... | uv pip install -r -` (basically). I wanted to update the command to use `uv sync`, but this command doesn't care about the activated venv, so it doesn't work.

Ideally, I'd love that `uv sync` uses the activated env, or at least a CLI switch like `uv sync --venv .venvs/3.8`, but I can live with the `UV_PROJECT_ENVIRONMENT` environment variable from #6834 :slightly_smiling_face: 

---

_Comment by @epicserve on 2024-08-31 16:31_

It's very confusing to me why `uv sync` doesn't respect the `VIRTUAL_ENV` environment variable. Even according to the [docs](https://docs.astral.sh/uv/configuration/environment/), it should be respecting it.
> VIRTUAL_ENV: Used to detect an activated virtual environment.

The following should just work. I would expect this to create a new virtualenv in `/opt/venv`, if one doesn't exist and then install all of the projects dependencies.
```
export VIRTUAL_ENV=/opt/venv
uv sync
```

At the very lest the docs should be updated to explain what commands respect the `VIRTUAL_ENV` environment variable and why others don't.

In the Docker context, I don't really want to be forced into creating my virtualenv in `.venv`.

---

_Comment by @JanMurmann on 2024-09-02 06:57_

> Take a look at #6834 and let me know if it is sufficient for you. Please remember to be considerate.

Yes, that would be sufficient for our use case. Thanks a lot for your work!

---

_Comment by @FBen3 on 2024-09-02 18:24_

> Take a look at https://github.com/astral-sh/uv/pull/6834 and let me know if it is sufficient for you.

Thank you for the swift PR regarding this! I think I'll be able to set `UV_PROJECT_ENVIRONMENT` in way I explained in my [comment](https://github.com/astral-sh/uv/pull/6834#issuecomment-2323073977), so this works for me.


---

_Comment by @blast-hardcheese on 2024-09-02 23:14_

I also agree, that would solve my issues as well! Thank you for the quick resolution here!

---

_Comment by @zanieb on 2024-09-03 14:53_

@epicserve that is discussed a bit in the summary of #6834 — we can add more documentation around this though.

Thanks for looking at the pull request everyone.

---

_Closed by @zanieb on 2024-09-03 17:52_

---

_Closed by @zanieb on 2024-09-03 17:52_

---

_Comment by @rickyzhang82 on 2024-12-23 20:22_

This is the pain in the a**. Why not respect env `VIRTUAL_ENV` and use the new one `UV_PROJECT_ENVIRONMENT`. The tool supposed to improve our productivity. But now everyone has to pay a toll to figure this out. It makes no sense to me.

---

_Comment by @zanieb on 2024-12-24 01:13_

@rickyzhang82 that's not a productive or acceptable way to engage with this project. I'm sorry this is confusing for you, but if you want it to be changed you need to contribute something new to the discussion.

---

_Comment by @inoa-jboliveira on 2024-12-24 16:32_

@zanieb I agree it was not really respectful of the project, but I cant help but say this is truly my biggest pet peeves. Possibly one of the easiest things to fix to improve quality of life.

Working with multiple python versions at the same time make it weird with it deleting my venv and creating a new. Specially when I am not fully commited to a lib and have it just installed with `uv pip` for testing purposes. My IDE goes crazy because it does not re-fetch the python version.

Of course all this can be fixed by changing how people do things but it is indeed a painful transition. The only argument I see is the one above:

> We're very hesitant to sync to active virtual environments.

I mean destroying and creating a .venv just because is the conventional name is way worse...

`.venv` is really just a convention and there is nothing requiring it to be the rule. The only formal mention I found was the now Withdrawn [PEP 704](https://peps.python.org/pep-0704/).

The env variable solution has yet another big issue which is to break under relative paths and it is annoying to set env variables for dev environments on cross platform teams.

If there is room for reconsideration, I also would be in favor. Maybe on another issue?

---

_Comment by @rickyzhang82 on 2024-12-24 21:20_

@zanieb 

I can't use `.env` under my project root because I have multiple Python versions. Some virtual environments may be shared with other projects as well due to the size. It is also unacceptable to ask everyone to provide this special env to workaround this issue.

When I use `uv pip install ...`, the packages are installed in the correct virtual environment that I specified by `uv venv`. But when I do `uv sync,` it doesn't respect my existing venv option but creates a `.venv` directory. I can't wrap my head around understanding this behavior, no matter how your document is going to describe the motivation.

I appreciate the performance of `uv pip.` But if you want to make this tool go beyond package management, you need to listen to the users. 

I tried `uv lock`. However, once the lock file is stored in Git and propagates between Mac and Linux platforms, `uv sync` accidentally deletes the correct packages from different platforms. IMHO, this feature is not mature enough to push it.


---

_Comment by @zanieb on 2024-12-28 17:12_

@rickyzhang82  it sounds like you want `uv pip sync pyproject.toml` instead? If you're using an _environment_-centric workflow then the `uv pip` interface is for you. The top-level `sync` interface is _project_-centric and is not designed to share environments across projects.

> However, once the lock file is stored in Git and propagates between Mac and Linux platforms, uv sync accidentally deletes the correct packages from different platforms. 

This doesn't make sense to me. Feel free to open an issue with the specifics, we prioritize bug fixes.

---

_Referenced in [johnthagen/python-blueprint#267](../../johnthagen/python-blueprint/issues/267.md) on 2025-04-10 14:52_

---

_Comment by @RafalSkolasinski on 2025-05-18 00:02_

> it sounds like you want uv pip sync pyproject.toml instead?

@zanieb would that make use of `uv.lock` file? I am looking to replace my current workflow which includes doing `poetry install` when having activated conda environment. I want to sync locked packages into conda environment.

---

_Comment by @charliermarsh on 2025-05-18 00:03_

(If you're trying to sync to an active Conda environment, did you try `uv sync --active`?)

---

_Comment by @RafalSkolasinski on 2025-05-18 00:06_

HI @charliermarsh, yes, I did. And it continues to create `.venv` environment for me and install into it. I was confused if installing into conda envs is supported through it and started digging, hence here I am. 

I have conda env activated and still I am getting 
```
Using CPython 3.13.3
Creating virtual environment at: .venv
Resolved 2 packages in 0.84ms
Installed 1 package in 5ms
 + ruff==0.11.10
```

if this is unexpected I will open (tomorrow) a bug report

---

_Comment by @charliermarsh on 2025-05-18 00:38_

Just re-read and we do _not_ support Conda environments with `--active`. I can't remember if that's intentional or not.

---

_Comment by @RafalSkolasinski on 2025-05-18 10:12_

Found https://github.com/astral-sh/uv/issues/11315 

---

_Referenced in [astral-sh/uv#15603](../../astral-sh/uv/issues/15603.md) on 2025-08-31 08:56_

---
