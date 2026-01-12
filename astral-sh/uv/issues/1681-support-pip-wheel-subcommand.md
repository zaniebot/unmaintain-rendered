```yaml
number: 1681
title: "Support `pip wheel` subcommand"
type: issue
state: open
author: kvelicka
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-19T08:42:57Z
updated_at: 2025-11-07T11:57:14Z
url: https://github.com/astral-sh/uv/issues/1681
synced_at: 2026-01-12T15:58:31Z
```

# Support `pip wheel` subcommand

---

_@kvelicka_

I've been trying `uv` on work projects to assess its coverage and suitability of being a full `pip`/`pip-tools` replacement for us. One of the features of pip that we use is `pip wheel`, which seems to not be supported by `uv` currently and I couln't find an open ticket discussing it, so I'm making one here.

---

_Comment by @zanieb on 2024-02-19 17:42_

We have all the plumbing to do this, I'm not sure where it fits on our roadmap though.

I'm curious how this would relate to 

- #1510 

---

_Comment by @henryiii on 2024-02-19 17:51_

Pip would prefer to remove the `wheel` subcommand AFIAK, and has refused adding a matching `sdist` command. I think `uv build --wheel` (along with `uv build --sdist` and `uv build`, which builds both) would be better than adding `uv pip wheel`.

`pip wheel` does one thing differently, by the way - it includes any wheels it also _builds_ (but not just downloads), while `build --wheel` only gives you the wheel you asked for. But most of the time that's actually a bug (trying to upload wheels you don't own), and `pip download` is there if you want to actually get existing files from PyPI (not in uv yet though).

---

_Comment by @zanieb on 2024-02-19 18:16_

Thanks for the additional context. I wonder what we can provide for people who want to generate wheels for all their dependencies though. It makes sense for pre-building and sharing when they aren't distributed by the original package.

---

_Label `enhancement` added by @zanieb on 2024-02-19 18:16_

---

_Label `compatibility` added by @zanieb on 2024-02-19 18:16_

---

_Comment by @sbidoul on 2024-02-21 17:07_

>  I wonder what we can provide for people who want to generate wheels for all their dependencies though. It makes sense for pre-building and sharing when they aren't distributed by the original package.

I echo this. `pip wheel` is useful to download and build all parts of an app for deployment. For instance in a multi-stage Dockerfile it is common to have a fat build stage with all build tools to create the wheels, then a slim stage which only installs the wheels built in the previous stage. 

---

_Comment by @henryiii on 2024-02-21 18:08_

But it doesn’t actually put wheels in the wheelhouse if there’s already a built wheel, IIRC? Pip download will get everything you need then you can loop over any SDists and build them if you need them?

---

_Comment by @sbidoul on 2024-02-21 18:21_

> But it doesn’t actually put wheels in the wheelhouse if there’s already a built wheel, IIRC? 

@henryiii I'm not entirely sure what you mean with that?

> Pip download will get everything you need then you can loop over any SDists and build them if you need them?

Sure. `pip wheel` is convenient because it does exactly that for you.

Actually `pip wheel` is quite easy to implement and maintain because, it basically does everything `pip install` does except it stores the wheels (whether they are downloaded or built locally) into a directory instead of unpacking them in site-packages. I'd argue it is simpler than `pip download`, from my experience with that part of the pip code base.


---

_Comment by @sbidoul on 2024-02-21 18:29_

That said, `build`, `wheel` and `download` are all useful, they are for different use cases.

---

_Comment by @henryiii on 2024-02-21 18:34_

> because it does exactly that for you ... whether they are downloaded or built locally

That's the problem, it does not store wheels that are downloaded. So if you depend on `numpy`, you will not get a `numpy` wheel in your wheelhouse _unless_ there is no `numpy` wheel for that platform. This causes `twine wheelhouse/*` to work until you build on a platform without numpy wheels, then it crashes because you can't push `numpy` to PyPI. We have to work around that in `cibuildwheel`, as well as all the other builders that have an option to use `pip wheel`.

---

_Comment by @sbidoul on 2024-02-21 18:57_

