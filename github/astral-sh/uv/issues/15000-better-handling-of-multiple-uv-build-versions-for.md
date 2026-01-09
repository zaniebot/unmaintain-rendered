---
number: 15000
title: Better handling of multiple uv-build versions for repackagers / distros
type: issue
state: open
author: geofft
labels: []
assignees: []
created_at: 2025-07-31T16:40:12Z
updated_at: 2025-08-15T20:29:22Z
url: https://github.com/astral-sh/uv/issues/15000
synced_at: 2026-01-07T13:12:19-06:00
---

# Better handling of multiple uv-build versions for repackagers / distros

---

_Issue opened by @geofft on 2025-07-31 16:40_

We recommend adding an upper bound to your `uv_build` dependency in `build-system.requires`: https://docs.astral.sh/uv/concepts/build-backend/#using-the-uv-build-backend This is a generally good thing to do: setting an upper bound means that if there's a backwards-incompatible change in uv_build 0.9.0, people who release code today, using uv_build 0.8.x, will not be broken.

This means that we expect to see in the wild some (older / less frequently updated) packages with a build-dependency on uv_build 0.8.x, and newer packages with a build dependency on uv_build 0.9.x. These cannot be co-installed. Python uses a single flat import namespace, so in any given Python environment, `import uv_build; uv_build.build_wheel()` can only find one of them.

For users using uv, pip, `python -m build`, etc., this is not a problem, because builds are isolated, meaning build dependencies are installed into an ad-hoc virtual environment for that one build. So it doesn't matter if two packages have conflicting build dependencies.

There are other use cases where this is much more annoying, for instance:
* The average Linux distro avoids using wheels/virtual environments. Packages are installed globally into e.g. /usr/lib/python3.x/site-packages. Builds happen in an environment that isn't isolated at the Python level; the backend is imported out of the standard, systemwide `sys.path`. Because the PEP 517 interface only specifies how to build wheels, my impression is the usual thing to do is to invoke `build_wheel()` and then immediately install/unpack the resulting wheel and discard it. While the builds are generally isolated at the level of _distro packages_ in the sense that official builds are done in minimal chroots/containers/etc., it's cumbersome to do interactive development work inside chroots as opposed to on your normal developer system. But if you have two packages with conflicting build dependencies, you can't install both sets of build dependencies.
* A corporate monorepo I'm familiar with supports vendoring 3rd-party Python packages. This predated PEP 517 and so the build tooling for them originally just set up PYTHONPATH correctly (i.e., pointing to other directories in the monorepo that were already built) and invoked `python setup.py`. Now I believe it uses `python -m build --no-isolation` and unpacks the resulting wheel.

In both these cases you can argue that the repackager is doing it wrong, and ought to set up isolated builds. But fixing this is a pretty disruptive change. (For instance, for the corporate monorepo, I believe the first attempt at PEP 517 support used isolated builds, but setting up virtualenvs made that portion of the build orders of magnitude slower, because for pure-Python packages, actually doing the build involves approximately no work. It also required us to keep wheels around, which we were discarding for PEP 517 projects and not even building for legacy projects. We also tried to avoid pulling in build dependencies of build dependencies onto the PYTHONPATH used during builds, but that broke other assumptions.)

Is there something we can do in `uv_build` that makes this less painful for repackagers without causing too much maintenance burden?

