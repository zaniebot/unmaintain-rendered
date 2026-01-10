---
number: 7957
title: Add support for cross-platform environments
type: issue
state: open
author: freakboy3742
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-10-07T03:56:32Z
updated_at: 2025-06-02T19:09:08Z
url: https://github.com/astral-sh/uv/issues/7957
synced_at: 2026-01-10T01:24:21Z
---

# Add support for cross-platform environments

---

_Issue opened by @freakboy3742 on 2024-10-07 03:56_

Python 3.13 added iOS and Android as Tier 3 supported platforms for CPython. It would be desirable to be able to use uv to develop apps for these environments.

However, providing this support requires that uv supports a new feature: cross-platform environments.

At runtime, iOS and Android behave largely as any other CPython platform would behave. However, the *build* experience is radically different, as you don't run `pip` (or any other build tooling) on the platform that will be running the code. Instead, a build machine (with an operating system different to the runtime platform) is used to package code into a distribution artefact, and that distribution artefact is then executed on the device. 

For example, when building an iOS app, the *build* must be executed on macOS; this collects any wheels (including binary wheels compiled for the iOS platform), and collates the wheels into a form into the compiled app bundle. It must be possible to create an "iOS environment" on macOS, or an "Android environment" on macOS/Linux/Windows, and install iOS/Android binary wheels into that environment. The build machine isn't able to run any of the code that has been installed (at least, not directly; a simulator/emulator environment must be used). 

## How to implement cross-platform environments 

At a bare minimum, cross-platform support requires being able to control the wheel tags that match as a part of dependency resolution. However, a more comprehensive solution would require creating an entire "cross-platform" virtual environment.

`pip` has a `--platform` argument that can be used to directly control the wheel tags that match an installation request. AFAICT, there is no analog of this argument for uv at present. By passing in `--platform ios_13_0_arm64_iphoneos`, pip will install binary wheels that are compatible with execution on an ARM64 iOS device [^1]. 

[^1]: At present, pip will only match the *exact* iOS version named in the platform tag; a full solution requires an analog of macOS ABI version lookups. A [PR adding support for iOS package resolution is currently under review](https://github.com/pypa/pip/pull/12962); the [PR implementing the underlying lookup scheme](https://github.com/pypa/packaging/pull/832) has already been merged. 

[Crossenv](https://crossenv.readthedocs.io/en/latest/) recently merged [this PR](https://github.com/benfogle/crossenv/pull/117), providing a mechanism for creating a "crossenv" for iOS. This project is based on a trick with `site.py` that monkeypatches a standard virtual environment so that when code is executed in the environment, it will return `sys.platform` and `sysconfig` values that reflect the host/target environment, rather the build/source environment. When an iOS crossenv is active, invoking `pip` will download iOS-compatible wheels, rather than macOS-compatible wheels that would run on the build machine. 

## What a cross-platform environment *isn't*

It's important to differentiate between a cross-platform *build* environment, and feature requests like #2408 and #7753. Those tickets are requesting the ability to run uv *on* Android - that is, a traditional Python development environment, but running on an Android device. While there are clearly people that use Android in this way, it is *not* how the vast majority of Android users interact with their devices, nor does it reflect a workflow that would allow the development of apps that could be distributed in the Google Play store (or similar).

## Other uses

Although cross-platform build environments are a requirement for supporting iOS and Android, the generic functionality of cross-platform environments can be useful in any situation where you wish to produce a binary artefact for a platform without actually running the code for that platform is a candidate. For example, you could produce Raspberry Pi ARM64 binary wheels while on a Github Actions provided x86_64 Linux machine. 


---

_Comment by @charliermarsh on 2024-10-08 22:37_

We do support `--python-platform` which lets you create a virtual environment with wheels targeted at another platform. But it won't perform _cross compilation_, i.e., if it builds from source, you get a wheel for the current platform. Are you asking for something different here?

---

_Label `question` added by @charliermarsh on 2024-10-08 22:37_

---

_Comment by @freakboy3742 on 2024-10-09 00:30_

Looks like I missed the `--python-platform` option because I was looking for the exact match of `--platform`. Thanks for the pointer. 

That flag does solves cross-platform wheel installation, which is a substantial part of the underlying problem. There's a related issue to add support for the PEP 730 iOS and PEP 738 Android tags; but I'll open that as a separate feature request.

The part that `--python-platform` *doesn't* solve is creating a runnable Python environment that identifies as being cross platform. This isn't about cross-compilation *per se*; but it is about providing the conditions to make cross-compilation possible. Since `uv` considers environment creation part of its domain, creating a cross-platform environment is (arguably) also part of that domain.

In order for PEP 517 interfaces to work, the Python install being used to construct the wheel must identify as the *host* platform - that is, `sys.platform` must return "ios" not "darwin"; `sysconfig.get_platform()` must return "ios-13.0-arm64-iphoneos", not "macosx-10.13-universal2"; and so on. Any tools used during the build process (e.g., CMake, Meson, Cython) need to use binaries that will run on the *build* platform; but must produce binaries that are compatible with the *host* platform (either as a result of interrogating `sys.platform` et al, or as a result of some other environmental configuration).

[Crossenv](https://crossenv.readthedocs.io/en/latest/) is one tool that can be used to achieve this; the core of crossenv is essentially a site package hack that monkey patches `sys`, `sysconfig` et al to allow build tooling to pretend it's the host environment. 

What I'm envisaging is a similar set of tools built into `uv venv`, so that `uv venv --python-platform ios_13_0_arm64_iphoneos` (or similar) would create a viable cross-platform environment that could be used to build iOS wheels, and where `uv pip install` would default to the same platform tag.


---

_Label `question` removed by @zanieb on 2024-10-09 01:27_

---

_Label `enhancement` added by @zanieb on 2024-10-09 01:27_

---

_Label `wish` added by @zanieb on 2024-10-09 01:27_

---

_Comment by @zanieb on 2024-10-09 01:28_

This sounds cool, but I would be surprised if we had the time to build it soon. I'm curious if there's demand from more people for a feature like this.

---

_Referenced in [pypa/cibuildwheel#2286](../../pypa/cibuildwheel/pulls/2286.md) on 2025-02-24 06:34_

---

_Comment by @eabase on 2025-02-25 07:57_

Also consider #11773 

---

_Referenced in [pypackaging-native/pkgconf-pypi#74](../../pypackaging-native/pkgconf-pypi/issues/74.md) on 2025-05-26 02:46_

---

_Comment by @rgommers on 2025-06-02 19:08_

> What I'm envisaging is a similar set of tools built into `uv venv`, so that `uv venv --python-platform ios_13_0_arm64_iphoneos` (or similar) would create a viable cross-platform environment

I think the clean solution is to start using [PEP 739](https://peps.python.org/pep-0739/) and provide the `build-details.json` file for the Python interpreter matching the host environment installed with `--python-platform`. The PEP was only recently accepted and build backend support for it is still WIP, but it gives build tools access to the host platform and other `sysconfig`/`sys` module properties without hacks or having to run the non-native interpreter. I suspect all `uv venv --python-platform` should then be doing is install `build-details.json` in a standard location, so a cross build tool can be built on top that uses `uv` to create build and host envs and then invokes a cross build passing the location of that file to the build backend.

---