> That's the problem, it does not store wheels that are downloaded. So if you depend on numpy, you will not get a numpy wheel in your wheelhouse unless there is no numpy wheel for that platform.

That's strange. It is not my understanding of how `pip wheel` works. I use it daily and it always stores a wheel for each top level requirement and all their dependencies, whether dependent wheels exist in the index (in which case it will download them to the wheelhouse) or not (in which case it will download the sdists, build them and store the resulting wheels in the wheelhouse).

Now if the purpose is uploading wheels to an index, I'd use `build -w`, or `pip wheel --no-deps`, which are more or less equivalent?

---

_Comment by @henryiii on 2024-02-21 20:13_

I’ll investigate, maybe it changed or there’s an another reason it was behaving like that.

---

_Comment by @akx on 2024-03-27 13:05_

I'd like an `uv pip wheel` sort of command; it's very useful for multi-stage Docker image builds where you might need e.g. the full Python dev kit to build wheels, and then want to just use those cached wheels in a subsequent stage, á la

```
FROM python:3.12 AS requirements
COPY requirements.txt /wheels/requirements.txt
RUN cd /wheels && pip wheel --no-cache-dir -r requirements.txt

FROM python:3.12-slim AS runtime
RUN --mount=type=cache,ro,from=requirements,source=/wheels,target=/wheels pip install --no-cache-dir --no-index --find-links /wheels /wheels/*.whl
```

---

_Comment by @potiuk on 2024-03-27 13:26_

> I'd like an uv pip wheel sort of command; it's very useful for multi-stage Docker image builds where you might need e.g. the full Python dev kit to build wheels, and then want to just use those cached wheels in a subsequent stage, á la

Small comment on that one (I do not contest the need for pip wheel of course just explaining how we are doing it in Airflow). I found that you can do quite a bit better than that by just copying the whole `venv` installed in the "build stage". If you keep it in the same location, this will work out of the box as well.

Smthing like:

```
FROM python:3.12 AS requirements
COPY requirements.txt /requirements.txt
RUN python -m venv /home/user/.venv && /home/user/.ven/bin/python -m pip install -r /requirements.txt

FROM python:3.12-slim AS runtime
COPY --from=requirements ~/home/user/.venv /home/user/.venv
```

That saves the hassle of running `pip/uv install` twice. 


---

_Comment by @mmerickel on 2024-04-05 05:57_

Just to second this issue, my pipeline is dependent on the recursive nature of `pip wheel` to download all dependencies into a wheelhouse which we can then install offline. Using `pip download` is possible but would require another loop to build wheels for everything into a proper wheelhouse and I'd really be looking for the `uv` equivalent to support the full cycle directly from requirements file -> wheelhouse.

---

_Comment by @pradyunsg on 2024-04-06 15:14_

I tend to think of `pip wheel` as being useful for multiple roles:

- "give me a bunch of wheels" by default.
- "give me a wheel for this package" via an opt-in (`--no-deps`).

The default is _not_ the most common operation during development, and the functional behaviour provided by `pypa/build` is better suited to the right behaviours during package development workflows. Ideally, these two should be under a logically different commands in uv.

None the less, my two cents would be that this shim should not be provided and uv should instead focus on its own dedicated CLI outside of the pip namespace; keeping these two "consumer" and "publisher" pieces separated.

---

_Comment by @vigneshmanick on 2024-04-11 07:45_

To second this issue, In our usecase we build the wheel once (`pip wheel . -w dist`)and store it as an artifact which is then used by subsequent pipelines.  This helps us to ensure that the production and ci environments are consistent and also helps to manually check the wheel contents without having to resort to additional commands. 



---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-31 16:47_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-31 18:49_

---

_Comment by @mmerickel on 2024-09-06 23:48_

FWIW I added a concrete example of how I use pip wheel to https://github.com/astral-sh/uv/issues/7148 if it's helpful in motivating this feature.

---

_Comment by @notatallshaw on 2024-09-06 23:51_

The command `uv build` now exists, does this not cover this requirement?

---

_Comment by @mmerickel on 2024-09-07 00:00_

The issue is that the new command doesn’t build the dependencies. Neither the local dependencies in the workspace nor the remote dependencies from the package index. 

