```yaml
number: 6459
title: "Run `uv sync` without creating venv (similar to poetry config) (I believe `uv pip` has functionality like that)"
type: issue
state: closed
author: adiberk
labels:
  - needs-decision
assignees: []
created_at: 2024-08-22T19:13:28Z
updated_at: 2025-08-17T00:32:13Z
url: https://github.com/astral-sh/uv/issues/6459
synced_at: 2026-01-12T15:59:04Z
```

# Run `uv sync` without creating venv (similar to poetry config) (I believe `uv pip` has functionality like that)

---

_@adiberk_

Our current Dockerfile install command with poetry - we can disable venv creation
`poetry config virtualenvs.create false...`

It would be nice to be able to run `uv sync` with a similar command `uv sync --no-venv` or something.  This way we don't have to change how our app is built and run, as well as commands that get run when exec into docker
As of now using a venv in our Dockerfile causes too much of a headache (at least from what i can tell)


---

_Renamed from "Is there a way to run uv sync without creating venv (similar to poetry)" to "Run uv sync without creating venv (similar to poetry) (using --system doesn't seem to work for me)" by @adiberk on 2024-08-22 19:14_

---

_Renamed from "Run uv sync without creating venv (similar to poetry) (using --system doesn't seem to work for me)" to "Run uv sync without creating venv (similar to poetry) (I believe this is what uv pip --system does)" by @adiberk on 2024-08-22 19:15_

---

_Renamed from "Run uv sync without creating venv (similar to poetry) (I believe this is what uv pip --system does)" to "Run uv sync without creating venv (similar to poetry config) (I believe this is what uv pip --system does)" by @adiberk on 2024-08-22 19:18_

---

_Renamed from "Run uv sync without creating venv (similar to poetry config) (I believe this is what uv pip --system does)" to "Run uv sync without creating venv (similar to poetry config) (I believe this is what uv pip has functionality like that)" by @adiberk on 2024-08-22 19:18_

---

_Renamed from "Run uv sync without creating venv (similar to poetry config) (I believe this is what uv pip has functionality like that)" to "Run uv sync without creating venv (similar to poetry config) (I believe `uv pip` has functionality like that)" by @adiberk on 2024-08-22 19:19_

---

_Renamed from "Run uv sync without creating venv (similar to poetry config) (I believe `uv pip` has functionality like that)" to "Run `uv sync` without creating venv (similar to poetry config) (I believe `uv pip` has functionality like that)" by @adiberk on 2024-08-22 19:19_

---

_Comment by @charliermarsh on 2024-08-22 19:26_

Not yet but we're likely to support it soon.

---

_Comment by @mike-taylor-nomihealth on 2024-08-27 10:39_

Just wanted to add some support for this use case - we would love to use uv sync to install at the system level so we don't have to do it separately and get the advantage of a uv sync layer in the docker build (https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers) and more consistency between our production and dev builds in docker - the uv run use case is great for a local install that doesn't conflate itself with my system packages, but it makes it more troublesome when trying to install into a container since then we have to do something like `uv run pip install --system -r pyproject.toml` instead of something like a `uv sync --lock --system` directly.  

It's not that we don't trust you guys to build cool stuff, it's just adding a layer of abstraction to our containers by running `uv run <command>` doesn't really make sense to add the risk of a `uv` bug impacting production when we could potentially use it more directly to get similar benefits in our container builds.

It seems like we can do someting similar with the system layer with the `uv pip install -r` as an intermediary build instead though, so this might just be my ignorance on why we would/woudln't want to support using the `uv sync` to manage system packages and not just the venv ones

---

_Comment by @leddy231 on 2024-08-27 11:39_

We need this as well coming from a similar poetry setup.

But perhaps it should not be a functionallity of `uv sync`, as it is designed to work with virtual envs and add / remove packages to match the lockfiles.

Would it make more sense to allow `uv pip install` to be able to read `uv.lock` files? Then the command would be
`uv pip install -r uv.lock --system` 
With an optional `uv lock --locked` to make sure the lockfile is up to date.

This would also allow installing multiple projects in the same system python without it uninstalling previous packages (which im guessing `uv sync` would do?). This is also something we need, becuse we still have just a single dockerfile for our multiple projects :sweat_smile: 

