---
number: 3163
title: "Support `pip download`"
type: issue
state: open
author: idlsoft
labels:
  - compatibility
  - cli
assignees: []
created_at: 2024-04-20T16:09:23Z
updated_at: 2025-12-16T23:49:27Z
url: https://github.com/astral-sh/uv/issues/3163
synced_at: 2026-01-07T13:12:17-06:00
---

# Support `pip download`

---

_Issue opened by @idlsoft on 2024-04-20 16:09_

This would be especially useful for buildng docker images.

You could then rely on `uv` for a quick resolve and use a simple `pip install --no-deps --find-links` in your `Dockerfile`.


---

_Comment by @samypr100 on 2024-04-23 16:12_

This is along the similar lines of supporting [pip wheel](https://pip.pypa.io/en/stable/cli/pip_wheel/) discussed in https://github.com/astral-sh/uv/issues/1681



---

_Label `compatibility` added by @zanieb on 2024-04-23 17:16_

---

_Label `cli` added by @zanieb on 2024-04-23 17:16_

---

_Comment by @inoa-jboliveira on 2024-05-09 15:31_

Hi everyone, is this feature on the roadmap? I am guessing supporting "pip download" would be straightforward since uv already downloads packages. It would just have to not install them. 

Any help needed?

---

_Comment by @charliermarsh on 2024-05-09 16:28_

We should be able to support it... Though it's not trivial because we don't store the `.whl` files at all, we unzip them directly into the cache. So most of the data pipelines are oriented around an API that receives the unzipped wheel, rather than the zipped wheel.

What are the typical use-cases here?

---

_Comment by @idlsoft on 2024-05-09 17:02_

> What are the typical use-cases here?

In my case it's a docker build in a github workflow.

Caching docker layers on github runners is impossible AFAIK. Caching `~/.cache` is trivial.
So a build could download whls into the docker working dir, then do
```
COPY whls whls
RUN pip install --no-deps --find-links whls ....
```
Which wouldn't hit pypi and wouldn't need any additional caching from docker.

... This can be even better if you can do `RUN --mount ...` 



---

_Comment by @charliermarsh on 2024-05-09 17:03_

I'm mostly wondering if it _has_ to be wheels or if we could just make it easy to pre-populate the uv cache.

---

_Comment by @idlsoft on 2024-05-09 17:08_

> I'm mostly wondering if it _has_ to be wheels or if we could just make it easy to pre-populate the uv cache.

wheels are supported by standard `pip install`.
Otherwise you need to use `uv` inside docker build. Not bad necessarily, but a bit less flexible.

---

_Comment by @zanieb on 2024-05-09 17:10_

I'm not sure how much we should go out of our way to support using pip to consume an output of uv? It seems weird to use uv in one case and pip in another, right?

---

_Comment by @charliermarsh on 2024-05-09 17:11_

If it were equally easy for us I'd probably _prefer_ to output wheels, it's a nicer intermediary format that's less coupled to our internal cache. I'd need to see how hard it is to support.

---

_Comment by @inoa-jboliveira on 2024-05-09 17:16_

> What are the typical use-cases here?

Hi, my use case is that I have to supply bundles of my application with all dependencies for systems where it is not possible to download them (firewall blocking). Right now I use pip download which results in a bunch of wheel files.

Also we would like to be able to cross platform download them too

---

_Comment by @charliermarsh on 2024-05-09 17:24_

That makes sense, thanks.

---

_Comment by @idlsoft on 2024-05-09 17:30_

> I'm not sure how much we should go out of our way to support using pip to consume an output of uv? It seems weird to use uv in one case and pip in another, right?

That part of the workflow may not be entirely under your control.
@inoa-jboliveira's example is a better one I think, because it's essentialy about packaging your application.
Packaging may need to comply with a specific post-install procedure. 

---

_Comment by @samypr100 on 2024-05-10 01:54_

> Right now I use pip download which results in a bunch of wheel files.

Note, both `pip download` and `pip wheel` are similar but there's crucial differences. If you're looking to package up your wheels, `pip wheel` is often recommended instead since it covers for cases where a download does not have a pre-built wheel, being a more complete solution to pre-packaging **wheels** for a target system.

As a result, I tend to just always use `pip wheel` nowadays to make sure I always have wheels rather than potential source distributions that I'll have to build on the target system. From my perspective, `pip download` is more useful when you want to package up sdists or when you don't care if everything you download is fully pre-built.

Some of these tradeoffs were actually discussed in #1681.

---

_Comment by @inoa-jboliveira on 2024-05-10 18:27_

Hi @samypr100 in this specific case I really just want to download pre built wheels and not build anything. I can `pip download --platform foo` and I am good to go. That's why I need and still use pip download.

As for pip wheel, I can't cross platform download (nor compile) anything

---

_Comment by @pickfire on 2024-06-14 03:53_

`pip download -d vendor/ --index-url internal_pypi internal-sdk` can be used with `uv pip install -f vendor` if we need to vendor anything.

---

_Referenced in [astral-sh/uv#4809](../../astral-sh/uv/issues/4809.md) on 2024-07-04 15:26_

---

_Referenced in [astral-sh/uv#2078](../../astral-sh/uv/issues/2078.md) on 2024-07-04 15:27_

---

_Comment by @jbw-vtl on 2024-07-15 18:57_

Also quite interested in this as `pip download` is a slower part in our CI environment.

In our case our security scanning tool requires to run on a folder of wheel / source distributions, we currently use pip download to gather these.

---

_Referenced in [astral-sh/uv#5100](../../astral-sh/uv/issues/5100.md) on 2024-07-16 12:17_

---

_Comment by @cbrnr on 2024-07-26 10:42_

Another use case would be downloading build time dependencies (in addition to runtime dependencies). I'm not sure if this is feasible, since it is not supported by pip (https://github.com/pypa/pip/issues/7863). However, this would be extremely useful when building a Flatpak which involves Python packages, which is currently broken because of that (https://github.com/flatpak/flatpak-builder-tools/issues/380).

---

_Renamed from "Support pip download" to "Support `pip download`" by @zanieb on 2024-08-11 14:49_

---

_Referenced in [astral-sh/uv#6006](../../astral-sh/uv/issues/6006.md) on 2024-08-11 14:49_

---

_Comment by @ei-grad on 2024-08-13 09:03_

Although `pip download` is very handy for multi-stage Docker builds to efficiently cache dependencies,

> `uv` doesn’t store the .whl files; instead, it unzips them directly into the cache.

It would be even better if the `uv` cache could be used to install requirements directly into the target stage instead of installing from the wheels. I'm curious whether there's a straightforward method to populate the `uv` cache for a list of packages.

---

_Referenced in [astral-sh/uv#6323](../../astral-sh/uv/issues/6323.md) on 2024-08-21 23:50_

---

_Comment by @RichardDally on 2024-08-29 15:34_

We have a business use case to scan dependencies of a Python project, we need to pip download requirements, it's slow without uv 😒

---

_Comment by @notatallshaw on 2024-08-29 17:54_

> We have a business use case to scan dependencies of a Python project, we need to pip download requirements, it's slow without uv 😒

Not saying you shouldn't use uv, but do you have an example where pip 24.2 is slow at downloading?

Especially if you've already pre-resolved the requirements with uv pip compile, as hopefully the biggest bottleneck is IO. I should be able to profile and see if there's any low hanging fruit in pip that can be fixed.

Also should be a good scenario to see if uv can advertise being faster here or not.

---

_Comment by @inoa-jboliveira on 2024-08-29 18:13_

pip download is pretty ok/fast enough for **my** needs. I do also open multiple processes and am constrained only by network so UV won't be any faster without cache (I'm not the guy above).

It is necessary for 2 reasons:
 - fully replace pip and not need it as a dependency
 - it is likely 99% done already, just missing the interface or some "do not unzip" flag to the actual data that is cached. Maybe a re-zip cached wheels for not redownloading them. 

By using cache it would indeed be faster than pip for same platform downloads. Although, for my use case, I need to download cross platform, so the packages won't be there (e.g. numpy or pandas which are large plataform specific packages).



---

_Referenced in [astral-sh/uv#7296](../../astral-sh/uv/issues/7296.md) on 2024-09-12 09:22_

---

_Comment by @lmmx on 2024-09-12 09:33_

> What are the typical use-cases here?

At the risk of repeating what other people have said, to chime in with my use case (also Docker image building for deployment, reproducibility is a secondary concern for me) and perhaps shed light on why pip download is important enough to support:

- In a Docker build context, there used to be (i.e. outdated practice) a flag to pip install `—global-option=build_ext` which would trigger package build from source
- Now if you want to do that `build_ext` has been deprecated* for an explicit build step, so to build from source you would first run pip download to download that source** followed by building that and uv build takes these downloaded wheel
  - *because it kind of was considered out of scope for install commands to be also building, and presumably with the intro of the build backend toml format
  - **or get it from a repo’s release files, but via pip was the standard way
- So the pseudocode workflow used to be “`pip install —build-and-install my-package` and now it’s `pip download my-package`, `python setup.py build my-package my-wheel`, `pip install-my-wheel`”.
- With `uv` we’d perhaps be able to something more integrated (`uv download+build+install my-package`), maybe with helpful optimisations like caching

Also I’d note that as a user of these toolsets it can be confusing to keep up with the proliferation of ways to do the one thing as the state of the art evolves

I found an issue RE: `pip wheel` when looking for this thread, and it notes that “pip would prefer to deprecate pip wheel” ([#1681](https://github.com/astral-sh/uv/issues/1681#issuecomment-1952952263))

---

_Comment by @daler-rahimov on 2024-09-13 14:30_

Hello everyone, I wanted to contribute some additional use cases for consideration. While most discussions here focus on the cloud, my perspective comes from the embedded world. Let’s consider the 10 billion devices currently operating on cellular networks, of which 1.8 billion are IoT/M2M devices. Many of these devices do not have access to "unlimited good bandwidth" but still require software updates, CI, etc. 

When using Python, one way to accelerate these deployments is by pre-fetch PyPI packages ahead of time. This is not about supporting `pip download` but rather working with cached downloads. For example, you could use a method like this in a pre-fetch for big packages like TensorFlow (~400MB):

```bash
file_url="https://files.pythonhosted.org/packages/5e/31/d49a3dff9c4ca6e6c09c2c5fea95f58cf59cc3cd4f0d557069c7dccd6f57/tensorflow-2.7.4-cp39-cp39-manylinux2010_x86_64.whl"
wget --continue --quiet -P . "$file_url"
```
And then the actual software deployment could use this pre-fetched file and get the rest of the dependencies from the PyPI directly. 

By enabling the UV package manager to use these pre-fetched/cached files, deployments become more efficient. Also, it's intuitive for a user to think about UV operations as downloading, storing, and installing. 

---

_Comment by @notatallshaw on 2024-09-13 15:29_

> By enabling the UV package manager to use these pre-fetched/cached files, deployments become more efficient. Also, it's intuitive for a user to think about UV operations as downloading, storing, and installing.

I don't think this is the same issue? And should already be possible.

If you have acquired the wheels you can install directly from them (or use `--find-links`):

```
$ pip download requests --no-deps
Collecting requests
  Using cached requests-2.32.3-py3-none-any.whl.metadata (4.6 kB)
Downloading requests-2.32.3-py3-none-any.whl (64 kB)
Saved ./requests-2.32.3-py3-none-any.whl

$ uv pip install ./requests-2.32.3-py3-none-any.whl
Resolved 5 packages in 176ms
Prepared 5 packages in 120ms
Installed 5 packages in 37ms
 + certifi==2024.8.30
 + charset-normalizer==3.3.2
 + idna==3.8
 + requests==2.32.3 (from file:///home/dshaw/uvtest/3163/requests-2.32.3-py3-none-any.whl)
 + urllib3==2.2.3
```

Also, pretty sure you can install from uv on your base machine, copy the cache to other devices, and then point uv cache to the copied directory, this should use even less resources (CPU and storage) on your IoT devices, as there's no extra step of unzipping the contents and storing it somewhere.

---

_Comment by @daler-rahimov on 2024-09-13 18:40_

> I don't think this is the same issue? And should already be possible.

Unfortunately it's not possible. I have better explanation here if you are interested #7296.  

> $ uv pip install ./requests-2.32.3-py3-none-any.whl

Installing a single package isn't the goal here but managing all the dependencies with uv. Basically doing something like this `uv 
sync --find-links [path-to-some-pre-fetched-wheels]`    

>  copy the cache to other devices, and then point uv cache to the copied directory,

In this use case, the biggest problem is data usage (for some devices, you pay per MB of usage), and the UV's cache contains the unzipped versions of wheels. For example, TensorFlow, which is ~400MB, expands into GB of data  

---

_Referenced in [pex-tool/pex#2512](../../pex-tool/pex/pulls/2512.md) on 2024-09-13 19:25_

---

_Referenced in [pex-tool/pex#2371](../../pex-tool/pex/issues/2371.md) on 2024-09-13 19:31_

---

_Comment by @notatallshaw on 2024-09-13 23:20_

> In this use case, the biggest problem is data usage (for some devices, you pay per MB of usage), and the UV's cache contains the unzipped versions of wheels. For example, TensorFlow, which is ~400MB, expands into GB of data

To copy onto the device before it does any downloading or the total amount of storage on the device?

Because if it's the total amount of storage on the device then you will use less space by copying the cache, because copying the wheel will take up the wheel + the install, whereas copying the cache will just be the install, and the the site-packages location will just hard link to the cache and use no additional space.

If it's to initially copy onto could zip the uv cache up and then have a small script that unzips it into the actual uv cache folder and then delete the zip.

I'm not saying it wouldn't be helpful for uv to have a download function and what you propose in https://github.com/astral-sh/uv/issues/7296, just spitballing solutions with existings tools. 


---

_Comment by @T-256 on 2024-11-17 19:57_

According to prior discussion, instead of having two `uv pip download` and `uv pip wheel`, I think we can have one top-level command, let's call it `uv collect` for now. it then will be able to collect all files of dependencies by using resolved dependency of current lockfile.
```shell
$ # auto build sdists.
$ uv collect

$ # prevent include prebuilt wheels.
$ uv collect --requires-build-only

$ # cross-platform lockfile resolving.
$ uv collect --python-platform windows

$ # avoid build sdists, but clone them as they are (for sdist wheels copy `tar.gz` file,
$ # for git dependency clone repo/subdirectory, for editables copy folder).
$ uv collect --clone

# by default, it collects all indexes, we can limit it.
$ uv collect --exclude-index pypi  # collect all others except `pypi`
$ uv collect --index internal_pypi  # only `internal_pypi`
```
I also think it could be integrated to some part of uv's caching system, e.g. `built-wheels` caches.

I hope it help to this issue and #1681 to going forward.

---

_Referenced in [astral-sh/uv#1681](../../astral-sh/uv/issues/1681.md) on 2024-11-17 20:55_

---

_Referenced in [astral-sh/uv#9345](../../astral-sh/uv/issues/9345.md) on 2024-11-22 05:11_

---

_Referenced in [astral-sh/uv#9346](../../astral-sh/uv/issues/9346.md) on 2024-11-23 03:07_

---

_Referenced in [astral-sh/uv#10460](../../astral-sh/uv/issues/10460.md) on 2025-01-10 13:33_

---

_Comment by @astafan8 on 2025-02-17 14:24_

I am happy to take a stab at implementing `pip download` as a way for me to learn Rust. My hope is that the functionality of download is to a large extent already implemented as part of `uv pip install`, so it seems to be a great case of figuring out how to compose a feature from existing implementations, add new CLI command, write tests. However, for that to happen, I would need guidance of where to start and how to proceed in steps, e.g. start in file foo.rs, use function Bar from it, and also use function Baz from bleh.rs as a reference to steps that need to be done to implement the feature. All other blanks I can fill in from your contributors guide, and with some questions on github.

What do you think? ping @charliermarsh 

---

_Comment by @zanieb on 2025-02-17 15:26_

A problem with this it the overlap with `pip wheel`, e.g., see https://github.com/astral-sh/uv/issues/1681#issuecomment-1952952263

It's sort of an open design question how we want to build a better unified interface here. Additionally, our cache is pretty complicated. For those reasons, I'm not sure if it's a great candidate for a first-time contribution.

---

_Referenced in [astral-sh/uv#12009](../../astral-sh/uv/issues/12009.md) on 2025-03-06 15:37_

---

_Comment by @waynew on 2025-03-24 16:35_

Until there's a proper `uv` interface/approach for this, `uvx pip download <pkg>` works as a stopgap for me (especially since I typically work on systems that don't like `ensurepip` or `pip --user`)

---

_Comment by @sohang3112 on 2025-04-09 18:02_

> I'm not sure how much we should go out of our way to support using pip to consume an output of uv? It seems weird to use uv in one case and pip in another, right?

@zanieb Hi. My use case is that I use `uv` to build my project, but occassionally I want to install something like `ipython` that isn't really related to the project so I install it with `pip` to avoid adding it to pyproject.toml . (OTOH I use `uv add --dev mypy` because I use mypy in pre commit & CI).

```console
$ uv run pip install ipython
```

This errors because venv created by `uv lock & uv sync` doesn't have pip installed. So right now I need to manually do `uv run python -m ensurepip` to install pip in .venv/, and then run pip install again. But that's unintuituve & most people dont know about this command.

---

_Comment by @zeevro on 2025-04-10 10:54_

@sohang3112 I don't really see how your problem is related to this issue.
You can do what you want in two ways:
1. `uv pip install` will install packages in your venv without checking or updating your `pyproject.toml` or `uv.lock`.
2. `uv run --with ipython ipython` will allow you to use IPython in your venv without installing it.

---

_Comment by @sohang3112 on 2025-04-10 13:07_

Thanks @zeevro - I did not know about these commands 👍.

---

_Comment by @jc00ke on 2025-04-18 21:23_

> Hi, my use case is that I have to supply bundles of my application with all dependencies for systems where it is not possible to download them (firewall blocking).

I'm in the same boat here. Developer machines are on a network that does not allow access. I'd love to see a simple way to vendor deps in a local folder.

---

_Referenced in [astral-sh/uv#13263](../../astral-sh/uv/issues/13263.md) on 2025-05-02 10:44_

---

_Comment by @gsemet on 2025-05-02 11:00_

The command that i would need for creating zipapp using shiv is to replace:
```
$ {{ uv }} run pip install --extra-index=https://my/interna/pypi/repository/pypi/simple -r dist/prod-requirements.txt --target distshiv/
```

shiv actually packages a "site-packages" folder


---

_Referenced in [astral-sh/uv#13304](../../astral-sh/uv/issues/13304.md) on 2025-05-05 16:24_

---

_Referenced in [pex-tool/pex#2790](../../pex-tool/pex/issues/2790.md) on 2025-06-25 22:53_

---

_Comment by @matthias-busch on 2025-06-30 11:04_

Any progress/news on this or https://github.com/astral-sh/uv/issues/1681?

---

_Referenced in [astral-sh/uv#14994](../../astral-sh/uv/issues/14994.md) on 2025-07-31 13:42_

---

_Comment by @RichardDally on 2025-08-22 15:22_

Would be useful for us too. Currently, downloading wheels slowly with pip for security scan purposes.

---

_Comment by @vadimkantorov on 2025-09-04 09:27_

Also, would be nice if `uv` could also directly consume a wheel cache during a sync, without creating a second, intermediate uv-styled cache. This is useful when keeping the wheel cache at a slow networked drive, where it's important to not have thousands of files, and then syncing/extracting from it directly without any other extra caches

---

_Comment by @ei-grad on 2025-09-04 12:24_

> Also, would be nice if `uv` could also directly consume a wheel cache during a sync, without creating a second, intermediate uv-styled cache. This is useful when keeping the wheel cache at a slow networked drive, where it's important to not have thousands of files, and then syncing/extracting from it directly without any other extra caches

What will be the point of using uv then?


---

_Referenced in [aspect-build/rules_py#544](../../aspect-build/rules_py/pulls/544.md) on 2025-09-22 03:31_

---

_Referenced in [astral-sh/uv#16016](../../astral-sh/uv/issues/16016.md) on 2025-09-24 14:27_

---

_Comment by @sterliakov on 2025-10-19 01:49_

I found myself looking for `uv pip download` (and even assuming it exists, crashing first CI run attempt) for a weird reason: I need to download sdist of a package to unpack and `uv build` it into a wheel afterwards. Now I just `uv pip install pip && pip download...`, but that hurts aesthetically! This won't be solved by only allowing wheels downloads.

---

_Comment by @zeevro on 2025-10-19 06:40_

@sterliakov I do this in CI. It's ugly, but it does the job.
```sh
uvx --with distlib python <<EOF
from distlib.index import PackageIndex
from distlib.locators import locate
dist = locate('python-poppler-qt5')
fn = dist.source_url.rsplit('/', 1)[-1]
PackageIndex(dist.locator.base_url).download_file(dist.source_url, fn, dist.digest)
print(f'SDIST_FILENAME={fn}')
EOF
```

---

_Referenced in [astral-sh/uv#16410](../../astral-sh/uv/issues/16410.md) on 2025-10-22 14:55_

---

_Comment by @patstrom on 2025-11-14 13:37_

We have a situation where we need to package the application so it can be installed completely offline with `pip install --no-index --find-links`.

What we're doing now is the following (simplified)

```
echo '--extra-index-url <private-repo>' > package_directory/requirements.txt

uv run export --no-hashes --locked --no-default-groups --no-emit-project --format=requirements.txt > package_directory/requirements.txt

uv run --with pip python -m pip download --disable-pip-version-check --platform $platform --no-deps -r package_dir/requirements.txt -d package_dir/wheels

rm package_dir/requirements.txt

uv build --all-packages --wheel -o package_dir/wheels
```

And then later on when we've moved package_dir to the installation environment (which doesn't have uv) we run

```
pip install --no-index --find-links package_dir/wheels application-name
```

Having uv support this workflow so that we didn't have to neither manually add the --extra-index-url as well as skip the whole `uv export` step would be great.

---

_Comment by @gsemet on 2025-11-14 15:08_

that is problematic in a sense because your wheelhouse is only compatible for your `$platform`, the export might have captured the extra/platform specific parameters, but i am pretty sure your pip download is platform-interpreter specific. that is a pbl because you might want to build a true, multiplatform wheelhouse (mac, win, linux, intel, mac silicon, py32, py313, p314,...). i would love to have the ability to build a truely multiplatform auto-instlalable wheel (even if it means packaging tons of unused wheels in my deliverable package).

---

_Comment by @notatallshaw on 2025-11-14 15:14_

@gsemet I've not used it, but my understanding is `pex download` supports multi-platform downloads, you can probably uv export to a pylock file and use pex to download that.

---

_Comment by @ketozhang on 2025-12-16 23:45_

There is currently no tool that supports downloading wheels/sdist from pylock.toml (PEP 735).
This makes the standard not very useful for internet-less private index for deploying to local-only environments.

uv cache isn't a good solution right now, we cannot host cache servers (without being hacky), the same way we can host private index servers. A cache solution pushes uv further away from standard and becoming their own packaging ecosystem like conda.

The hack I am currently using is unfavorable as we go back to using `requirements.txt`:

```sh
# Onboarding process for bringing in internet packages into a private pypi server 
# for downstream deploys of internet-less environments
uv venv --seed  
uv pip sync pylock.toml
uv run pip freeze > requirements.txt
uv run pip download -r requirements.txt --dest wheelhouse/
uv publish --index myprivateindex wheelhouse/*
```

---

_Referenced in [openbraininstitute/neuroagent#645](../../openbraininstitute/neuroagent/pulls/645.md) on 2025-12-26 11:36_

---
