```yaml
number: 4028
title: "Allow users to install a project's dependencies, without the project itself"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2024-06-04T22:29:09Z
updated_at: 2025-04-21T06:43:16Z
url: https://github.com/astral-sh/uv/issues/4028
synced_at: 2026-01-10T03:41:46Z
```

# Allow users to install a project's dependencies, without the project itself

---

_Issue opened by @charliermarsh on 2024-06-04 22:29_

Not sure if we want this -- it's like Poetry's `--no-root`. Let's wait and see if we can find motivating use-cases for it. Poetry also has this concept of ["operating modes"](https://python-poetry.org/docs/basic-usage/#operating-modes) which toggle this automatically.


---

_Label `preview` added by @charliermarsh on 2024-06-04 22:29_

---

_Comment by @charliermarsh on 2024-06-04 22:32_

(Not decided, filed to revisit later.)

---

_Comment by @zanieb on 2024-06-04 22:51_

Lots of prior chatter over in https://github.com/python-poetry/poetry/issues/800

---

_Comment by @kdebrab on 2024-06-06 17:29_

Maybe see also https://github.com/pypa/pip/issues/11440 for potential use cases.

---

_Comment by @charliermarsh on 2024-06-06 17:46_

(Interestingly, we actually kind of already support what's described in that issue because we allow `pip install -r pyproject.toml`. But that's separate from this issue.)

---

_Comment by @kdebrab on 2024-06-19 10:30_

> (Interestingly, we actually kind of already support what's described in that issue because we allow `pip install -r pyproject.toml`. But that's separate from this issue.)

That doesn't support the [extras] syntax (e.g. `pip install -r pyproject.toml[doc]`)?

Personally, I'd love to see support for the syntax suggested [here](https://github.com/pypa/pip/issues/11440#issuecomment-1445119899):
```
pip install --only-deps  .[doc]  # project.dependencies and project.optional-dependencies.doc
```

[EDIT]
Just discovered that `pip install -r pyproject.toml --extra doc` gives the functionality I was looking for. That's really great!

---

_Comment by @firtrfl on 2024-06-26 09:33_

This works great `pip install -r pyproject.toml` :) It would be still great to add a flag and also add it to the pyproject.toml

```
[tool.uv]
package-mode = false
```

So it would also work with `uv add <package>`. Many things that I work on are not packages, but it would still be great to use uv to manage dependencies. 

PS: uv is such an amazing tool - thanks for building it ❤️ 

---

_Comment by @kdeldycke on 2024-07-13 10:23_

