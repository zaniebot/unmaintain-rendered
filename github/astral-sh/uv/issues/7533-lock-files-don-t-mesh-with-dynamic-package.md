---
number: 7533
title: "Lock files don't mesh with dynamic package version info"
type: issue
state: closed
author: hynek
labels: []
assignees: []
created_at: 2024-09-19T05:59:19Z
updated_at: 2025-06-23T09:49:00Z
url: https://github.com/astral-sh/uv/issues/7533
synced_at: 2026-01-07T13:12:17-06:00
---

# Lock files don't mesh with dynamic package version info

---

_Issue opened by @hynek on 2024-09-19 05:59_

Since tox-uv just shipped lock files, I took a stab at moving attrs to a fully-locked dev environment.

Here's the experiment PR: https://github.com/python-attrs/attrs/pull/1349

A problem I've run into is that packages often use dynamic packaging data that is based on git metadata (setuptools-scm, hatch-vcs, etc). As such, attrs's package version changes after every commit, which allows us to upload to test PyPI and continually verify our package. See, for example, https://test.pypi.org/project/attrs/24.2.1.dev19/

Unfortunately the current project gets locked like this:

```toml
[[package]]
name = "attrs"
version = "24.2.1.dev20"
source = { editable = "." }
```

Which means it's outdated after each commit, because every commit increments the number behind `dev`.

Is there a way around this that I've missed? If not, could there be built one? 😇 AFAICT, this is currently the biggest blocker for FOSS use from _uv_'s side. I'm also not sure what the point of that version lock is in the first place for the current, editable project? But that's a topic for another day. :)

---

```
❯ uv --version
uv 0.4.12 (2545bca69 2024-09-18)
```

---

_Comment by @sbidoul on 2024-09-19 06:23_

@hynek have you seen `tool.uv.cache-keys` ?

---

_Comment by @hynek on 2024-09-19 06:29_

I have while searching this bug tracker, but I couldn't find a way for it to affect `uv.lock`? Changing the value to `cache-keys = [{ git = true }, { file = "pyproject.toml" }]` at least doesn't do anything when running `uv.lock`.

From the documentation (and its name 🤓) I've gathered it's mostly about how caching, not about locking? And TBH it sounds to me as if adding git as a git key, I'm actually _adding_ cache busting.

---

_Comment by @charliermarsh on 2024-09-20 02:59_

I think you could pass `--upgrade-package attrs` to ensure that `attrs` is rebuilt and that the lockfile is updated. Or even set it in your configuration:

```toml
[tool.uv]
upgrade-package = ["attrs"]
```

But there's sort of an infinite loop here, right? Since every time you commit the lockfile, it will be outdated. So you then re-lock, and commit again, etc. I don't know how to solve this holistically. We could omit versions for editables, maybe? Or even, write "dynamic" or something for the version, for packages that have dynamic versions?


---

_Comment by @hynek on 2024-09-20 03:27_

> But there's sort of an infinite loop here, right? Since every time you commit the lockfile, it will be outdated. So you then re-lock, and commit again, etc. We could omit versions for editables, maybe? Or even, write "dynamic" or something for the version, for packages that have dynamic versions?

Yeah that's what I meant in my last sentence, but I'm always assuming Chesterton's Fence. :) Setting it to `dynamic` seems the best way since it maps nicely to `project.dynamic`. Just dunno if it would be a problem that there's a case where there's an invalid version "number" in the field which would speak for omitting.

---

_Comment by @jkeifer on 2024-09-20 17:52_

I too have this problem. I generally subscribe to the philosophy that I use a VCS for versioning, thus what is in the VSC repo shouldn't track it's own version because that's what I'm using the VCS for. Said another way, at best a version tracked within repo is imperfect for exactly the reasons described above about how you can't update a version from a commit without making another commit, i.e., a new version.

I'd love to understand the rationale behind storing this version in the lock file. Can anyone elaborate on that?

---

_Comment by @burgholzer on 2024-09-26 15:53_