---

_Comment by @Ralith on 2024-09-16 05:40_

This would be useful for packaging Blender extensions, which need their dependencies bundled as wheels in a subdirectory: https://docs.blender.org/manual/en/dev/advanced/extensions/python_wheels.html

---

_Comment by @sliva0 on 2024-10-02 15:16_

This issue is essential to adopt `uv` in a project I'm working on to make reproducible offline installations possible, and it doesn't seem to move forward. Is there a way I can contribute to working on this as a rust novice?

---

_Comment by @abhiaagarwal on 2024-11-15 19:55_

This issue has become a blocker for hardened python images, as we use want to use uv in a build step to and then copy over the dependencies into a venv we can't overwrite (Iron Bank images as a concrete example https://repo1.dso.mil/dsop/opensource/python/python313)

Similar to other people, I'd like to build reproducible wheels in an environment with dev dependencies (cmake), as the hardened images don't have anything to work with. My workaround has been using uv export to generate a requirements.txt, then pull an ubi9 image to build the wheels, and then load them into the required venv, but this, as you can imagine, is not fun. 

---

_Comment by @mmerickel on 2024-11-15 20:50_

Building on my earlier comments, I'll expand slightly with a concrete workaround.

I'm hacking around the lack of this feature right now by using a combination of `uv export` and `pip`. A reminder that the workspace itself is a bunch of independent packages with their own build backend etc that can be built using other build tools like `pip wheel`, `python -m build`, or `uv build` so you can pick things apart and build them as needed without uv commands. For example:

```
uv export --no-hashes --locked --format requirements-txt > requirements.txt
grep -v '^-e ' requirements.txt > requirements.remote.txt

# build all remote wheels
pip wheel -w wheels --find-links wheels -r requirements.remote.txt

# build all local packages to wheels
uv build --all-packages --wheel -o wheels
```

You can expand this further, for example I'm doing similar things for other dependency groups independently, or build requirements first,, and I'm not actually using `uv build` directly because I'm caching the local wheels based on git commits since `uv build` does not cache things well to avoid rebuilding 20 packages if only 1 changed.

The goal of something like this ticket is that we could just directly use uv build like this `uv build --all-packages --wheel -o wheels` and tack on another flag like `--recursively-build-all-deps`.

---

_Comment by @notatallshaw on 2024-11-15 20:56_

> pip wheel -w wheels --find-links wheels -r requirements.remote.txt

You can also add `--no-deps` here to save pip from reading and resolving dependencies, since you already have fully resolved the dependencies with uv.

---

_Comment by @abhiaagarwal on 2024-11-17 14:47_

I've started working on this, please let me know if anyone else is. Most of the plumbing is already there. 

---

_Comment by @mmerickel on 2024-11-17 16:32_

I should note it’d be nice if there was an option to build only the dependencies and not the local packages - and specifically only the dependencies that aren’t in the target destination already similar to how `--find-links` works. :-)

---

_Comment by @abhiaagarwal on 2024-11-17 17:14_

> I should note it’d be nice if there was an option to build only the dependencies and not the local packages - and specifically only the dependencies that aren’t in the target destination already similar to how `--find-links` works. :-)

I'll need to think about the right way to do this, but my general design idea is:

`uv build`: build wheels (already exists) 
`uv build --all-packages`: build wheels for all workspace packages (already exists)
`uv build --with-deps`: build wheels + dependencies
`uv build --all-packages --with-deps`: build wheels + dependencies for all workspace packages
`uv pip wheel`: same as `uv build --with-deps`, provided as shim

I'm not sure what's the best way to support installing **only** dependencies. 

---

_Comment by @Tremeschin on 2024-11-17 19:11_

@abhiaagarwal These new proposed options looks great! I may have a novel use case for them btw 😄

Basically, since it would download and build a fully reproducible offline installation talked previously, we can side-load all wheels on a pyapp binary (I have a fork for it) for a simpler solution on entirely offline executables :)

My two cents; It would also be nice if we could specify `--platform {windows,linux,macos} --arch {x86_64,arm64}` etc. for getting cross platform files, and also support for `uv pip download` (as `uv download`?) for individual/outlier packages like pytorch and its many flavors, but this might be out of this issue's scope or complex to implement haha

