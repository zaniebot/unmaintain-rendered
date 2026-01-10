---
number: 16093
title: "Support meson-python in `uv init`"
type: issue
state: open
author: refack
labels:
  - enhancement
assignees: []
created_at: 2025-10-01T23:07:53Z
updated_at: 2025-10-29T21:51:47Z
url: https://github.com/astral-sh/uv/issues/16093
synced_at: 2026-01-10T01:26:03Z
---

# Support meson-python in `uv init`

---

_Issue opened by @refack on 2025-10-01 23:07_

### Summary

For some unknown reason you've chosen to support [`scikit-build-core`](https://docs.astral.sh/uv/concepts/projects/init/#projects-with-extension-modules) which while interesting, is a pre-release system. And you chose to pushdown `meson-python` which is at version 1.9.1 and is used by projects like `numpy` and `scipy`....  ¯\\_(ツ)_/¯.

IMHO meson-python (or just meson) should be the lingua-franca of compiled packages and compiled extensions.

~(FYI, if you find a way to solve the setuptools_scm bootstrap loop, you will get my undying devotion, and patronage)~
[`extra-build-dependencies`](https://docs.astral.sh/uv/reference/settings/#extra-build-dependencies) solved it for my use case.
Thank you astral 🎊 

### Example

_No response_

---

_Label `enhancement` added by @refack on 2025-10-01 23:07_

---

_Renamed from "meson-python!!!" to "Support meson-python in `uv init`" by @konstin on 2025-10-02 08:34_

---

_Comment by @eli-schwartz on 2025-10-29 01:38_

Although I'm slightly biased as a Meson upstream maintainer... I do think that it is generally interesting to support multiple options here given that for one reason or another there will be people who bounce hard off of some subset of the possibilities. e.g. the build definition styles for Meson versus CMake are quite different.

To demonstrate my bias, I will note that I think Meson has a particular (and objective) major advantage over CMake which may tempt people to offer it as a default, in that Meson is a build backend written in pure python and:
- is, at 1 mb, very small to download and install (CMake wheels average 30 - 50 mb)
- is available per definition for all operating systems and CPU architectures (CMake wheels are only available for a relatively small handful of platforms)

But ultimately, compiled code with dependencies is inherently challenging and will require the author to invest time and effort into interacting with the build configurations -- it's not like simply packaging up a src/ directory as an installable module -- so I think there's very little and possibly no value at all in trying to discourage choice and get everyone to stick to one or the other for the simplifying properties of "uniformity's sake". Commentary can be had about whether one perhaps feels that "all options other than XXX are actually eye-wateringly painful, the other upstream projects should simply close down and discontinue themselves for the preservation of user sanity" but short of that kind of religious holy war (the answer is naturally that XXX == vim, thankyouverymuch) I really think the best option is just to lay the options out on the table for users and go with whatever they pick.

I'm available to provide feedback for meson configs, if necessary.

---

_Comment by @eli-schwartz on 2025-10-29 01:55_

> [`scikit-build-core`](https://docs.astral.sh/uv/concepts/projects/init/#projects-with-extension-modules) which while interesting, is a pre-release system. And you chose to pushdown `meson-python` which is at version 1.9.1


I'd like to note in fairness here that scikit-build-core is at version 0.11.6 while meson-python is actually at version 0.18.0

Meson itself is 1.9.1, which roughly corresponds to CMake 4.x, as both are the respective underlying C/C++ build configurators.

The version number of meson-python is of course still higher than scikit-build-core, and meson-python is much older, and... 

>  and is used by projects like `numpy` and `scipy`.... ¯_(ツ)_/¯.

This is quite true. (Pandas and matplotlib as well.)

> (FYI, is you solve the setuptools_scm bootstrap loop, you will get my undying devotion, and patronage)

I'm not aware of a bootstrap loop here, above and beyond the general issue of jaraco.* packages being deps for setuptools but also depending on setuptools itself (setuptools-scm is therefore irrelevant).

---

_Comment by @konstin on 2025-10-29 18:16_

We didn't exclude meson-python, we just didn't have anyone who added support for, meson-python is popular enough that it makes sense to support it in `uv init`.

---

_Comment by @refack on 2025-10-29 21:33_

> the build definition styles for Meson versus CMake are quite different.

I never liked CMake's style. meson reminds me of my old stomping ground [`GYP`](https://github.com/refack/gyp3)'s style (except for the exact opposite approach WRT declarative/imperative)

> I'm not aware of a bootstrap loop here

the crux is when running with `--no-build-isolation`, where `uv` needs `setuptools-scm` to find the version of the project before it solves a recipe for `build-system.requires`.
RE: my original comment, I believe that is one of the use cases for the (in preview) [`extra-build-dependencies`](https://docs.astral.sh/uv/reference/settings/#extra-build-dependencies) setting. So WW Astral !

> I'd like to note in fairness here that scikit-build-core is at version 0.11.6 while meson-python is actually at version 0.18.0

I stand corrected.


---
