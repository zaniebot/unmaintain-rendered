```yaml
number: 5653
title: uv should respect uv.lock in the workspace root when invoking pip install -target in a subproject
type: issue
state: open
author: jpambrun-vida
labels: []
assignees: []
created_at: 2024-07-31T14:08:06Z
updated_at: 2025-07-12T02:19:22Z
url: https://github.com/astral-sh/uv/issues/5653
synced_at: 2026-01-12T15:58:57Z
```

# uv should respect uv.lock in the workspace root when invoking pip install -target in a subproject

---

_@jpambrun-vida_

I have a worspace with a virtual top level project. uv sync correctly install and create an `uv.lock` file.

Later I need to create multiple docker images from subprojects and I want to narrow the dependencies for each.

My current attempt involves invoking from subproject
```
WORKDIR services/some/subproject
RUN --mount=type=cache,target=/root/.cache/uv uv pip install --compile-bytecode -r pyproject.toml --target /deps/
```

However, testing locally by editing the lock files doesn't seem to have any effect (I could be doing this wrong). Also, debug logs with `-v` show no activity around uv.lock.

This might be a bit related to #5008.

---

_Comment by @zanieb on 2024-07-31 14:20_

`uv pip install` will not respect the lock file (and probably never will). I think #5008 is the proper solution.

---

_Comment by @jpambrun-vida on 2024-07-31 14:26_

I am not sure I see a path with `uv sync --package ...` to create individual docker images for each service with the proper locked subset of dependency? 

---

_Comment by @jpambrun-vida on 2024-07-31 14:36_

[This comment](https://github.com/astral-sh/uv/issues/5008#issuecomment-2260651468) did help me understand the intent.

I think it could work, but `pip install --target` is a bit different as it just it creates a folder to add to PYTHONPATH while I imagine sync implies a venv that needs to be activated.

---

_Comment by @jpambrun-vida on 2024-08-02 13:58_

I think this works well if you intend to deploy as a venv to production, but I am trying to avoid that. 

Ideally I would have to venv workflow in dev with editable packages, but In prod I would like to have a normal folder with all deps added to PYTHONPATH. Ultimately, this is for AWS lambdas and I am trying to use their base image without changing the entrypoint.

I need the prod dependency to be locked, which appears impossible with `uv pip install -r xyz/pyproject.toml --target`. As an alternative I thought I could `uv sync --compile-bytecode --no-dev --package xyz`  and copy `.venv/lib/python3.10/site-packages/` (although clunky), but I end up with a bunch of `__editable__.abc-0.1.0.pth` with hard coded path. I need those `.pth` in dev, but I want it copied for prod.

I could be seeing this all wrong

cc @charliermarsh 

---

_Comment by @jpambrun-vida on 2024-08-02 18:10_

To add to this. uv sync doesn't seem to respect `VIRTUAL_ENV`? How are we supposed to install packages using uv.lock outside of the `.venv` in the source tree?

---

_Comment by @charliermarsh on 2024-08-02 18:31_

That's not supported right now. The lockfile is only intended for use in the workspace environment. Very likely we'll support it but we haven't committed to an API yet.

---

_Comment by @paveldikov on 2024-08-03 23:20_

My use case is probably not the exact same as this ticket's (happy to open another one!), but since the discussion is heading in a somewhat related direction:

> uv pip install will not respect the lock file (and probably never will). [@zanieb]

> The lockfile is only intended for use in the workspace environment [@charliermarsh]

Is this a permanent limitation, or something you'd be willing to relax? I think the value there is immense; e.g. when building a Docker image of my package I definitely do not want to `uv sync` an entire development workspace -- but I certainly do want my image to use the same dependency versions as the ones I developed/tested against.

Right now our approach is a very simple `RUN uv pip install my-freshly-built.whl -c constraints.txt` -- would there be a lockfile equivalent for this operation?

(either that, I guess projects would have to maintain lockfiles and constraints files side-by-side, but that seems duplicative and possibly error-prone?)

---

_Comment by @charliermarsh on 2024-08-03 23:27_

> Is this a permanent limitation, or something you'd be willing to relax?

I'm pretty open-minded about it right now. Is it something we can solve outside of the `pip` API though? Like `uv sync --no-root` or some other commands to better control what gets included?


---

_Comment by @jpambrun-vida on 2024-08-05 12:19_

The new `uv sync --package` helps, but 1) I wish I could avoid venv altogether and have something equivalent to pip's `--target` and 2) I wish I had a way disable the __editable__[...].pth mechanism and just copy the files.

