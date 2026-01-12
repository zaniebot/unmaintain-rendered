```yaml
number: 14690
title: Add release notes and bump version for 0.8.0 
type: pull_request
state: merged
author: zanieb
labels:
  - no-build
  - no-test
assignees: []
merged: true
base: release/080
head: zb/release-notes
created_at: 2025-07-17T20:25:01Z
updated_at: 2025-07-17T22:10:06Z
url: https://github.com/astral-sh/uv/pull/14690
synced_at: 2026-01-12T16:11:21Z
```

# Add release notes and bump version for 0.8.0 

---

_@zanieb_

[Rendered](https://github.com/astral-sh/uv/blob/zb/release-notes/CHANGELOG.md)

---

_@zanieb reviewed on 2025-07-17 20:25_

---

_Review comment by @zanieb on `CHANGELOG.md`:14 on 2025-07-17 20:25_

I'll unwrap these lines before merging.

---

_@zanieb reviewed on 2025-07-17 20:25_

---

_Review comment by @zanieb on `CHANGELOG.md`:6 on 2025-07-17 20:25_

I need to write front-matter still.

---

_Label `no-build` added by @zanieb on 2025-07-17 20:29_

---

_Label `no-test` added by @zanieb on 2025-07-17 20:29_

---

_Label `no-test` removed by @zanieb on 2025-07-17 20:34_

---

_Renamed from "Add release notes for 0.8.0" to "Add release notes and bump verison for 0.8.0 " by @zanieb on 2025-07-17 20:40_

---

_Renamed from "Add release notes and bump verison for 0.8.0 " to "Add release notes and bump version for 0.8.0 " by @zanieb on 2025-07-17 20:44_

---

_Review comment by @geofft on `CHANGELOG.md`:11 on 2025-07-17 20:48_

It's true that even users who are "broken" have a path forward, they just might have to tweak things, right? Do you want to say that explicitly somehow?

Possibly
```suggestion
Since we released [0.7.0](https://github.com/astral-sh/uv/releases/tag/0.5.0) in April, we've
accumulated various changes that improve correctness and user experience, but could be backwards-incompatible for some
workflows. This release contains those changes; many have been marked as backwards-incompatible out of an
abundance of caution. We expect most users to be able to upgrade without making changes.
```

---

_Review comment by @geofft on `CHANGELOG.md`:18 on 2025-07-17 20:52_

Definitely not "into the bin" 🗑️ :) "into your `bin` directory", "into `PATH`", etc. would be okay.

---

_Review comment by @geofft on `CHANGELOG.md`:27 on 2025-07-17 21:00_

I would add something like

"Please note that these `python3.x` commands will run the base Python installation with no installed packages, and they will not respect your project configuration. To run the right version of Python from your current project with dependencies installed, continue to use `uv python run`. There is no way to install additional packages into this base Python installation. If you need to run Python with dependencies without detecting settings from your current directory, you can use something like `uv python run --no-config --with numpy`. See [our documentation on ...]\(...) for more details."

---

_Review comment by @ntBre on `CHANGELOG.md`:68 on 2025-07-17 21:00_

```suggestion
  When working in a project, uv uses the [presence of a build
```

"in" or "on" maybe?

---

_Review comment by @geofft on `CHANGELOG.md`:74 on 2025-07-17 21:05_

```suggestion
  present, the `setuptools.build_meta:__legacy__ ` backend will be used (per [PEP 517](https://peps.python.org/pep-0517/#source-trees)).
```

---

_Review comment by @geofft on `CHANGELOG.md`:76 on 2025-07-17 21:06_

Is there a reason that one would want to opt out of this? Like I understand that this is breaking in the sense that `import foo` might succeed when it previously failed, but is there some common(-ish) case where people might run into this, like tests, or placeholders for foreign-language code, or something, that would be worth alluding to here?

---

_Review comment by @ntBre on `CHANGELOG.md`:323 on 2025-07-17 21:11_

Do you want these to change down here?

---

_Review comment by @ntBre on `CHANGELOG.md`:27 on 2025-07-17 21:11_

```suggestion
  You can opt out of this behavior with `uv python install --no-bin` or `UV_PYTHON_INSTALL_BIN=0`.
```

I think these can all be two words.

---

_@ntBre reviewed on 2025-07-17 21:12_

Nice! I only saw two tiny nits and a question on the 0.7 notes.

---

_@zanieb reviewed on 2025-07-17 21:14_

---

_Review comment by @zanieb on `CHANGELOG.md`:323 on 2025-07-17 21:14_