I have a use case for that: [my blog](https://github.com/kdeldycke/kevin-deldycke-blog), which is actually Markdown content only, and is built by [Pelican](https://getpelican.com), a pure Python static website generator.

In that case the `package-mode = false` from Poetry was handy.

In the mean time, I hacked the process and [added a dummy package at the root](https://github.com/kdeldycke/kevin-deldycke-blog/commit/7df59bfc9fd1eb00f8e116ad55beb5e19106b496) of my repository:
```shell-session
$ ls -lah ./blog
Permissions Size User Date Modified    Name
drwxr-xr-x     - kde  2024-07-13 14:12  .
drwxr-xr-x@    - kde  2024-07-13 14:17  ..
.rw-r--r--     0 kde  2024-07-13 14:11  __init__.py
```

And added the following directive in `pyproject.toml`:
```toml
[tool.setuptools.packages.find]
include = ["blog"]
```

---

_Comment by @Vigilans on 2024-07-21 14:22_

My use case: ansible project with bundled `ansible` and other related packages.

For ansible project, the majority of code are written in yaml, by providing `pyproject.toml` we can build a self-contained ansible environment in project's `.venv` folder, with guaranteed ansible modules versions. 

Besides, non-python-package programs like `sshpass` can be distributed into `.venv/bin`, so by `source .venv/bin/activate` ansible will be able to use functionally like remote host with password.

---

_Comment by @msw-kialo on 2024-07-26 13:25_

We use `--no-root` to better caching of container image layers.

The first layer just installs the project dependencies; the second adds the projects itself.
So if dependencies don't change, the layer can be reused.

We hope to write something like:

```Dockerfile

WORKDIR /app
ADD pyproject.toml uv.lock /app
RUN uv sync --no-dev --no-root
ADD src /app
RUN uv sync --no-dev
```

---

_Comment by @sbidoul on 2024-08-14 08:42_

In the above scenario of a layered Dockerfile to leverage caching of infrequently changing dependencies, it is important that there is a way to install from `uv.lock` without involving `pyproject.toml`. That is because when `pyproject.toml` gets involved, it may in turn require the presence of the whole project (because of an in-tree build backend, dynamic dependencies, etc).

At first sight, `uv.lock` contains everything that is needed to do this, except for an indication about the top level package(s).

IMHO this is a critical feature for adoption of `uv.lock`, as without it the space and time efficiency of dockerfile builds will be severely impacted, compared to the traditional approach of installing requirements.txt in a layer, then the app in another.


---

_Comment by @charliermarsh on 2024-08-14 13:14_

I agree that something like this is necessary.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @adriangb on 2024-08-21 12:29_

I'll chime in that `--no-root` is not enough. You probably also want a `--no-directory` or `--no-local`, maybe just generalize to a source selector or something.

Motivation: https://python-poetry.org/docs/faq#poetry-busts-my-docker-cache-because-it-requires-me-to-copy-my-source-files-in-before-installing-3rd-party-dependencies

In particular the idea is that if you have a monorepo you can still copy in your top-level lockfile and not have to copy in _anything_ (not even the `pyproject.toml`) from your sub-projects/packages.

---

_Comment by @charliermarsh on 2024-08-21 12:35_

Yeah 100% agree that you should be able to sync from just the lockfile. The lockfile itself is actually designed to support that (it doesn’t rely on pyproject.toml), but we need to expose the right options to users…

---

_Comment by @charliermarsh on 2024-08-21 13:01_

I can see a few approaches here (all fairly easy to implement, mostly about getting the API right)...

- `--no-project` (or `--no-workspace`) along with a separate `--no-local` flag (to ignore path dependencies that aren't part of the workspace)
- `--only-remote` (the inverse)
- Something like `--seed` or another semantic term that indicates why this is useful


---

_Comment by @hynek on 2024-08-21 13:15_

In case you’re looking for opinions, I like “seed”. The others are rather generic and I’d have to guess what they exactly mean. 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-21 13:28_

---

_Comment by @charliermarsh on 2024-08-21 13:28_

(Gonna see if we can come up with an API and ship this ~today.)

---

_Comment by @charliermarsh on 2024-08-21 13:29_

Opinions very welcome.

---

_Comment by @adriangb on 2024-08-21 14:18_

I think `--no-project` is pretty clear but don't care too much as well as it's documented.

---

_Comment by @baggiponte on 2024-08-21 14:30_

Would `--no-self` and `--only-self` be more intuitive?

---

_Comment by @sbidoul on 2024-08-21 14:43_

`--seed {package-name}[extra,...]`, because you need to tell it what top level package's dependencies you want to install?

---

_Comment by @zanieb on 2024-08-21 20:56_

Some thoughts....

I think extras would be covered by the normal `--extra` and `--all-extras` options.

I worry about `--no-project` and `--no-workspace` because those have a different meaning elsewhere; which is, don't discover a project or workspace entirely. We still should perform normal project or workspace discovery, e.g., to find the `uv.lock` file if you're in a subdirectory for some reason.

I think we might want to use `--without-` and `--only-` prefixes, to match the our `--with` options.

The difficulty is in choosing how to describe what we're selecting. There are a few things we want to be able exclude:

- The current project
- A workspace member
- A path dependency 

Unfortunately `--without-editables` doesn't work because a path dependency can be either editable or not editable. We also want to be able to toggle whether or not the current project and workspace members are installed as editable in the future.

All of these are "local" dependencies, as in they're on the local file system, so `--without-local` might make sense. Does this include the current project though? Do we need an inverse flag, like `--only-remote`? Is that easier to reason about?

It feels like we need a separate flag for excluding _just_ the current project, like `--without-self` or `--without-root` (I think `--without-project` isn't an option because it overlaps too much with `--no-project`). Presumably we'd pair this with an inverse flag like `--only-self` or `--only-root`. 

Do we need the ability to exclude all local dependencies except the current project? Or can it be done in two steps:

```
uv sync --without-local
uv sync --inexact --only-self
```

What's the use-case for that?



---

_Comment by @charliermarsh on 2024-08-21 21:00_

I think the only use-case for the Docker caching situation is "exclude all locals".

I think excluding the _current project_ (but not other workspace members) is independently useful, like for "virtual projects." We could choose not to solve that here, but we should probably have a design that won't cause us problems in the future if we pursue it.


---

_Comment by @zanieb on 2024-08-21 21:02_

As a note on my own post, I think I prefer `--without-locals` over `--without-local`?

---

_Comment by @charliermarsh on 2024-08-21 21:36_

What about `--no-locals` or `--no-local` to match options like `--no-project`?

---

_Comment by @charliermarsh on 2024-08-22 01:50_

Ok, I didn't get this shipped today, but I did implement it: https://github.com/astral-sh/uv/pull/6398

---

_Comment by @charliermarsh on 2024-08-22 01:50_

The gist is that you can copy in `uv.lock`, then run `uv sync --frozen --no-locals`.

---

_Comment by @zanieb on 2024-08-22 03:53_

Well, I specifically opted for `--without-` instead of `--no-` since we use `--with-` for including dependencies it probably makes sense to do the same for excluding them?

---

_Comment by @hynek on 2024-08-22 03:53_

I would be remiss if I didn't mention that this feature would be massively more useful (and less surprising, I guess) in the Docker context if `uv sync --python /some/venv/bin/python` worked as expected. 😇

But mega kudos for the quick implementation as always; this is amazing! 🚀

---

_Comment by @zanieb on 2024-08-22 03:54_

We'll definitely be tackling alternative sync targets ASAP as well.

---

_Comment by @mitsuhiko on 2024-08-22 07:47_

I think the docker case is real but at the end of the day it creates a situation in the virtualenv that I would argue should not exist.  So at least in my mind there is a significant difference between "install third party deps", make a layer, then follow by installing the rest vs "never ever install the root package because it's inconvenient".

Particularly for the docker situation I quite honestly wonder if not a fundamentally better solution is to allow the installation process to be explicitly being built for that use case by supporting skipping/interrupting and continuing.  I understand that this is not how any other tool works but particularly for more complicated setups I can see this being quite useful.

I'm imagining that this might be quite interesting.  This would work if the lockfile is up to date prior to the sync.

```dockerfile
# first layer, third party deps only
RUN uv sync --partial --skip-package="root-package" --skip-package="second-package"
COPY src/root-package ./src/root-package
# second layer, bring one package in
RUN uv sync --partial --skip-package="second-package"
COPY src/second-package ./src/second-package
# sync what we have not seen yet
RUN uv sync
```

At sentry for instance we do install packages from two different repositories (open source git checkout followed by internal billing code checkout).

I think being more explicit about what this is used for would be a good thing and also forces that the docker case is intentionally supported on the `uv` side and not just a happy little accident.

This also leaves the option open to yell at the user if the virtualenv is being utilized and does not actually reflect a full sync.

---

_Comment by @hynek on 2024-08-22 08:12_

Your example doesn't include cache busting by copying the uv.lock file into the build container which is kind the whole point? You want the layer to be rebuilt if the lock file changes. but keep it around otherwise.

I suspect your example is incomplete on purpose to just sketch out the idea, but I think it would be good to complete it, because those `COPY`s and `cd`s are kinda elemental to the whole thing.

I for one could live with explicitly naming the package I want to sync later (i.e. `uv sync --skip-package="my-app"`) but I'm not 100% sure we're on the same page.

---

_Comment by @paveldikov on 2024-08-22 08:22_

My use case here is very similar to the ones listed here, but with a twist. We do not _copy_ the workspace into the build context, rather, we _mount_ a wheel that was already built in a previous CI stage.

This reduces duplication, both in terms of stages (build-backend only invoked once), as well as storage (if I `COPY src` and then proceed to `uv sync` that, I'd have an `src` tree left over in my resulting image, taking space and causing confusion).

Most importantly, by using a wheel and not a raw workspace, this also lets me have consistent builds between different targets (docker, pypi, rpm/deb/nix, etc etc)

Being able to `uv sync` without the root package will help to an extent, but I am not sure if it is a globally-optimal solution. I'd end up with:
```
RUN uv sync --frozen --no-locals
RUN uv pip install /mnt/my-actual-application.whl       # This part is lockfile agnostic; no guarantee that things won't drift!
```

For context, right now we do:
```
RUN uv pip install /mnt/my-actual-application.whl -c /mnt/constraints.txt
```
Which works exactly as intended -- but it's not a proper lockfile (and being able to slice it into layers is a very nice thing to be able to do)


---

_Comment by @mitsuhiko on 2024-08-22 08:23_

> Your example doesn't include cache busting by copying the uv.lock file into the build container which is kind the whole point? You want the layer to be rebuilt if the lock file changes. but keep it around otherwise.

Yes, the lock file needs to be copied first, but only prior to the first partial sync and if it's not there, `uv sync --partial` would fail.

My point is mostly, without going too much into the implementation, that a partial sync would be an explicit, multi-stage approach precisely to safely make a multi-layer caching solution work.  The benefit for the user is that it can be guided without the user having too many ways to unintentionally fuck it up or changes in uv breaking layer caching accidentally.

> I for one could live with explicitly naming the package I want to sync later (i.e. uv sync --skip-package="my-app") but I'm not 100% sure we're on the same page.

My reasoning with explicit skipping here is not so much that this is how it should be (eg: this is how you name what has not been synced) but that it's a very intentional process and you don't accidentally start interacting with a partial venv and then end up surprised about random failures later down the line.

---

_Comment by @charliermarsh on 2024-08-22 12:54_

I think it would be pretty reasonable to use `--skip-package` rather than `--no-locals`. It would enable strictly more use-cases. The downside is that you need to include the package name (of course) and that, if you have a large workspace, it's a little tedious to enumerate them all. But it might be a better, more flexible solution than `--no-locals`.

---

_Comment by @sbidoul on 2024-08-22 13:20_

Does the lock file include dev dependencies? In that case it would be very tedious if we don't want these in the docker image.

How about `--dependencies-of package` ?

---

_Comment by @charliermarsh on 2024-08-22 13:23_

Yeah it does include dev dependencies. But you can omit those with `--no-dev`.

---

_Comment by @sbidoul on 2024-08-22 13:26_

Ah, right I now see the lock file knows the distinction between regular and dev dependencies :+1: 

---

_Comment by @zanieb on 2024-08-22 16:41_

Might be worth considering https://github.com/astral-sh/uv/issues/4422 too when designing a `--skip-package` interface.

---

_Comment by @bluss on 2024-08-22 17:51_

What stands out to me is that uv sync is not a command designed for application install or deployment. It sets up the venv and workspace (for development/test?) and all the defaults point towards that use case (?)

---

_Comment by @charliermarsh on 2024-08-23 02:14_

Today we discussed doing something like:

- `--no-install-root` (skip root package)
- `--no-install-workspace` (skip any `workspace` dependencies, including the root)
- `--no-install-paths` (skip any `path` dependencies)
- `--no-install-package [package]` (skip a specific package)

So most usages would just be `--no-install-root` or `--no-install-workspace`.

We may even want to have a dedicated separate command for this with different defaults (per the above).


---

_Comment by @zanieb on 2024-08-23 20:40_

This will be available when we release (soon, I'm sure). Let me know if you encounter any problems!

---

_Closed by @zanieb on 2024-08-23 20:40_

---

_Comment by @zanieb on 2024-08-24 00:41_

This is available now in 0.3.3

---

_Comment by @justenstall on 2024-08-27 15:44_

Is `--no-install-paths` still planned?

---

_Comment by @zanieb on 2024-08-27 15:51_

@justenstall we're waiting to see how necessary it is since we have `--no-install-package` and the amount of options could get excessive. We can track in https://github.com/astral-sh/uv/issues/6695

---

_Comment by @ddorian on 2024-08-28 07:02_

Can `--no-install-project` be set in config?

---

_Comment by @zanieb on 2024-08-28 12:48_

No, it's not intended for long-term usage. You're probably looking for #6585 instead (which will release today).

---

_Comment by @VigneshVSV on 2025-04-08 18:04_

For those who have specified a build-backend and wish to avoid building and installing the project for which you have created the `.venv` with `uv` , do:

`uv sync --no-install-project`

Thanks to uv community for this project.

---

_Comment by @Quang-elec44 on 2025-04-21 03:47_

@VigneshVSV Hi, can you help me? I want to add a library (e.g., requests), but when I ran `uv add requests`, it also built my package and installed it into the environment. How can I avoid it? Do you have any better idea?

---

_Comment by @VigneshVSV on 2025-04-21 06:42_

> [@VigneshVSV](https://github.com/VigneshVSV) Hi, can you help me? I want to add a library (e.g., requests), but when I ran `uv add requests`, it also built my package and installed it into the environment. How can I avoid it? Do you have any better idea?

@Quang-elec44, Unfortunately, I could not find a quick answer. You could comment out the build section in your `pyproject.toml`, or add your dependencies as UV is currently behaving and delete and recreate the environment using `sync --no-install-project`.

```toml
# just comment it out
# [build-system]
# requires = ["setuptools>=42", "wheel"]
# build-backend = "setuptools.build_meta"
```

There are `--no-build` options, however this only skips the build process and assumes that a binary exists. 

---