I do think the path to creating a production docker image with locked dependencies is a bit cumbersome. Some documentation and best practice would help.

---

_Comment by @charliermarsh on 2024-08-05 12:22_

Can you expand a bit on why either of these two things are a problem?

---

_Comment by @jpambrun-vida on 2024-08-05 17:15_

1) means you need to invoke python within the venv which is not always possible or desirable, like when using the aws lambda base image or datadog's wrapper.

2) the pth is absolute which means it can be relocated.

Nothing is unsurmountable.. I wouldn't call that a "problem", more like "sub-optimal"?  but it feels weird to use venv in docker as they are both different ways to solve the same problem. 

There was a [similar argument](https://github.com/python-poetry/poetry/issues/1937) on poetry. I don't feel that strongly about it, but many of the folks there did.

---

_Comment by @jpambrun-vida on 2024-08-06 12:22_

I just stubbled a real use case that I can't see any workaround for. As I am converting the  current build system at work, I need to build a [zip lambda package](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html) for some functions. I don't think either .pth or .venv would wok.

---

_Comment by @charliermarsh on 2024-08-06 12:35_

I would consider that unsupported by the `uv sync` interface right now. It's a use-case we should probably support but it's just not something we've done yet for that part of the CLI.

---

_Comment by @paveldikov on 2024-08-06 13:42_

I am not sure if the `uv sync` interface is the right one here (or the workspace construct after all -- here we are dealing with a production installation, not a development environment)

That said, I sense a XY problem brewing here... so let me take a step back and define what I want to achieve (appears to be similar to @jpambrun-vida's use case after all?) instead of the 'how':

* I have a standard Python project
* I have built it into a standard Python distribution i.e. a wheel
* Downstream of that, I want to re-package this wheel into derivative distribution formats e.g. Docker image, RPM, deb, Nixpkg, ...
* I want to ensure that the dependencies that I am installing into this derivative distribution, do not contravene my lockfile (as opposed to being strictly equal to my lockfile)

---

_Comment by @charliermarsh on 2024-08-06 14:01_

I sense something like `uv build`.

---

_Comment by @paveldikov on 2024-08-06 14:31_

I would love that -- I own an equivalent tool in my org and I would love to eventually replace it with off-the-shelf uv commands.

(side note: I strenuously avoided the `build` term so as to avoid stepping on pypa/build's toes.. maybe unnecessary? I called it `install` out of deference to `make install` but `package` might be a good one as well)

---

_Comment by @adunmore on 2024-09-10 18:19_

> I just stubbled a real use case that I can't see any workaround for. As I am converting the current build system at work, I need to build a [zip lambda package](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html) for some functions. I don't think either .pth or .venv would wok.

Bumping this request. We have the exact same use case.

Currently getting around this limitation with
```
uv export -o requirements.txt --locked
uv pip install -r requirements.txt --target dist/
```

---

_Comment by @Levelleor on 2024-11-11 22:19_

> Bumping this request. We have the exact same use case.
> 
> Currently getting around this limitation with
> 
> ```
> uv export -o requirements.txt --locked
> uv pip install -r requirements.txt --target dist/
> ```

Same here, I found that PDM has a packing plugin to handle such cases: https://github.com/frostming/pdm-packer 

That "bundling" functionality would be great, or at least having the ability to sync into a target directory. Although, sync sounds wrong in this context, I feel like install suits better in this case as the intention is to perform a one-time download of packages into a target location.


---

_Comment by @itcarroll on 2025-07-12 02:19_

I'm not sure if `uv pip sync` originated after the comment above that `uv sync` may eventually discover and use an active environment (via `VIRTUAL_ENV`), but it seems like `uv pip sync` when not given a `<SRC_FILE>` argument could go off the `uv.lock` file.

---