I want to be clear, first off, that I expect `uv_build`'s backwards compatibility story to be similar to uv's: semantic versioning/upper bounds are done out of an abundance of caution, and we expect the behavior in newer versions to be better and easy to migrate to. (See the [uv 0.8.0 changelog](https://github.com/astral-sh/uv/releases/tag/0.8.0) for a good example of the sort of things that are considered breaking.) So, 99% of the time, the correct response for a repackager encountering a conflict is to submit a PR upstream bumping the older project's `uv_build` version, or carry a patch, and most of the time, that PR/patch needs to do nothing other than bumping the version. All of this is to address the unlikely but possible cases where making those changes is hard.

A couple of thoughts for brainstorming:
* If `uv_build` were versioned in its own name, e.g., if you had `uv_build_0_8` and `uv_build_0_9`, then you could co-install these packages just fine and import the one you need. But I think that's a pretty distasteful cost to impose on the ecosystem as a whole for the combination of an unlikely scenario + a small class of affected users.
* Would it help to commit to an API promise that `uv_build` _can_ be renamed and still work fine, even if we don't officially release renamed versions? Then a downstream packager can fork the packaging of an old `uv_build`, rename it to `uv_build_0_8`, and patch the (few) packages that cannot be upgraded to do `build-system.requires = ["uv_build_0_8"]`, and that can be coinstalled with the regular `uv_build`.
* Do packagers typically strictly interact with the PEP 517 abstraction, or are there special cases for known build backends? The `uv_build` backend is just a wrapper around invoking the `uv-build build-wheel` command, and it sounds like that CLI interface could be a formal part of the public API from uv. Is it enough to be able to ship a `uv-build-0.8` binary on the system somewhere, and then have the Python packaging helper invoke that instead of doing any Python imports at all? (A bonus of this approach is it would speed up distro builds by avoiding running Python just to run a subprocess.)
* One thing to think about: breaking changes in `uv_build` are probably rarer than in uv itself. Would it help to version them differently? Maybe `uv_build` has a major number of its API stability and a minor number of the corresponding uv version, e.g., `1.0.8.0`, and so users would specify `build-system.requires = ["uv_build>=1.0.8.4,<2"]`? I don't know if this causes problems for uv itself detecting a package using a compatible `uv_build` version.

There are probably other approaches I'm not thinking of yet. 

cc @konstin, @zanieb, and a few folks from the discussion in https://github.com/pypa/packaging.python.org/pull/1880#discussion_r2217670477 - @hroncok @mgorny @musicinmybrain 

---

_Referenced in [pypa/packaging.python.org#1880](../../pypa/packaging.python.org/pulls/1880.md) on 2025-07-31 16:41_

---

_Comment by @musicinmybrain on 2025-07-31 18:21_

> While the builds are generally isolated at the level of distro packages in the sense that official builds are done in minimal chroots/containers/etc., it's cumbersome to do interactive development work inside chroots as opposed to on your normal developer system. But if you have two packages with conflicting build dependencies, you can't install both sets of build dependencies.

In Fedora, you can assume packagers are developing and testing packages in isolated chroots: the only reasonable way to do local test builds of Fedora packages is with https://github.com/rpm-software-management/mock. Therefore, conflicting packages are not necessarily a problem as long as they are only ever used as build-time dependencies. They *can* become a problem if developer tools start growing runtime dependencies on things that everyone originally thought would always only be build-time dependencies.

For us, the bigger obstacle to having multiple “compat” versions of something like `uv-build` in Fedora is just the additional burden of maintaining them. As @hroncok notes in https://github.com/pypa/packaging.python.org/pull/1880#discussion_r2243681997, each different version that is packaged brings some additional work in testing, patching, and otherwise keeping the package buildable and testable.

Since we use system packaged Rust libraries, keeping around old versions of things written in Rust also tends to increase the number of dependencies on old versions of Rust libraries. This either means more downstream patching and validation work to allow newer, SemVer-incompatible versions of Rust dependencies, or it increases the number of Rust compat packages that we need to maintain. As I noted in https://github.com/pypa/packaging.python.org/pull/1880#discussion_r2243379395, we use Rust compat packages freely when they are needed, and they can be created with little friction. However, we are always looking for ways to reduce the number of Rust compat packages needed in the distribution, because maintaining and testing them isn’t “free” either, and the number of compat packages can easily spiral out of control if we don’t actively work to avoid and eventually remove them.

In general, when it’s practical and we have a choice, we tend to find that our time is better spent helping upstreams port to new releases of their dependencies rather than maintaining old versions of things and waiting for upstreams to move on their own.

----

Regarding versioned names for `uv_build`, like `uv_build_0_8`, it seems this would do more harm than good. We would still need to maintain multiple versioned packages, and now we would also need to patch all dependent packages to use a versioned build backend name. If we have to patch all dependent packages, I would rather try to patch them to use the new release (or remove the upper bound and rely on build-time testing to reveal any regressions) than patch them to use a versioned name for an old release.

For conflicting compat packages, no patching of dependent packages is required, because the version bounds are reflected in the RPM dependency, so the highest-versioned package that provides `python3dist(uv-build)` and is consistent with the version bounds is the one that gets installed. In this respect, versioned names would be a big step backwards in maintenance burden from the status quo.

This approach would also appear to require a new PyPI project for each release. In general, versioned compat packages don’t need to be re-reviewed in Fedora, but a package like `python-uv-build-0-8` providing `python3dist(uv-build-0-8)` and corresponding to a new Python package name might need to be re-reviewed.

The *only* benefit I can think of for renaming like this is would be that multiple “compat” versions could be parallel-installable, which might be useful if `uv-build` ever becomes a runtime dependency of anything (but could perhaps still be a bit of a minefield in unanticipated ways).

----

> Do packagers typically strictly interact with the PEP 517 abstraction, or are there special cases for known build backends?

@hroncok is the expert here, but for Fedora, it’s the former: https://src.fedoraproject.org/rpms/pyproject-rpm-macros is not aware of particular build backends.

----

> One thing to think about: breaking changes in uv_build are probably rarer than in uv itself. Would it help to version them differently?

Assuming this is reasonable in a broader sense, and assuming it does reduce the frequency of SemVer breaks in `uv_build`, it seems like this would in fact make things easier in Fedora. We already have `uv` and `python-uv-build` as separate source packages, and I already anticipate that their versions may not be synchronized in stable releases. (As a developer tool, it can make sense to offer the latest `uv` in Fedora even if that means an update contains technically-breaking changes, and `uv` has been granted an exception to the general prohibition on incompatible updates with that rationale. On the other hand, there’s little incentive to update `python-uv-build` across SemVer boundaries in a stable release since it’s primarily a build dependency for other packages. Developers using Fedora’s `uv` will generally be working with venvs, not using Fedora’s packaged `uv_build`.)

Other distribution packagers may have different perspectives. Considering https://github.com/astral-sh/uv/issues/12389, it seems like diverging the versions *might* make @mgorny’s life harder.

---

_Comment by @geofft on 2025-07-31 20:16_

Thanks, this is all very helpful.

> In general, when it’s practical and we have a choice, we tend to find that our time is better spent helping upstreams port to new releases of their dependencies rather than maintaining old versions of things and waiting for upstreams to move on their own. [...] If we have to patch all dependent packages, I would rather try to patch them to use the new release (or remove the upper bound and rely on build-time testing to reveal any regressions) than patch them to use a versioned name for an old release.

OK, cool, that's what I was hoping would be the case! My sense here is that we're trying to figure out a palatable worst-case option _if_ you cannot patch an upstream package to build with a newer build dependency. So (as I understand it) the fact that you can make the compat package if necessary is enough, even if it's cumbersome—everyone shares the goal that this should never be necessary and that it should be easy to bump a package to use the latest `uv_build`.

Again, going off the [uv 0.8.0 release notes](https://github.com/astral-sh/uv/releases/tag/0.8.0) as an example - most things have a knob that's listed for how to get the old behavior, or some sort of description of what change to make. I expect build-backend stuff to work the same way.

> Since we use system packaged Rust libraries, keeping around old versions of things written in Rust also tends to increase the number of dependencies on old versions of Rust libraries. This either means more downstream patching and validation work to allow newer, SemVer-incompatible versions of Rust dependencies, or it increases the number of Rust compat packages that we need to maintain.

A very good point, thanks!

---

_Comment by @mgorny on 2025-08-01 09:27_

Okay. let me put the Gentoo perspective here.

-------------

First of all, we have a strong preference of building everything from sources. This is pretty similar to Debian or Fedora — using sources rather than binaries, supply chain security, license transparency, blah blah blah. That said, this is rather irrelevant to pure Python packages, since technically the wheel has just the same `.py` source files as the source package, so we could use it instead. Nevertheless, that means we effectively end up having o maintain two different workflows for Python packages, which is some overhead.

Furthermore, there are other reasons to use source packages: tests are rarely included in wheels (and we do want to run tests), patches are easier to apply, we can benefit from build system fixes (e.g. if the wheel has broken metadata). These things can be worked around, but again at a cost.

Gentoo is a bit special here, though, since we are a source-first distribution, which means the majority of our users will actually end up building packages from source. So if we hit any issues, it's insufficient that we devise a "local" solution that lets us build a binary package in some setup; we need a "global" solution that will also work well on end user systems.

-----------

Secondly, our builds are done against system packages and non-isolated. The former is not uncommon — again, it's the matter of supply chain security that we don't fetch random packages from the Internet, but use what we have. Our builds are entirely disconnected from the Internet. The latter is more of an implementation detail, partially related to the fact that binary packages are not a part of the regular workflow, and getting an isolated environment from system packages is nontrivial, and probably can fail in nontrivial ways, in some scenarios. Also note that we are aggressively unbundling everything, so our build backends are not really standalone.

On top of that, since source builds are part of the whole package installation process, i.e. there is no really separate "build binary package, then install it" pipeline, the package manager needs to be able to come up with a consistent dependency graph that works for building all packages involved. In other words, if we end up requiring two different `uv-build` versions for two different packages, then users may end up hitting an "impossible scenario" when they have to install two versions of `uv-build` simultaneously. Of course, this is often something that can be worked around by installing the packages separately, but these kind of things tend to break automated workflows hard.

Some of these things are a matter of implementation details, you could even consider them bugs. However, changing them requires a lot of effort, and comes with risk of introducing more bugs and making things even less maintainable, as complexity increases. At this point, the benefits did not seem to outweigh the costs of changing things, and even if they were, it's not clear if anyone would actually be able to dedicate enough time to do that. On top of that, we have a strong backwards compatibility policy for the package managers, so even if we were to change things, it will take a few years before we'd able to rely on them.

----------------

Now, upper bounds per se are not a problem for us, because our build process simply ignores `build-system.requires` list. We need to state the build dependencies in the package metadata anyway, and our package manager ensures that they are installed, so there doesn't seem to be any reason to check them again. Plus, far too often they end up being incorrect, and there's really no point in patching a lot of `pyproject.toml` files just to ensure that dependency check doesn't misfire. Still, I do recognize that they can be problems for others, and that they unfortunately encourage upstreams to rely on pinning old versions rather than fix bugs / update their code (but then, permanent breakage for end users is not a solution either).

What would be a problem for us are the breaking changes themselves. As noted above, we can't really rely on packages requiring multiple `uv-build` versions simultaneously. On the plus side, unlike PyPI publishers, we can proactively patch things in our packages to fix incompatibilities.

Plus, as long as `uv-build` follows the common standards and practices, and upstreams don't rely on exclusive `uv-build` features, we can also easily override the build system to use `flit-core`, `hatchling` or anything else that achieves the same result.

---

_Comment by @hroncok on 2025-08-15 17:26_

As anecdata, we currently only have 2 packages in Fedora using uv-build, and one of them already needed patching because it pinned `uv_build<0.8.0`.

https://src.fedoraproject.org/rpms/python-mkapi/c/8ecdaabbf78c34915d4c7eb9d3be5b9c2db20ce0?branch=rawhide

---

_Comment by @zanieb on 2025-08-15 20:11_

@hroncok thanks for the data point! I think concrete examples are helpful.

The authors of those packages should probably update the upper bound as they upgrade uv and validate their package is compatible, right?

---

_Comment by @musicinmybrain on 2025-08-15 20:29_

> The authors of those packages should probably update the upper bound as they upgrade uv and validate their package is compatible, right?

Yes, ideally, upstream maintainers would closely tend their dependencies and build systems. In practice, even among actively-maintained projects, there are some that operate on a philosophy of “if it ain’t broke, don’t fix it,” and others that have a release cadence on the order of several years, so… results vary.

In the case of that particular example, upstream chose to remove the version bound altogether in an unreleased commit, https://github.com/daizutabi/mkapi/commit/e0777398e7f5e285bf88fbd0b048f2eeb3d9ceaa.

---