Looking forward for progress on this! 👍🏻 

---

_Comment by @T-256 on 2024-11-17 20:55_

> I've started working on this, please let me know if anyone else is. Most of the plumbing is already there.

Hey, it's great to hear that, there are some of my thoughts about combined version of `pip wheel` and `pip download`:
https://github.com/astral-sh/uv/issues/3163#issuecomment-2481505055

> I'm not sure what's the best way to support installing **only** dependencies.

As mentioned in examples in there, I think it could be solved by index filtering (May need to have extra index configuration in `pyproject.toml` to explicitly split them.)

---

_Comment by @abhiaagarwal on 2024-11-17 20:55_

@Tremeschin yeah, that's the thing, pip install / wheel / download are all expressions of the same base concept, except doing slightly different things (ie. wheel is just download, except it also builds if it can't pull a wheel directly from the index).


---

_Comment by @abhiaagarwal on 2024-11-17 20:58_

@T-256 you read my mind, I was also thinking of the exact nomenclature of `uv collect`, because `uv pip download/wheel` can be implemented using the exact same logic in `uv build` except for the actual building part. Since uv uses its cache first, the act of "downloading a wheel" is already abstracted away (we just need to query the cache for the wheel, it'll download it if it doesn't exist, and then actually package it as a .whl).

It also ties in to a higher-level abstraction of the cache itself, ie `uv cache populate` or something of that sort 

---

_Comment by @mmerickel on 2024-11-21 22:25_

> I'm not sure what's the best way to support installing only dependencies.

I'd request a `--only-deps` option on both `uv export` and on `uv build`. It would trigger the flow to exclude anything that is local and only build things that it'd normally have to get from an index. With `uv export` the rendered lock files wouldn't contain the local packages. With `uv build` it would not build the local packages, just the external things they depend on.

---

_Comment by @henryiii on 2024-11-21 22:57_

You have to build local packages to get the dependency list, though? You can shortcut it in the case of a PEP 621 package without dependencies in the dynamic array, and some backends let you shortcut the build with the get metadata hook, but sometimes you have to build the package to get the deps.

---

_Comment by @mmerickel on 2024-11-21 23:12_

That dependency information is already in the lock file but otherwise yes you would need to build/discard it to get the metadata if you didn't have a lock file.

---

_Comment by @edwardpeek-crown-public on 2024-11-21 23:37_

FWIW our use case for `pip wheel` and dependencies we already know what the dependencies we want are, so just run `pip wheel --no-deps $DEPENDENCY` for each of them. 

You can get a such a list already using `[uv] pip compile -e $PROJECT_DIR` so full `--only-deps` functionality might not be needed for an MVP implementation.

---

_Comment by @Spindel on 2024-12-03 16:45_

I've got multiple uses of "pip wheel",  one of them is as many others, artifacts for a build or multi-stage pipeline,  where we also separate dev dependencies (linters, etc)  from real deps, so we can always use the same for child pipelines. Sometimes these end up in a kind of "dev container" that's used as a base, in order to prevent updated packages in repositories from causing new fun issues.

The other case is to make sure our internal repo has cached binary builds of certain projects for non x86_64 platforms,  fex. lxml, cryptography, etc.   

So, for that reason, being able to download or build both `--dev` and normal deps is needed.

---

_Comment by @kimvais on 2024-12-17 08:52_

> `pip wheel` does one thing differently, by the way - it includes any wheels it also _builds_ (but not just downloads), while `build --wheel` only gives you the wheel you asked for. But most of the time that's actually a bug (trying to upload wheels you don't own), and `pip download` is there if you want to actually get existing files from PyPI (not in uv yet though).

Actually, including wheels you don't "own" is desirable for several use cases, namely for private PyPI-like repositories (e.g. Google Artifact registry, AWS CodeArtifact et al.) at least:

1.  Security. If you can publish all the dependencies in your private repo, there is not even a theoretical code injection via PyPI vulnerability as you can use `--index-url` instead of `--extra-index-url`. Consider package "companyname-foobar-security` - if you use `--extra-index-url` and someone guesses the package name and pushes the package with that name and most satisfactory version number -> RCE.
2. Faster build times when you had to version lock a dependency, or the binary wheel isn't available for your runtime and arch combination for some other reason and each `uv pip install` needs to rebuild the package from sdist.


---

_Comment by @ion-elgreco on 2025-01-17 09:33_

@abhiaagarwal hey! Did you make any progress on this? :D 

---

_Comment by @abhiaagarwal on 2025-01-17 11:53_

@ion-elgreco I had, and then we managed to solve the problem without the use of `pip wheel` so it became less of a priority.

I can publish a WIP branch once I rebase on main, if that would be useful for you?

---

_Comment by @Skillossus on 2025-02-06 14:53_

@abhiaagarwal really curious as well if you have a tl;dr of how you accomplished this.

---

_Comment by @omer54463 on 2025-02-14 20:50_

Is anyone working on this feature? I could really use it.
My current solution is to install pip, which works but isn't ideal.

---

_Comment by @abhiaagarwal on 2025-02-15 22:57_

> [@abhiaagarwal](https://github.com/abhiaagarwal) really curious as well if you have a tl;dr of how you accomplished this.

Haven't had cycles to rebase recently, but the general idea is you can pull directly from uv's wheel cache using the same logic as the sync command, just into a directory rather than a venv. This is also how pip download can be naturally supported.

---

_Comment by @ajanitshimanga on 2025-03-06 01:10_

Is this being worked on currently? I am considering adopting uv as the dependency manager of choice for my application but found a post mentioning that, "uv doesn't generate wheels, so my quest is: UV unusable for 2-stage docker image building" ([medium link](https://medium.com/@gnetkov/start-using-uv-python-package-manager-for-better-dependency-management-183e7e428760)) and would I have to work around UV by using requirements.txt or pip directly. Additionally, how difficult is this to implement, is this something a new contributor can pick up?

---

_Comment by @detlefla on 2025-03-11 21:33_

I'd also love to see some functionality like this in uv. I'm still using `pip wheel` for this (which has to be run by uv again so that it picks up the right Python interpreter – not a good fit for a uv-based workflow) for deployments on webservers which shouldn't have compilers and dev tools / libs installed.

It would be even better if `uv sync` had an option for dropping a wheel into a directory for each installed package, making sure that this wheel and the hash in uv.lock agree. Then the wheels and the lock file could be copied over to the server and safely synced into a venv there.

---

_Comment by @abhiaagarwal on 2025-03-26 12:11_

I opened a draft MR for this, after looking through uv's cache at work on a whim and realizing uv's cache internal archive format is entirely identical to a wheel (validated with `diff -r -q` between uv/pip wheel) and you can just zip the archive to create a "wheel". It's kind of an open question whether this is correct behavior / something that I should rely on, because from a design perspective, it means that uv is tied to the implementation of its cache resembling a wheel, which _feels_ bad.

That being said, treating `uv pip wheel` as another part of `uv pip install`, since `uv pip install` also downloads dependencies / builds them is the right approach. It's now the question of how given a prefilled cache, we turn that into wheels. 

---

_Comment by @henryiii on 2025-03-26 16:25_

> uv doesn't generate wheels

Reminder, `uv build --wheel` absolutely does generate wheels. It just doesn't save wheels for your dependencies too. I think these are the common needs, ordered by how commonly they are needed:

* The end wheel only. That's `uv build --wheel`. Good for uploading to PyPI. If you just want to download an existing wheel, `pip download numpy` is identical to `pip wheel numpy` (no uv equivalent for that one).
* All dependencies too. That's `pip download` (no uv equivalent). Good for making your own index (including for tests).
* All dependencies + build wheels for SDist-only dependencies (no uv equivalent). Good for making your own index on a specific platform.

I believe `pip wheel` used to not save wheels for things that were already wheels on PyPI, but it looks like modern versions do save all dependency wheels. I think `pip wheel` then is identical to `pip download`, then running `uv build --wheel` on every SDist, then deleting all the SDists.

---

_Comment by @abhiaagarwal on 2025-03-26 16:42_

`uv build --wheel` doesn't technically generate wheels. What does generate wheels is the build backend, which uv calls (via the build frontend). That's kind of the problem here, uv has the unzip wheel => store in cache, but it doesn't have the reverse. I found that by zipping their cache you get a wheel that is hash-equivalent to the indexed wheel.

---

_Comment by @waynew on 2025-04-25 21:03_

I thought that I had posted it but I can't find it in here -- `uvx pip download . -d dist` actually works for me so far 🤷 

---

_Comment by @BradyAJohnston on 2025-04-26 01:53_

@waynew that's just using `uv` to run the actual `pip` module, rather than `uv`'s submodule (equivalent to running `python -m pip download`). People are wanting to actually use `uv` for the download which is much faster for dependency resolution & downloading, rather than just running vanilla `pip`.

---

_Comment by @waynew on 2025-04-28 12:52_

Ah - yes; apparently I forgot to include that bit. It's definitely just a workaround -- mainly it avoids the scenario where your platform doesn't like installing `pip`.

Though... now I wonder if there's a `pip download` that could use `uv`'s cache until `uv` gets their own `pip download` equivalent.

Anyway -- entirely ➕ 💯  on uv having a baked-in command. For anyone who still needs to download deps, but also wants to use `uv`s workflow and not have to have `pip` installed as a dependency of their project, or has a platform that doesn't like you having pip... hopefully that helps!

---

_Comment by @notatallshaw on 2025-04-28 13:44_

@BradyAJohnston @waynew the problem is the cache, uv caches installed packages, but pip caches distributions. 

At the moment it's not clear to me, without a cache redesign, that uv would be faster than pip, when cache is available pip can simply copy the files, but uv either needs to try and recreate the wheels (it can't recreate sdists) or redownload them. When cache isn't available the scenario tends to be dominated by IO, uv downloads concurrently but this doesn't always help and in some edge cases slows things down. 

Maybe uv would be better suited by coming up with its own solution to your use case rather than copying pip. For example having a command that creates one zip file, or a zip file per package, of the relevant uv cached packages, and uv can be pointed to that file and extract them from there.

---

_Comment by @waynew on 2025-04-28 14:09_

I wouldn't claim to have a comprehensive scenario of folks' `uv` usage, and while [I'd love a slightly different flow](https://hynek.me/articles/docker-uv/) currently I'm limited to using `uv` pretty much only in my development environment. And my target deploy environment has extremely limited capabilities. If I want `.whl`s over there I have to `rsync` from the dev environment. So I use `pip download` to pull all the wheels that I can sync over to the dev/staging/prod environments.

Not sure how this fits in with uv's caching model, but it's my current approach that works!

---

_Comment by @edwardpeek-crown-public on 2025-04-29 00:35_

Our workflow would benefit from the caching being shared across:
* `uv pip compile` - for filtering pinned CI + runtime deps down to just runtime deps
* `uv pip sync` - for installing all pinned deps into a CI venv
* `pip wheel` (or uv alternative) - for bundling runtime deps so they can be efficiently offline installed into arbitrary venvs

In the past we've seen a lot of time wasted by these steps individually repeating expensive builds of packages including native code (eg. numpy, uwsgi), which is why a common cache between all three is helpful. Right now there's no way to avoid steps 1 & 3 doing redundant work

Step 3 could hypothetically be replaced by hacking something out of `uv venv --relocatable`, but we'd prefer to use standard packaging (ie. wheels) rather than reworking it all around uv-exclusive functionality.

---

_Comment by @akaszynski on 2025-10-29 15:17_

> I thought that I had posted it but I can't find it in here -- `uvx pip download . -d dist` actually works for me so far 🤷

This would be perfect, except it doesn't support the `--python-platform` flag.

---

_Comment by @noonedeadpunk on 2025-11-07 11:57_

+1 to the discussion. We are also missing equivalent of `pip wheel --requirement` to download/build wheels for third-party packages as part of software deployment process, when we wanna to prepare a local cache based of constraints, which will be served later during installation.

But we don't have a python project to build from, rather a list of requirements. 

---