We are also facing a similar problem over at https://github.com/cda-tum/mqt-core/pull/706 where we use renovate to keep the `uv` lock file up to date, which would be pretty convenient in principle.
However, currently it triggers a never ending chain of renovate update PRs because every update to the lock file adds a commit, which changes the `setuptools-scm` version, which causes another update PR to be triggered. (see https://github.com/cda-tum/mqt-core/pull/703, https://github.com/cda-tum/mqt-core/pull/704, https://github.com/cda-tum/mqt-core/pull/705, https://github.com/cda-tum/mqt-core/pull/706)

---

_Comment by @LordFckHelmchen on 2024-10-01 11:36_

This problem is not only for dynamic versioning. We have a release-please workflow, which will create a PR for the next release. This will update the pyproject.toml version correctly but of course does not know about the uv.lock version. 
If there would be an option to omit the project version from the uv.lock it would be great, as I currently also don't understand why it needs to be kept in the lock-file itself.

---

_Referenced in [astral-sh/uv#7994](../../astral-sh/uv/issues/7994.md) on 2024-10-08 09:29_

---

_Comment by @david-waterworth on 2024-10-08 22:22_

As I mentioned in  ttps://github.com/astral-sh/uv/issues/7994 I use`python-semantic-release` which behaves the same. The way I work around the infinite build loop is to configure my Jenkins script to skip commits that contains the `python-semantic-release` message - these commits include the changed `pyproject.toml` and `__version__.py` file, along with a generated CHANGELOG.md 

As a workaround, I also added `uv lock` at the start of the build script and `git add uv.lock` at the end - this ensures that `python-semantic-release` commit includes the correctly versioned `uv.lock` file (I'm not sure yet if this causes other problems, i.e. updating other packages - ideally there would be a way of only updating the package version in `uv.lock` and nothing else.


---

_Referenced in [edgarrmondragon/meltano-hub-api#143](../../edgarrmondragon/meltano-hub-api/issues/143.md) on 2024-10-17 01:58_

---

_Referenced in [astral-sh/uv#8590](../../astral-sh/uv/issues/8590.md) on 2024-10-26 11:58_

---

_Comment by @alejandrodnm on 2024-10-31 10:59_

I've just encountered this. We are also using release-please and uv in an internal project, after a release then the surprise is that the uv.lock was outdated.

Ours plans were to add uv to https://github.com/timescale/pgai but since we are already using release-please, we are hesitant due to this issue.

---

_Referenced in [astral-sh/uv#6860](../../astral-sh/uv/issues/6860.md) on 2024-11-04 10:50_

---

_Referenced in [localstack/localstack-sdk-python#5](../../localstack/localstack-sdk-python/pulls/5.md) on 2024-11-05 09:11_

---

_Comment by @RazerM on 2024-11-06 11:49_

While the setuptools-scm case seems difficult to solve, I think another common reason[^1] a dynamic version is used is when the version is statically defined but not in `pyproject.toml`. Typically as a `__version__` attribute like in this example:

1. Create example project

    ```bash
    uv init --lib mpypkg && cd mpypkg
    ```

2. Configure `pyproject.toml` to use a dynamic version

    ```bash
    cat <<EOF > pyproject.toml
    [build-system]
    requires = ["setuptools"]
    build-backend = "setuptools.build_meta"

    [project]
    name = "mpypkg"
    requires-python = ">=3.12"
    dynamic = ["version"]

    [tool.uv]
    cache-keys = [{ file = "pyproject.toml" }, { file = "src/mpypkg/__init__.py" }]

    [tool.setuptools.dynamic]
    version = { attr = "mpypkg.__version__" }

    [tool.setuptools]
    package-dir = { "" = "src" }

    [tool.setuptools.packages.find]
    where = ["src"]
    EOF
    ```

3. Add `__version__` attribute to `__init__.py`

    ```bash
    cat <<EOF >> src/mpypkg/__init__.py
    __version__ = "1.0"
    EOF
    ```

4. Run `uv lock`

5. Change version number

    ```bash
    sed -i.bak 's/__version__ = "1\.0"/__version__ = "1.1"/' src/mpypkg/__init__.py
    ```

6. Run `uv lock`

What I expected:

```diff
 version = 1
 requires-python = ">=3.12"
 
 [[package]]
 name = "mpypkg"
-version = "1.0"
+version = "1.1"
 source = { editable = "." }
```

What happened: uv.lock still has `version = "1.0"`

I've also tried `uv lock --no-cache --refresh` to no avail.

The version doesn't change until I do an unrelated `uv add` or something. This seems like a cache invalidation problem more than anything else, which I expected `cache-keys` to solve.

[^1]: sometimes for backward compatibility reasons or in my case I can't move version into pyproject.toml until some other tooling I use catches up.

---

```
❯ uv --version
uv 0.4.30 (61ed2a236 2024-11-04)
```

---

_Comment by @charliermarsh on 2024-11-06 13:59_

We can fix this, but I still strongly recommend against setting the version dynamically like that.

---

_Referenced in [astral-sh/uv#8866](../../astral-sh/uv/issues/8866.md) on 2024-11-06 16:30_

---

_Comment by @charliermarsh on 2024-11-06 16:31_

I've fixed the issue @RazerM pointed out in https://github.com/astral-sh/uv/pull/8867. I guess I'll leave _this_ issue open though since it's more about the inherent incompatibility between lockfiles and VCS-based versioning.

---

_Comment by @cpascual on 2024-11-06 18:44_

Hi @charliermarsh, you mention _"inherent incompatibility of lock files and VCS-based versioning"_ , but you also mentioned:

> We could omit versions for editables, maybe? Or even, write "dynamic" or something for the version, for packages that have dynamic versions?

To me the above workaround seems good enough, specially if it is controlled by an opt-in configuration (e.g. `omit_lock_version=["mypackage",]`). 

Is there any fundamental problem with this approach?


---

_Comment by @cpascual on 2024-11-06 18:44_

my use case is that I use `setuptools-scm` and the presence of commit-dependent version numbers in `uv.lock` is a frustrating cause of git merge conflicts preventing what otherwise would be an automated merge.

---

_Comment by @ceejatec on 2024-11-08 08:15_

> We can fix this, but I still strongly recommend against setting the version dynamically like that.

@charliermarsh Out of curiosity, why?

---

_Comment by @Coruscant11 on 2024-11-08 18:03_

I just discovered this issue with the recent release 0.5.0. Using --locked when syncing the project makes everything fails. I have a dynamic version based on git tags, and when we release we tag our repository. So the lock file can be outdated without any code change.

I think excluding editable dependencies in the lock file could be a good workaround. Or we can also write "dynamic" as a version. By the way I will check what tools like poetry for example are doing in this use case.
@charliermarsh Is there any news on this? In the meantime, we will pin our version to the 0.4.30.

By the way I would suggest to list it in the breaking changes of the release

Thanks !!!

Edit: Forgot to mention, I was wondering if you already have some implementation ideas, I totally volunteer to work on this if needed. I will try a draft pull-request on my side for fun 

---

_Comment by @Stealthii on 2024-11-13 00:14_

> We can fix this, but I still strongly recommend against setting the version dynamically like that.

If references or opinion articles exist around this topic, they are few in number. I think a few on this issue would be interested in wider discussion around this. [PEP 621](https://peps.python.org/pep-0621/) introduced toml metadata, and as of writing the [official specification](https://packaging.python.org/en/latest/specifications/pyproject-toml/#pyproject-toml-spec) still allows `version` to be set dynamically, with no recommendation to the contrary.

It's a pretty standard paradigm (see [hatch-vcs](https://github.com/ofek/hatch-vcs), [setuptools-scm](https://github.com/pypa/setuptools-scm), [poetry-dynamic-versioning](https://github.com/mtkennerly/poetry-dynamic-versioning)) for single project repos where versioning, point release branches, development builds, etc. are all managed and controlled via VCS change control.

Whilst alternatives exist for separating static versions (such as `0.5.1` from a generated identifier ref like `f399a5271` in `uv 0.5.1 (f399a5271 2024-11-08)`), there are some compelling reasons to use dynamic (such as [dunamai](https://github.com/mtkennerly/dunamai)) formed versions:
* Unique reference of all release, post, dev, dirty distributions of Python packages
* All tarball/wheel builds being uniquely available in an internal PyPI repository for further testing or triage
* Rules around versioning and releases can be set by project owners irrespective of development language
* No room for user error (such as forgetting to update a static version of `0.5.1` for the git tag `v0.5.1` and publishing builds)
* Exposing the version in test logs, headers, etc. automatically shows the exact build used, aiding debugging

---

My expectation for the project package that has a **dynamic** version (`source = { editable = "." }` specifically) would be simply not defining version in `uv.lock` for the local package (or providing a dynamic reference). This by nature isn't a lockable reference (especially when set by VCS signatures) whereas other uses of `uv lock` such as multi-project mono repos or static versions uphold the current behaviour.

It's worth noting that although version is defined dynamically in source control, builds of the package provide the version statically:
```console
❯ ls -1 dist/*
dist/mypackage-0.5.0.dev5+g5d056c1-py3-none-any.whl
dist/mypackage-0.5.0.dev5+g5d056c1.tar.gz
❯ unzip -p dist/*.whl '*.dist-info/METADATA' | grep -E '^Version: '
Version: 0.5.0.dev5+g5d056c1
❯ tar -Oxf dist/*.tar.gz '*.egg-info/PKG-INFO' | grep -E '^Version: '
Version: 0.5.0.dev5+g5d056c1
```

---

_Comment by @charliermarsh on 2024-11-13 03:05_

> If references or opinion articles exist around this topic, they are few in number.

This is just my personal opinion! Dynamic metadata makes a number of things more complicated than they need to be. I understand the use-case for scm versioning, but in my opinion using dynamic metadata to read a version from `__init__.py` is a really heavy hammer with the only benefit being that you get to avoid writing out a constant twice -- I was responding to that specific case, where I don't think the tradeoffs are very good.

---

_Comment by @charliermarsh on 2024-11-13 03:06_

> ...simply not defining version in uv.lock for the local package (or providing a dynamic reference)

Unfortunately it's not super simple -- it breaks the fundamental assumption that packages have versions, so we have to take that into account everywhere.

---

_Comment by @Coruscant11 on 2024-11-13 06:07_

> Unfortunately it's not super simple -- it breaks the fundamental assumption that packages have versions, so we have to take that into account everywhere.

Yes this is what I saw this week-end when I was trying to contribute on this 🥲
But anyway, I had the opportunity to deep dive in the code base so it's fine.
We will have to trust you on this 😄 (No need to worry at all then)

Thanks!

---

_Comment by @ceejatec on 2024-11-13 06:19_

> it breaks the fundamental assumption that packages have versions

Not exactly, I don't think. The package always DOES have a version. It's just that the version may change outside of uv's immediate notice. Perhaps it would be fine if `uv lock` and `uv sync` and similar commands re-assessed the version if it's marked `dynamic` in the `pyproject.toml`. That wouldn't handle cases where the dynamic version contains the git commit SHA in some form, but I suspect that's pretty much an intractable problem.

That said, I feel like a cleaner solution would be to omit the main package from `uv.lock` entirely. However I'm definitely not familiar with the implementation here, so I don't know what unforeseen consequences that might have.

---

_Comment by @hynek on 2024-11-13 08:25_

> > If references or opinion articles exist around this topic, they are few in number.
> 
> This is just my personal opinion! Dynamic metadata makes a number of things more complicated than they need to be. I understand the use-case for scm versioning, but in my opinion using dynamic metadata to read a version from `__init__.py` is a really heavy hammer with the only benefit being that you get to avoid writing out a constant twice -- I was responding to that specific case, where I don't think the tradeoffs are very good.

By the way, this can be inverted and made more standard-compliant while doing so by putting something like this into your `__init__.py`:

```python
def __getattr__(name: str) -> str:
    if name != "__version__":
        msg = f"module {__name__} has no attribute {name}"
        raise AttributeError(msg)

    from importlib.metadata import version

    return version("YOUR-PACKAGE")
```

You get static metadata with no duplication.

---

_Comment by @ninoseki on 2024-11-13 08:35_

I have the same pain point and would like to share how many projects in the wild do Git based versioning for your information.

- [hatch-vcs](https://github.com/search?q=path%3Apyproject.toml+%22hatch-vcs%22+%22dynamic+%3D+%5B%5C%22version%5C%22%5D%22&type=code): 2.2k files
- [poetry-dynamic-versioning](https://github.com/search?q=path%3Apyproject.toml+%22tool.poetry-dynamic-versioning%22+%22enable+%3D+true%22&type=code): 1.6k files

---

_Comment by @roteiro on 2024-11-13 09:36_

> I have the same pain point and would like to share how many projects in the wild do Git based versioning for your information.
> 
> * [hatch-vcs](https://github.com/search?q=path%3Apyproject.toml+%22hatch-vcs%22+%22dynamic+%3D+%5B%5C%22version%5C%22%5D%22&type=code): 2.2k files
> * [poetry-dynamic-versioning](https://github.com/search?q=path%3Apyproject.toml+%22tool.poetry-dynamic-versioning%22+%22enable+%3D+true%22&type=code): 1.6k files

add to that: 
+ [versioningit](https://github.com/search?q=path%3Apyproject.toml+%22versioningit%22+%22dynamic+%3D+%5B%5C%22version%5C%22%5D%22&type=code) 420 files

---

_Comment by @jamesbraza on 2024-11-13 18:21_

I think https://github.com/astral-sh/uv/pull/8867 released in `uv==0.5.0` leads `uv` to correctly respect dynamic metadata, which now breaks `setuptools-scm` + `pre-commit` users using the `uv-lock` hook: https://github.com/astral-sh/uv-pre-commit/issues/25.

This is textbook [Hyrum's Law](https://www.hyrumslaw.com/) 😆

---

To share on the SCM versioning debate, I use `setuptools-scm` as it's a nice way for an automated (no human error) workflow of GitHub tagged release to PyPI publish with matching version.

---

_Comment by @charliermarsh on 2024-11-13 18:24_

Yeah, it seems like fundamentally the only way to make this work with `setuptools-scm` is to omit the version or otherwise mark it as dynamic. I don't think it's _impossible_, but it's not, like, an hour of work. I'm also fairly worried about how we would integrate this if PEP 751 gets approved -- the PEP doesn't make any carveouts for dynamic versions, and there hasn't been any discussion of it in the [PEP thread](https://discuss.python.org/t/pep-751-now-with-graphs/69721/1). If we add a feature like this here, and it doesn't make it into the PEP, we'll have to remove it later.


---

_Referenced in [Future-House/ldp#152](../../Future-House/ldp/pulls/152.md) on 2024-11-13 18:38_

---

_Label `needs-design` added by @zanieb on 2024-11-14 15:32_

---

_Label `needs-decision` added by @zanieb on 2024-11-14 15:32_

---

_Comment by @apollo13 on 2024-11-14 15:53_

> I think #8867 released in `uv==0.5.0` leads `uv` to correctly respect dynamic metadata, which now breaks `setuptools-scm` + `pre-commit` users using the `uv-lock` hook: [astral-sh/uv-pre-commit#25](https://github.com/astral-sh/uv-pre-commit/issues/25).

Can confirm that our workflow (using hatchling as build backend with a dynamic version) breaks when trying to do `uv sync --locked` since uv `0.5`. `0.4.30` is fine. Whether it is actually #8867 that broke it I cannot tell, but sounds plausible.

---

_Comment by @charliermarsh on 2024-11-14 16:16_

It likely was https://github.com/astral-sh/uv/pull/8867 that led to the change, though sadly that really was a bugfix -- the existing behavior was wrong. Right now there just isn't a clear way to use scm-based versioning with lockfiles, but I'm thinking about it. My biggest hesitation to investing in it deeply is the aforementioned PEP 751. I don't want to do something that will become invalid once the standard exists.


---

_Comment by @dsully on 2024-11-14 17:40_

Is @brettcannon aware of this problem? Should PEP-751 take this usage into account?

(I'm also not 100% sold we need a universal lockfile format. I believe the 715 discussion thread also mentions this. 🤷 )

---

_Comment by @charliermarsh on 2024-11-14 18:04_

I've been deeply involved in the thread and don't _think_ it's come up, but I'll post about it today.

(Edit: @hynek already did.)

---

_Comment by @apollo13 on 2024-11-14 18:18_

> (I'm also not 100% sold we need a universal lockfile format. I believe the 715 discussion thread also mentions this. 🤷 )

Having a universal lockfile seems better than trying to add support for 100 different lock file formats to every other SBOM tool out there :D

---

_Comment by @brettcannon on 2024-11-14 18:21_

> Having a universal lockfile seems better than trying to add support for 100 different lock file formats to every other SBOM tool out there :D

I'm going to ask that if @dsully or anyone else wants to express concern over standardizing lock files they bring it up on the PEP 751 discussion over on discuss.python.org and not here as I don't think the uv issue tracker is the best place to talk about PEPs that affect the wider Python packaging ecosystem 😉

---

_Referenced in [astral-sh/uv#9126](../../astral-sh/uv/issues/9126.md) on 2024-11-14 18:25_

---

_Comment by @hynek on 2024-11-14 18:34_

For posterity: https://discuss.python.org/t/pep-751-now-with-graphs/69721/86

---

_Comment by @dsully on 2024-11-15 17:52_

Will do @brettcannon, thanks! Though looks like Hynek got there first 😎 

---

_Referenced in [reservoir-data/tap-fedidb#25](../../reservoir-data/tap-fedidb/issues/25.md) on 2024-11-15 18:13_

---

_Comment by @tomrutter on 2024-11-17 23:59_

Sorry to pipe in on a largely completed debate but I have one idea to put.

My issue here seems the same as many other users:

1. My python package uses automatic versioning based on git tag for fewer user errors and easier tracking of built artefacts.
2. The package version is included in lock file, which when committed changes commit tag and thus the version.
3. As part of CI, I check lock file is up to date (for reproducibility). This check fails because of the version change of the local workspace package. 


Changing any one of these links could fix the issue. I think most of the discussion has been about 1 and 2. That is, either moving away from dynamic versioning or changing how lock files work for local packages. 

However, a simpler resolution might be via step 3? If uv were to allow users to check that lock files are up to date apart from the versions of any current workspace packages that have dynamic versions then the current workflow would be able to continue. Effectively this returns to the workflow of the pre 0.5.0 uv, while still correctly updating the lock file (and re-installing the package). I would expect this would catch most of the current use cases for `uv lock --frozen` 

This could work by adding an option to `uv lock --frozen` etc to allow versions of local packages with dynamic versioning to change but to error if any other dependencies change. 


---

_Comment by @charliermarsh on 2024-11-18 13:48_

I think the solution here will ultimately be to stop storing versions for source trees (either entirely, or when dynamic -- I prefer doing so entirely, and not based on whether it's dynamic). I just want PEP 751 to play out a little more before I make any change.

---

_Comment by @charliermarsh on 2024-11-18 13:50_

(Once we have clarity on what we want to do, I'll prioritize the change.)

---

_Comment by @nathanjmcdougall on 2024-11-19 03:46_

As a temporary kind of hacky semi-workaround, I am using `uv export` via [uv-pre-commit](https://github.com/astral-sh/uv-pre-commit). Changes to the dependencies in `pyproject.toml` get reflected in a changed `requirements.txt` file, failing the hook. Since the `requirements.txt` file doesn't include the version of the editable-installed package, it doesn't suffer from the same issue as checking against `uv.lock` directly. 

In theory, the user could still get around this by modifying both the `pyproject.toml` and `requirements.txt` files but not the `uv.lock` file, since the hook is not checking the lockfile directly. I don't really think that would be possible in most cases without creating a `requirements.txt` file that mismatches what would get created by `uv export` so it would still fail, forcing a lockfile update.

It protects against a situation where a dependency gets declared in `pyproject.toml` but the user forgets to run `uv sync` or `uv lock` - the pre-commit would fail, and the lockfile would be updated as a side-effect. But if the user is using `uv add` then this adds little benefit since it's unlikely the user would declare in `pyproject.toml` manually in the first place.

My plan is to get my organization to stop using dynamic versioning in packages...

---

_Referenced in [astral-sh/uv#9233](../../astral-sh/uv/issues/9233.md) on 2024-11-19 16:28_

---

_Comment by @mjpieters on 2024-11-19 16:48_

We use a [`Taskfile.yml` task definition](https://taskfile.dev/), so I have a _little_ more flexibility, and I use it to apply an ugly work-around: When we update the lock I force the version to be `0.0.0` by setting `SETUPTOOLS_SCM_PRETEND_VERSION` (which works for either `hatch-vcs` or `setuptools-scm`):

```
  dev:lock:
    aliases:
      - lock
    desc: Lock dependencies
    preconditions:
      - sh: type "uv" &>/dev/null
        msg: Please install uv, see https://docs.astral.sh/uv/getting-started/installation/
    sources:
      - pyproject.toml
    generates:
      - pyproject.toml
      - uv.lock
    env:
      # avoid locking with a dynamic (hatch-vcs) version.
      SETUPTOOLS_SCM_PRETEND_VERSION: 0.0.0
    cmds:
      - uv lock
```

and then, when linting:

```
  dev:lint:
    aliases:
      - lint
    desc: Runs linters
    preconditions:
      - sh: type "uv" &>/dev/null
        msg: Please install uv, see https://docs.astral.sh/uv/getting-started/installation/
    cmds:
      - |
        SETUPTOOLS_SCM_PRETEND_VERSION=0.0.0 uv lock --locked 2>/dev/null || {
          echo -e '\033[0;31mThe lockfile at `uv.lock` needs to be updated. To update the lockfile, run `task dev:lock`\033[0m'.
          exit 1
        } >&2
```

This lets us at least avoid recording the current dynamic version in the lock file.

---

_Referenced in [zanieb/uv#6](../../zanieb/uv/issues/6.md) on 2024-11-26 17:33_

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2024-11-26 21:15_

---

_Comment by @thcrt on 2024-12-11 00:02_

Just checking- is there any way to use dynamic versioning and build with GitHub Actions while also including `uv.lock`in version control? 

I haven't been able to get it working, and I'm not  sure whether this is a problem with [actions/checkout](https://github.com/actions/checkout), with [hatch-vcs](https://github.com/ofek/hatch-vcs), with [setuptools-scm](https://github.com/pypa/setuptools-scm) or with uv.

I can run `uv build` on my own machine just fine, and when I do this after having tagged the latest commit, the resulting builds have the version of the tag. However, I've found it impossible to get the same thing to happen via a workflow. The `uv sync` step *always* seems to set the version to include a [developmental release segment](https://packaging.python.org/en/latest/specifications/version-specifiers/#developmental-releases) and a [local version identifier](https://packaging.python.org/en/latest/specifications/version-specifiers/#local-version-identifiers), such as `0.2.2.dev0+g38cf6c2.d20241210`.

The only way that I can find to prevent this from happening is removing `uv.lock` from version control. Obviously, this isn't ideal.

I'd love to know if there's something I'm missing, and it's possible to use `hatch-vcs`, `uv` and GitHub Actions workflows together. If this is an issue with one of the other projects involved, let me know and I'll report it to them :)

---

_Comment by @nathanjmcdougall on 2024-12-11 02:22_

@thcrt This probably isn't exactly what you're looking for, but I've found this works:

https://gist.github.com/nathanjmcdougall/42135563c719cfda59a1de317da5116b

Note that this deliberately avoids running `uv sync`, to avoid the issue. I'm using `hatch-vcs`.

---

_Comment by @thcrt on 2024-12-11 11:17_

@nathanjmcdougall Incredible. You are a *saint* (and a fellow Kiwi!)

Genuinely, thank you so much, this has been a major headache for me in two different projects. 

I imagine that not running `uv sync` in CI may have its downsides, but I'm happy to run it locally and manually if that's the tradeoff for having builds work properly.

Thank you so much again. 

---

_Comment by @roteiro on 2024-12-12 06:54_

Since [PEP 751](https://discuss.python.org/t/pep-751-now-with-graphs/69721) and therefore a holistic solution within uv looks like it will take some more time, here is my work-around to avoid the `uv sync` <-> `git commit` loop of `uv.lock`, aimed at local development of an app `myapp`:

Instead of running `uv sync` or e.g. `uv run myentrypoint` which calls `uv sync` implicitely, I do the following:
```
uv sync
sed -i -e '/myapp/ {' -e 'n; s/.*/version = \"0.0+dynamic\"/' -e '}' uv.lock # replace next line in uv.lock with dynamic version if previous line matches name of our package. Taken from: https://stackoverflow.com/questions/18620153/find-matching-text-and-replace-next-line/18620241#comment76950756_18620241:
uv run --frozen myentrypoint
```

I take care to only commit the modified `uv.lock` with `version = 0.0+dynamic`. I'm sure that this approach could be integrated into a pre-commit rule as well, but haven't looked into it yet. Of course, this can also be integrated into the task runner of your choice, I use [task](https://taskfile.dev/) and my `Taskfile.yml` has something like:
```yaml
tasks:
  dynamic-version:
    desc: replace next line in uv.lock with dynamic version if previous line matches name of our package, see  https://stackoverflow.com/questions/18620153/find-matching-text-and-replace-next-line/18620241#comment76950756_18620241
    cmds:
      - sed -i -e '/myapp/ {' -e 'n; s/.*/version = \"0.0+dynamic\"/' -e '}' uv.lock
     
  sync:
    desc: runs a uv sync followed by resetting the package version to dynamic
    cmds:
      - uv sync
      - task: dynamic-version
      
  run:
    desc: runs myapp
    cmds:
        - task: sync
        - uv run --frozen myentrypoint


```
For the moment, this serves me well. If there are any unintentional side-effects, I'm eager to learn about them 



---

_Referenced in [astral-sh/uv#5633](../../astral-sh/uv/issues/5633.md) on 2024-12-12 14:34_

---

_Referenced in [astral-sh/uv#9874](../../astral-sh/uv/issues/9874.md) on 2024-12-13 17:51_

---

_Comment by @superlopuh on 2024-12-13 19:28_

Would it be possible to allow for this behaviour?

```
uv sync --locked # Fails due to dynamic version mismatch
uv sync --locked --allow-editable # Succeeds
```

---

_Comment by @ayjayt on 2024-12-16 03:34_

from @charliermarsh: 
> But there's sort of an infinite loop here, right? Since every time you commit the lockfile, it will be outdated. So you then re-lock, and commit again, etc. I don't know how to solve this holistically. We could omit versions for editables, maybe? Or even, write "dynamic" or something for the version, for packages that have dynamic versions?

Narrowing things down, this specific issue is only when you are trying to pin the version of the repository you are currently working in. No other dependency matters, editable, dynamic, or not, since `uv.lock` updates won't cause the infinite loop if they don't effect those repos (so I'll ignore whether or not its helpful to try and lock a `-e`).

Furthermore, you don't *need* to mark the version in `uv.lock` of the current repository, theoretically, because the `uv.lock` will be checked into git against that version anyway.

So I propose something like

`version = "self"` if the `uv.lock` is storing metadata for the same project from which a `pyproject.toml` is in that same directory.

### Side Note: Further Spooky Behavior (extra notes I'm writing for if I come back to this convo)
 nevermind about side note, its just https://github.com/astral-sh/uv/issues/6860 + not knowing when `uv lock --upgrade` runs or doesn't

---

_Comment by @JacobHayes on 2024-12-16 04:20_

If you're using `hatch-vcs`, I've been working around this by setting `SETUPTOOLS_SCM_PRETEND_VERSION` as documented [here](https://github.com/ofek/hatch-vcs#version-source-environment-variables).

Eg, I've added this to my `.envrc` (for [direnv](https://direnv.net)):

```sh
# `uv` pins the version in the `uv.lock` file, but this leads to churn with each commit when using
# `hatch-vcs`. Instead, we can override the version to force stability until [1] is resolved.
#
# 1: https://github.com/astral-sh/uv/issues/7533
export SETUPTOOLS_SCM_PRETEND_VERSION="0.1.0"
```

and then all `uv sync` commands I run _locally_ build a wheel with that version, meaning the `uv.lock` is stable. I've tested adding a dependency to `project.dependencies` and a subsequent `uv sync` detects that and adds it to the `uv.lock`.

Seems to be working fine, but I'm not sure what other ramifications it might have - at the least, it probably doesn't play as well with projects with non-python (ie: built) dependencies.

---

edit: ah, I see `SETUPTOOLS_SCM_PRETEND_VERSION` was [noted above too](https://github.com/astral-sh/uv/issues/7533#issuecomment-2486235749).

---

_Referenced in [xdslproject/xdsl#3639](../../xdslproject/xdsl/pulls/3639.md) on 2024-12-16 08:26_

---

_Referenced in [urllib3/urllib3#3530](../../urllib3/urllib3/pulls/3530.md) on 2024-12-16 18:09_

---

_Comment by @sciyoshi on 2024-12-19 16:41_

Note that neither setting [package = false in [tool.uv]](https://docs.astral.sh/uv/reference/settings/#package) nor using --no-install-project addresses the issue, because it seems like the project is still included in uv.lock regardless.

---

_Comment by @brandon-avantus on 2024-12-19 17:36_

Here's a temporary fix for those using `hatch-vcs` that doesn't require setting environment variables in your development environment, unless you want to override the version, such as when rolling a distribution.

Copy the following code block, paste it into a file named `hatch_build.py`, and save it next to `pyproject.toml`. Then add `[tool.hatch.metadata.hooks.custom]` to `pyproject.toml`.

```python
# hatch_build.py

"""Hatchling custom metadata hook.

Temporary fix until Astral decides how to handle dynamic project versions
in the uv.lock file.

See https://github.com/astral-sh/uv/issues/7533

To enable, place this file next to and add the following line to the
pyproject.toml file:

    [tool.hatch.metadata.hooks.custom]

To override, set SETUPTOOLS_SCM_PRETEND_VERSION environment variable to the
desired version before building, or set CI=1 (already set in GitHub actions)
to let hatch-vcs (using setuptools-scm) determine the version from the git tag.
"""

import os

from hatchling.metadata.plugin.interface import MetadataHookInterface

if not os.environ.get("SETUPTOOLS_SCM_PRETEND_VERSION") and not os.environ.get("CI"):
    os.environ["SETUPTOOLS_SCM_PRETEND_VERSION"] = "0.dev0+local"

class CustomMetadataHook(MetadataHookInterface):
    def update(self, metadata: dict) -> None:
        pass
```

This uses the `hatchling` hook system to set the `SETUPTOOLS_SCM_PRETEND_VERSION` environment variable automatically during the build process, but only if the variable isn't already set and the `CI` environment variable isn't set, such as when building with GitHub actions.

Of course, feel free to set the default version to something that fits your project.

---

_Referenced in [astral-sh/uv#10054](../../astral-sh/uv/issues/10054.md) on 2024-12-20 13:47_

---

_Referenced in [rst2pdf/rst2pdf#1256](../../rst2pdf/rst2pdf/pulls/1256.md) on 2024-12-24 16:14_

---

_Referenced in [astral-sh/uv#10167](../../astral-sh/uv/issues/10167.md) on 2024-12-26 14:36_

---

_Referenced in [googleapis/release-please#2455](../../googleapis/release-please/issues/2455.md) on 2024-12-31 00:05_

---

_Comment by @ofek on 2025-01-08 16:40_

The solution [here](https://github.com/astral-sh/uv/issues/7533#issuecomment-2486235749) is what should be preferred for `hatch-vcs`, `setuptools-scm`, etc. Basically, only temporarily set the `SETUPTOOLS_SCM_PRETEND_VERSION` environment variable when running `uv lock`.

---

_Referenced in [astral-sh/uv#10559](../../astral-sh/uv/issues/10559.md) on 2025-01-13 09:32_

---

_Referenced in [astropy/astropy#17596](../../astropy/astropy/issues/17596.md) on 2025-01-13 09:59_

---

_Referenced in [PlasmaPy/PlasmaPy#2940](../../PlasmaPy/PlasmaPy/issues/2940.md) on 2025-01-14 23:24_

---

_Referenced in [astral-sh/uv#10622](../../astral-sh/uv/pulls/10622.md) on 2025-01-15 02:23_

---

_Comment by @charliermarsh on 2025-01-15 02:38_

I'm working on omitting dynamic versions from the lockfile: https://github.com/astral-sh/uv/pull/10622

---

_Closed by @charliermarsh on 2025-01-15 16:54_

---

_Closed by @charliermarsh on 2025-01-15 16:54_

---

_Referenced in [devminds-ch/project-python#15](../../devminds-ch/project-python/pulls/15.md) on 2025-01-15 18:04_

---

_Comment by @namurphy on 2025-01-15 22:01_

Many thanks for making this change and including it in `v0.15.9`!  🎉🎂💖🎆🪩🌌🥦

I just recreated `uv.lock` for a project with a dynamic version, and can confirm that the resulting `uv.lock` no longer contains the dynamic version for the project.  I deleted the prior `uv.lock` before running `uv lock` with `v0.15.9`, just to be safe.  

---

_Referenced in [urllib3/urllib3#3550](../../urllib3/urllib3/pulls/3550.md) on 2025-01-15 23:56_

---

_Referenced in [django-commons/drf-excel#110](../../django-commons/drf-excel/pulls/110.md) on 2025-01-19 13:36_

---

_Comment by @brendan-morin on 2025-01-28 21:06_

Perhaps I am misunderstanding the fix action, using this improperly, or this is a `hatch-vcs` problem, but are we sure this is working as intended? It seems that the behavior is inconsistent and flip flops depending on the state of uv's locking/my venv. Please let me know if this is better suited for a separate issue thread.

```
uv --version
uv 0.5.24 (Homebrew 2025-01-23)
```

I have a `pyproject.toml` like this:

```pyproject.toml
[project]
dynamic = ["version"]

[build-system]
requires = ["hatchling", "hatch-vcs"]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "vcs"
```

As a caveat, I experienced a huge amount of inconsistency in reproduction (generally almost always ending in a state where the version _was_ included in the `uv.lock` file), so I am not 100% confident these steps will repro for others. But this is what I used to repro my observed behavior.
1. delete `uv.lock` + `.venv`
2. run `uv lock`. This initially works as expected, no dynamic version in `uv.lock`
3. run `uv sync`. Still fine, no dynamic version in `uv.lock`. But we see the package has been built according to the logs:
```
$ uv sync
Resolved 108 packages in 1ms
   Built mypackage @ file:///...
Prepared 2 packages in 1.55s
Uninstalled 2 packages in 57ms
Installed 2 packages in 9ms
 ...
 ~ mypackage==5.1.29.dev1+gdd9890e.d20250128 (from file:///...)
```
4. run `uv lock` again. Now we see these new logs:
```
$ uv lock
Resolved 108 packages in 30ms
Updated mypackage (dynamic) -> v5.1.29.dev1+gdd9890e.d20250128
```
And the dynamic version is present in the lock file:
```
[[package]]
name = "mypackage"
version = "5.1.29.dev1+gdd9890e.d20250128"
```
5. Now no matter how many times I delete uv.lock, venv, run `uv lock`, or `uv sync`, the dynamic version remains. There's probably other ways, but if I want to reset the process, I can add this to pyproject.toml and rerun `uv lock`:
```pyproject.toml
[tool.hatch.build.hooks.vcs]
version-file = "_version.py" 
```
and `uv lock` yields:
```
uv lock
Resolved 108 packages in 1m 42s
Updated mypackage v5.1.29.dev1+gdd9890e.d20250128 -> (dynamic)
```
6. Repeat the process by running `uv sync` and `uv lock` again:
```
$ uv sync            
Resolved 108 packages in 1ms
   Built mypackage @ file:///...
Prepared 1 package in 945ms
Uninstalled 1 package in 0.58ms
Installed 1 package in 1ms
 ~ mypacakge==5.1.29.dev1+gdd9890e.d20250128 (from file:///...)
$ uv lock
Resolved 108 packages in 3.25s
Updated mypackage (dynamic) -> v5.1.29.dev1+gdd9890e.d20250128
```

---

_Comment by @charliermarsh on 2025-01-28 21:13_

That seems unusual. Are you able to put together a project that I can use to reproduce?

---

_Comment by @apollo13 on 2025-01-28 21:22_

I am seeing the same but having a hard time reproducing why this is happening. In my case it is enough to remove `uv.lock` and run `uv lock` again. Any debug logs that might have anything helpful?

---

_Comment by @apollo13 on 2025-01-28 21:29_

Okay, `uv lock -vvv` showed stuf like:
```
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=oversight @ file:///home/florian/dev/projectx/oversight
    0.014659s   0ms DEBUG uv_distribution::source No static `pyproject.toml` available for: oversight @ file:///home/florian/dev/projectx/oversight (PyprojectToml(DynamicField("version")))
    0.015007s DEBUG uv_fs Acquired lock for `/home/florian/.cache/uv/sdists-v6/editable/5bff42cdde91dde7`
    0.015906s   1ms DEBUG uv_distribution::source Using cached metadata for: oversight @ file:///home/florian/dev/projectx/oversight
    0.016398s   2ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.016577s   2ms DEBUG uv_fs Released lock at `/home/florian/.cache/uv/sdists-v6/editable/5bff42cdde91dde7/.lock`
 uv_resolver::resolver::solve 
    0.018200s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.13.1
    0.018216s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.13.0, <3.14
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.018550s   0ms DEBUG uv_resolver::resolver Adding direct dependency: oversight[container]*
    0.018629s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of oversight @ file:///home/florian/dev/projectx/oversight (*)
   uv_resolver::resolver::get_dependencies_forking package=oversight[container], version=0.4.1.dev31+ga536f6c.d20250128
     uv_resolver::resolver::get_dependencies package=oversight[container], version=0.4.1.dev31+ga536f6c.d20250128
    0.018700s   0ms DEBUG uv_resolver::resolver Adding direct dependency: oversight==0.4.1.dev31+ga536f6c.d20250128
    0.018724s   0ms DEBUG uv_resolver::resolver Adding direct dependency: oversight[container]==0.4.1.dev31+ga536f6c.d20250128
    0.018761s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of oversight @ file:///home/florian/dev/projectx/oversight (==0.4.1.dev31+ga536f6c.d20250128)
   uv_resolver::resolver::get_dependencies_forking package=oversight, version=0.4.1.dev31+ga536f6c.d20250128
     uv_resolver::resolver::get_dependencies package=oversight, version=0.4.1.dev31+ga536f6c.d20250128
    0.018960s   0ms DEBUG uv_resolver::resolver Adding direct dependency: dj-database-url>=2.2.0
```

After running a `uv cache clean` `uv lock` no longer seems to write the version into `uv.lock`.  Now running `uv sync` and removing `uv.lock` and rerunning `uv lock` again writes it back. 

So I wonder if this depends on the cache?

---

_Comment by @nathanjmcdougall on 2025-01-28 21:32_

I'm having a similar issue. To provide another data point, I have a project where:

1. Using the project created pre-v0.15.9 will fail to use remove the version from the lockfile, even when deleting the lockfile and .venv.
2. A fresh clone and running `uv lock` will remove the version from the lock file no problem.

So a cache issue seems likely to me.

---

_Comment by @neutrinoceros on 2025-01-28 21:36_

@Brendan-Morin Maybe a bit random but I noticed that your `[project]` table is lacking a `requires-python` entry, which seemed to be crucial in my encounter with a similar problem.

xref https://github.com/astral-sh/uv-pre-commit/issues/35

---

_Comment by @charliermarsh on 2025-01-28 21:38_

Is anyone able to upload a project that I can use to reproduce this? I'd love to fix it. I'm happy to fix it tonight.

---

_Comment by @apollo13 on 2025-01-28 21:43_

@charliermarsh Use this repo: https://github.com/apollo13/uv-bug

Follow this to the letter (I am not sure if you can repro if you do anything different):

 * `uv cache clean`
 * `uv lock` and observe that `git status` shows no changes
 * `uv sync`
 * `rm -rf uv.lock .venv`
 * `uv lock`
 * Run `git diff` and see the version written into the lock file (shouldn't be)
 * `uv cache clean` 
 * `rm -rf uv.lock .venv`
 * `uv lock` (No version written -> okay)

---

_Comment by @charliermarsh on 2025-01-28 21:44_

Thanks!

---

_Referenced in [astral-sh/uv#11047](../../astral-sh/uv/issues/11047.md) on 2025-01-29 01:12_

---

_Comment by @charliermarsh on 2025-01-29 01:14_

Thank you, this was a real bug. Fix here: https://github.com/astral-sh/uv/pull/11046.

---

_Comment by @brendan-morin on 2025-01-29 01:22_

Awesome, thank you for the quick repro @apollo13  and @charliermarsh for the quick fix!

---

_Referenced in [bihealth/cubi-tk#255](../../bihealth/cubi-tk/pulls/255.md) on 2025-02-03 13:06_

---

_Referenced in [prefix-dev/pixi#2512](../../prefix-dev/pixi/issues/2512.md) on 2025-04-24 09:15_

---

_Referenced in [nathanjmcdougall/postmodern-python-ci#1](../../nathanjmcdougall/postmodern-python-ci/issues/1.md) on 2025-05-08 09:47_

---

_Referenced in [prefix-dev/pixi#1274](../../prefix-dev/pixi/issues/1274.md) on 2025-05-28 19:02_

---

_Comment by @aqib-bhat on 2025-06-15 11:15_

I was experiencing the `uv.lock` going out of sync when `python-semantic-release` would bump the version in `pyproject.toml`. I fixed this by:
- Adding a step after the semantic release that updates the version of my project in the `uv.lock` file by running: `uv lock --upgrade-package <project_name>` followed by committing and pushing the change.
  - Link to my GitHub workflow file: https://github.com/aqib-oss/sonar-qube-gh-action/blob/main/.github/workflows/main-workflow.yaml#L74
  - Thankfully, this does not trigger another run of the `main` branch workflow, however, if it did, it can easily be handled by adding a pre-check job that determines if the last commit was for syncing the project version in the `uv.lock` file, and the main job will run only if the output from the pre-check job is `true`.

---

_Referenced in [astral-sh/uv#14137](../../astral-sh/uv/issues/14137.md) on 2025-06-20 17:30_

---

_Comment by @aqib-bhat on 2025-06-23 03:51_

I created an issue in the `python-semantic-release` repo as well because their uv integration guide only ensures that the project version is synced just before the semantic release action runs in the merge to main workflow. Other than that, for most of the time, the `uv.ock` file will remain out of sync.
https://github.com/python-semantic-release/python-semantic-release/issues/1280

---

_Label `needs-design` removed by @konstin on 2025-06-23 09:47_

---

_Label `needs-decision` removed by @konstin on 2025-06-23 09:47_

---

_Comment by @konstin on 2025-06-23 09:49_

A plain `uv lock` should get the `uv.lock` synced with the bumped version. Note that there's also `uv version --bump`, which does also update `uv.lock`.

---

_Referenced in [LEB-EPFL/just-focus#8](../../LEB-EPFL/just-focus/pulls/8.md) on 2025-08-05 08:16_

---