Oh, no — thanks!

---

_Review comment by @geofft on `CHANGELOG.md`:103 on 2025-07-17 21:14_

"packages" is incorrect here, the chart is about clients making request to PyPI. How about either "0.3% of PyPI users require `manylinux_2_17`" or "0.3% of requests to PyPI come from a system that is only compatible with `manylinux_2_17` and not newer versions"?

(also the number bumped to 0.3% since Charlie opened the PR)

---

_@zanieb reviewed on 2025-07-17 21:15_

---

_Review comment by @zanieb on `CHANGELOG.md`:27 on 2025-07-17 21:15_

I think this is consistent with the rest of the usage in the project — though I don't feel strongly.

---

_@zanieb reviewed on 2025-07-17 21:15_

---

_Review comment by @zanieb on `CHANGELOG.md`:27 on 2025-07-17 21:15_

😭 til https://english.stackexchange.com/a/385511

---

_Review comment by @konstin on `CHANGELOG.md`:18 on 2025-07-17 21:17_

"into PATH" maybe? bin is an overloaded word by itself

---

_Review comment by @geofft on `CHANGELOG.md`:77 on 2025-07-17 21:18_

A suggestion for alternative text / framing that you may or may not want to try to incorporate:

Several popular Python packages are written partly or even mostly in a compiled language (like C, C++, or Rust) instead of in Python for performance and functionality. These packages can made available on PyPI as both source code as well as precompiled binaries ("wheels") for common platforms.

A precompiled wheel needs to be compatible with the OS you're running for it to work. The `manylinux_2_17` tag, our old default, is compatible with most Linux distributions from 2014 or newer. At this point this is a little _too_ widely compatible and prevents using newer Linux features. We now default to `manylinux_2_24`, which is compatible with most Linux distributions from 2017 or newer. This change follows the lead of other tools such as cibuildwheel...

---

_Review comment by @geofft on `CHANGELOG.md`:107 on 2025-07-17 21:20_

> and should reduce cases where uv needs to build a wheel from source.