Edit discussion for that here #6670 

---

_Comment by @zanieb on 2024-08-27 11:50_

@mike-taylor-nomihealth Does the example at https://github.com/astral-sh/uv-docker-example and the [latest Docker guide](https://docs.astral.sh/uv/guides/integration/docker/) (just updated now) help? We have support for intermediate layers and it's a one-line change to avoid using `uv run`. No need to use `uv pip`.

> we would love to use uv sync to install at the system level so we don't have to do it separately

Can you clarify how system level syncing would improve layer caching?

> This would also allow installing multiple projects in the same system python without it uninstalling previous packages (which im guessing uv sync would do?).

Sorry, but this is a use-case I don't think `uv sync` should support — if you need to install multiple projects side-by-side you need to use workspaces and a single lockfile otherwise you're throwing out all of the guarantees that the project interface is built on.



---

_Assigned to @zanieb by @zanieb on 2024-08-27 11:50_

---

_Label `needs-decision` added by @zanieb on 2024-08-27 11:50_

---

_Comment by @leddy231 on 2024-08-27 11:53_

> Sorry, but this is a use-case I don't think uv sync should support — if you need to install multiple projects side-by-side you need to use workspaces and a single lockfile otherwise you're throwing out all of the guarantees that the project interface is built on.

Yes thats what i meant, `uv sync` should not be able to do that, but raw `uv pip install` should (and yes it would not be good practice, but sometimes needed)

---

_Comment by @adiberk on 2024-08-27 12:59_

Right, so I guess just having the ability to run `uv pip install —lock` or something would work as well. Assuming it basically installs and resolves dependencies in the same way as uv sync (except that you can specify system etc.). It does seem that uv sync isn’t meant to allow system install, which I hear.

As of now I have been able to get around it by creating a venv in a different workdir and then setting that venv in system path.

---

_Comment by @xymz on 2024-08-29 06:00_

FYI, In some cases(especially on CI), I also need something like `uv sync --system` or `uv pip install -r pyproject.toml --system --with-dev`. There is a workaround for last one(https://stackoverflow.com/a/78902981) but I think it is not follow uv standard. And it is not cool add some extra dependencies for CI(of course, It is could be meaningful if there is a difference) because it's redundant.

Yes, it's clear that we need a consistent option for using uv without virtualenv in special cases. Or we'll always have to spend time finding workarounds for every edge case.

---

_Comment by @zanieb on 2024-09-05 00:24_

This is supported now via #6834

---

_Closed by @zanieb on 2024-09-05 00:24_

---

_Comment by @xymz on 2024-09-05 01:54_

@zanieb Nice and cool. Thanks for your considerate work.

---

_Comment by @dpnova on 2024-11-14 09:12_

A use case here is we're deploying to AWS lambda using their container feature. Installing things outside the system folder is a pain... 

I'm thinking `sync` is not for us and we should use export to get a requirements file to install.

---

_Comment by @mc51 on 2025-08-16 21:39_

> A use case here is we're deploying to AWS lambda using their container feature. Installing things outside the system folder is a pain...
> 
> I'm thinking `sync` is not for us and we should use export to get a requirements file to install.

@dpnova: I was (probably) trying to solve the same issue. Turns out, it's possible now using [this](https://github.com/astral-sh/uv/pull/6834) which was mentioned [above](https://github.com/astral-sh/uv/issues/6459#issuecomment-2330368355).

In the `Dockerfile` for the lambda deployment, I'm using `FROM public.ecr.aws/lambda/python:3.12`.
Now, we set `ENV UV_PROJECT_ENVIRONMENT="/var/lang/"`.  The system python binary is in `/var/lang/bin/` for this image. So, uv will operate on the system python. Following, we can use `uv sync` with our lock file to install dependencies to the system.   
However, because `uv sync` will also [delete dependencies from the system](https://github.com/astral-sh/uv/pull/6834#issue-2495678347) which aren't defined for our project, **one must be cautious**. For example, we'll need to make sure `awslambdaric` is added as a dependency, otherwise lambda will fail.



---

_Comment by @dpnova on 2025-08-17 00:32_

Thanks @mc51 I suspect my attempts were removing deps as you say... Good info, cheers! 

---