I don't think this is right, is it? When installing a package / consuming a wheel, we detect manylinux compatibility based on the current glibc (I hope???). ~And strictly speaking this change will increase the number of cases where people need to build from source, but that's a good tradeoff.~ (no that's not true)

---

_@geofft reviewed on 2025-07-17 21:20_

---

_Review comment by @konstin on `CHANGELOG.md`:33 on 2025-07-17 21:22_

VS Code also uses that to discover interpreters (https://learn.microsoft.com/en-us/visualstudio/python/managing-python-environments-in-visual-studio?view=vs-2022). I don't fully understand how else VS Code finds interpreters, so I don't know how much impact that has for VS Code users.

---

_Review comment by @charliermarsh on `CHANGELOG.md`:38 on 2025-07-17 21:24_

So `uv sync` will still automatically tear down and re-create?

---

_@charliermarsh reviewed on 2025-07-17 21:24_

---

_@charliermarsh reviewed on 2025-07-17 21:24_

---

_Review comment by @geofft on `CHANGELOG.md`:105 on 2025-07-17 21:24_

Brief note about why we're doing this? Performance?

---

_Review comment by @charliermarsh on `CHANGELOG.md`:40 on 2025-07-17 21:24_

"opt out of this behavior" => something about how "opting out" means continuing to remove environments without confirmation.

---

_Review comment by @geofft on `CHANGELOG.md`:181 on 2025-07-17 21:25_

```suggestion
  When the default image user is overridden (e.g. `USER <UID>`) with a less privileged one, this may
```

---

_Review comment by @charliermarsh on `CHANGELOG.md`:148 on 2025-07-17 21:27_

`without opt-in`, maybe?

---

_@geofft reviewed on 2025-07-17 21:27_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:181 on 2025-07-17 21:27_

Maybe "by a less privileged image"

---

_@charliermarsh approved on 2025-07-17 21:28_

---

_@zanieb reviewed on 2025-07-17 21:28_

---

_Review comment by @zanieb on `CHANGELOG.md`:11 on 2025-07-17 21:28_

I'm not sure I want to change this — the language is copied from our previous releases and I don't think it's much clearer as you've proposed.

---

_Review comment by @konstin on `CHANGELOG.md`:100 on 2025-07-17 21:28_

To avoid alarming users about checking what their glibc version is, I'd add a sentence that this only affects user of that specific option (which is rare), we use the host glibc version for all other use cases such as `uv pip install` and `uv sync`.

---

_@geofft reviewed on 2025-07-17 21:28_

---

_Review comment by @geofft on `CHANGELOG.md`:181 on 2025-07-17 21:28_

(less privileged user, not image)

---

_@zanieb reviewed on 2025-07-17 21:29_

---

_Review comment by @zanieb on `CHANGELOG.md`:74 on 2025-07-17 21:29_

I mean, technically that's not the specification anymore but... fine :P

---

_@zanieb reviewed on 2025-07-17 21:30_

---

_Review comment by @zanieb on `CHANGELOG.md`:76 on 2025-07-17 21:30_

Your package might not build.

---

_@zanieb reviewed on 2025-07-17 21:30_

---

_Review comment by @zanieb on `CHANGELOG.md`:103 on 2025-07-17 21:30_

Thanks!

---

_Review comment by @zanieb on `CHANGELOG.md`:38 on 2025-07-17 21:32_

Yeah definitely.

---

_@zanieb reviewed on 2025-07-17 21:32_

---

_@zanieb reviewed on 2025-07-17 21:34_

---

_Review comment by @zanieb on `CHANGELOG.md`:105 on 2025-07-17 21:34_

I think that's covered in the stabilization post, I guess I can elevate that more? A uv build backend being the default for uv makes sense to people in a way that hatchling does not.

---

_@zanieb reviewed on 2025-07-17 21:34_

---

_Review comment by @zanieb on `CHANGELOG.md`:40 on 2025-07-17 21:34_

Hm I started with that pattern for the "opt out" statements but removed it because it was redundant. I kind of want the language to match across items.

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:560 on 2025-07-17 21:34_

We should start mocking the uv version in this test to prevent this hash from changing.

---

_@zanieb reviewed on 2025-07-17 21:35_

---

_Review comment by @zanieb on `CHANGELOG.md`:107 on 2025-07-17 21:35_

I'm quoting from Charlie's pull request. If you request a specific manylinux target, then we won't detect compatibility from your glibc.

---

_@zanieb reviewed on 2025-07-17 21:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:148 on 2025-07-17 21:36_

That's my preference too but I guess it's wrong? https://github.com/astral-sh/uv/pull/14690#discussion_r2214320960

---

_@zanieb reviewed on 2025-07-17 21:36_

---

_Review comment by @zanieb on `CHANGELOG.md`:181 on 2025-07-17 21:36_

It's the user that's less privileged.

---

_@zanieb reviewed on 2025-07-17 21:37_

---

_Review comment by @zanieb on `CHANGELOG.md`:103 on 2025-07-17 21:37_

I wonder if I should just drop these statistics.

There are both consumer and package statistics at https://mayeut.github.io/manylinux-timeline/

---

_@konstin approved on 2025-07-17 21:37_

---

_@konstin reviewed on 2025-07-17 21:41_

---

_Review comment by @konstin on `CHANGELOG.md`:103 on 2025-07-17 21:41_

I'd tend towards keeping this section short, reflecting that we expect it to have lower impact than other changes in this release.

---

_@ntBre reviewed on 2025-07-17 21:41_

---

_Review comment by @ntBre on `CHANGELOG.md`:148 on 2025-07-17 21:41_

I'd probably say "without opting in" here, to sidestep the issue a bit

---

_Review comment by @geofft on `CHANGELOG.md`:77 on 2025-07-17 21:42_

OK now that I understand the change a little bit better:

Several popular Python packages are written partly or even mostly in a compiled language (like C, C++, or Rust) instead of in Python for performance and functionality. These packages can made available on PyPI as both source code as well as precompiled binaries ("wheels") for common platforms. Precompiled wheels are significantly faster and more reliable to install than building code from source.

A precompiled wheel needs to be compatible with the OS you're running for it to work. When uv installs packages for your system, it detects the version of your OS to find a compatible precompiled wheel if one is available. However, if you are using the `--python-platform linux` option for `uv pip compile`, `uv pip install`, and a few other subcommands, we previously defaulted to `manylinux_2_17`, compatible with most Linux distributions from 2014 or newer. At this point this is a little too widely compatible and prevents using newer Linux features, and third-party projects are looking to target a newer baseline. `--python-platform linux` now defaults to `manylinux_2_24`, compatible with most Linux distributions from 2017 or newer. This change follows the lead of other tools in the ecosystem like [cibuildwheel](https://github.com/pypa/cibuildwheel/pull/2330), and ensures that resolutions from `--python-platform linux` are likely to find precompiled wheels if they exist while still maximizing compatibility.

Most users of uv will never need to specify `--python-platform`, but it may affect you if you are using uv to generate environments or `requirements.txt` files to run on other systems. If you know the exact OS version of the Linux systems you are targeting, it might be a good idea to specify the highest compatible manylinux version for those systems to get the best available packages, instead of using `--python-platform linux`.

You can opt out of this behavior by replacing `--python-platform linux` with `--python-platform x86_64-manylinux_2_17`.

---

_@geofft reviewed on 2025-07-17 21:42_

---

_@zanieb reviewed on 2025-07-17 21:43_

---

_Review comment by @zanieb on `CHANGELOG.md`:107 on 2025-07-17 21:43_

I'll just drop that comment.

---

_@geofft reviewed on 2025-07-17 21:44_

---

_Review comment by @geofft on `CHANGELOG.md`:100 on 2025-07-17 21:44_

Yes, that's my concern - it took me a while to understand that this does not affect normal resolution _or_ normal builds (since uv doesn't do builds in manylinux containers, that's cibuildwheel's job).

---

_@geofft reviewed on 2025-07-17 21:48_

---

_Review comment by @geofft on `CHANGELOG.md`:107 on 2025-07-17 21:48_

Oh I think I understand the thing that is wacky to me - if you are on e.g. glibc 2.40, and you do `uv pip install --python-platform linux`, and the only wheel available is manylinux_2_30, then we will not use that wheel and we'll build from source, _but the resulting locally-compiled wheel will be built against glibc 2.40_.

The thing I had in mind is that, in the `uv pip compile` case, we aren't building any wheels, and any consumer of the resulting `requirements.txt` file (either uv or pip) is likely to autodetect glibc compatibility and pick up wheels. ... ... except that we _are_ building wheels because we can't detect deps out of an sdist, right? but we're going to be building a local wheel against the local glibc and reading deps out of it, ugh

---

_@zanieb reviewed on 2025-07-17 21:58_

---

_Review comment by @zanieb on `CHANGELOG.md`:77 on 2025-07-17 21:58_

While this is nice, I think this description is really not proportionate to the size of the change 😬 I'm pretty hesitant to say this much?

---

_Review comment by @geofft on `CHANGELOG.md`:20 on 2025-07-17 22:00_

grammar nit: "and only include" (or "point to base Python installations that only include", or something)

---

_Label `no-test` added by @zanieb on 2025-07-17 22:00_

---

_Review comment by @geofft on `CHANGELOG.md`:40 on 2025-07-17 22:02_

Maybe "To match the behavior from 0.7.x., ..." for all of them?

---

_Review comment by @geofft on `CHANGELOG.md`:77 on 2025-07-17 22:02_

Lemme try to rework this to make it terser, I do think we need to clarify what this _doesn't_ effect.

---

_@geofft reviewed on 2025-07-17 22:02_

---

_@zanieb reviewed on 2025-07-17 22:05_

---

_Review comment by @zanieb on `CHANGELOG.md`:77 on 2025-07-17 22:05_

I did add that though?

---

_Merged by @zanieb on 2025-07-17 22:07_

---

_Closed by @zanieb on 2025-07-17 22:07_

---

_Branch deleted on 2025-07-17 22:07_

---

_Review comment by @geofft on `CHANGELOG.md`:77 on 2025-07-17 22:10_

`uv pip compile`, `uv pip install`, and a few other subcommands have a `--python-platform` option that lets you perform a [platform-specific resolution](https://docs.astral.sh/uv/concepts/resolution/#platform-specific-resolution) for a platform other than your current one.

`--python-platform linux` is shorthand for a particular [manylinux](https://github.com/pypa/manylinux) tag that is widely compatible. Previously, it was shorthand for `manylinux_2_17`, compatible with most Linux distributions from approximately 2014 onward. Now, it is shorthand for `manylinux_2_28`, compatible with most Linux distributions from approximately 2017 onward.

As third-party packages drop support for building precompiled wheels on very old Linux distributions, this improves the likelihood that uv will successfully find precompiled wheels when resolving packages. In particular, as of March 2025, [`cibuildwheel` now defaults to `manylinux_2_28`](https://github.com/pypa/cibuildwheel/pull/2330).

This change does not affect uv's resolution behavior if you do not specify `--python-platform`; it will continue to detect your current OS version.

You can opt out of this behavior by replacing `--python-platform linux` with `--python-platform x86_64-manylinux_2_17`.


---

_@geofft reviewed on 2025-07-17 22:10_

---
