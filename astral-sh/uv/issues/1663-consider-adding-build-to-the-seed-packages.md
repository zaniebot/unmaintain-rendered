```yaml
number: 1663
title: "Consider adding `build` to the \"seed\" packages"
type: issue
state: closed
author: charlesnicholson
labels:
  - virtualenv
assignees: []
created_at: 2024-02-18T18:08:33Z
updated_at: 2024-09-04T15:23:49Z
url: https://github.com/astral-sh/uv/issues/1663
synced_at: 2026-01-12T15:58:31Z
```

# Consider adding `build` to the "seed" packages

---

_@charlesnicholson_

`uv venv --seed` installs + upgrades `setuptools`, `pip`, and `wheel`, which implies that its purpose is to facilitate distribution package creation. The recent distribution-packaging overhaul over the past few years in the Python world makes `build` also a critical package. Would you consider adding it to the `--seed` set?

https://packaging.python.org/en/latest/tutorials/packaging-projects/#generating-distribution-archives

<img width="850" alt="Screenshot 2024-02-18 at 1 08 07 PM" src="https://github.com/astral-sh/uv/assets/3010295/c2b70524-e778-493c-9163-3d06c1486d5f">


---

_Comment by @zanieb on 2024-02-18 20:36_

Thanks for the feedback!

Is there another virtual environment tool that does this? Are there issues for this in `venv`, `virtualenv`, or other tools? Generally we'd like to come to decisions like this with a broad consensus in the community.

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @charlesnicholson on 2024-02-18 22:44_

`venv` doesn't appear to even install `wheel`, but I guess `venv` is kinda bare-bones (and so very slow).

<img width="426" alt="Screenshot 2024-02-18 at 5 29 58 PM" src="https://github.com/astral-sh/uv/assets/3010295/043ecd7d-b79b-44e8-9f07-6a387f807d76">

`virtualenv` appears to only seed some combination of `setuptools`, `wheel`, and `pip`: https://virtualenv.pypa.io/en/latest/user_guide.html#seeders

<img width="845" alt="Screenshot 2024-02-18 at 5 31 57 PM" src="https://github.com/astral-sh/uv/assets/3010295/f55a59a1-8733-4d4e-99bb-8bae3c7dce6e">

Your point about consensus makes total sense, and `uv` is fast enough that immediately following a seeded venv creation with `uv pip install build` is fine for us. I just figured I'd flag it since it seems like `build` is becoming the de-facto build front-end, and the official Python packaging docs are recommending it.

Thanks for reading!


---

_Comment by @mitsuhiko on 2024-02-18 22:58_

Generally in a perfect world `build` would never end up in a virtualenv. I went down this path a few times with `rye` already and the way it works there is that if you invoke `rye build` it deals with `build` in a completely isolated environment. This is from what I can tell also the way other systems already approach this.

For `uv` it's in a weird spot right now. Medium term it probably would make sense to have a `uv build` command that triggers the build process, but that might conflict with longer term goals.

---

_Comment by @charlesnicholson on 2024-02-19 00:46_

> Generally in a perfect world build would never end up in a virtualenv.

In our build (mixed C, C++, Python, firmware, host tools, etc) we use the built-in vanilla `venv` package to create an isolated virtual environment so that the programmer's host OS and developer-specific setup is irrelevant and doesn't influence the build or cause build divergences. We manually bootstrap the `setuptools`, `wheel`, `pip`, and `build` packages into that virtual environment, and then our build system has targets for editable package installations, wheel builds / distribution packages, tools that require venv installations (nanopb), etc.

I'm curious what you mean about `build` never needing to end up in a venv! Do you mean that `build` should be invoked from the host OS directly, or that Python's build solutions should just be better about isolation in general, or do you mean something else that I'm not understanding yet?

Our (initial) use case for `uv` is to just have faster-but-still-compatible versions of `venv` and `pip install` and shave 30+ seconds off of our build. We also like not diverging too far from the slow-but-well-trodden path of standard Python techniques. From your comment, though, it sounds like maybe you're describing a different use case, where `uv` ends up fully replacing `pip`, and how that could behave in the future?

---

_Comment by @kvelicka on 2024-02-19 08:27_

> I'm curious what you mean about build never needing to end up in a venv! Do you mean that build should be invoked from the host OS directly, or that Python's build solutions should just be better about isolation in general, or do you mean something else that I'm not understanding yet?

`build` builds stuff by creating a temporary virtualenv, downloading all the build dependencies there and using that to produce the actual artefacts

 This means that `build` doesn't _need_ virtualenv but (IMO) that doesn't mean that it can't/shouldn't be installed in a virtualenv anyway - personally I don't think I'd trust any non-first-party package to be installed directly into my system Python, `build` included.

---

_Comment by @henryiii on 2024-02-19 16:07_

(build maintainer here) I think `uv` should gain a `build` subcommand (I might even help work on that in the near future) - see https://github.com/astral-sh/uv/issues/1510. The producedure is well documented by PEPs and anyone can implement it, including `uv` (technically, it already has it, it mostly just needs a CLI). The primary purpose of `--seed` was originally to make a "minimal _working_ environment"; that was `setuptools`, `wheel`, and `pip`. Nowadays, setuptools and wheel aren't needed anymore, so those are no longer included (for backward compat reasons, only on 3.12+). That leaves only pip, and that's not needed if you use `uv`'s (or `pip`'s) targeting feature.

I don't think `build` is ever intended to be part of a minimal working environment. It's only useful if you need to build wheels or SDists and not everyone needs to do that.

> build builds stuff by creating a temporary virtualenv --- personally I don't think I'd trust any non-first-party package

By the way, pip/uv do this too, and all three allow you turn this off by turning off build isolation. And build is just as much or as little of a third-party tool as pip or virtualenv is, all of them are PyPA projects.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-31 16:47_

---

_Closed by @charliermarsh on 2024-09-04 15:23_

---

_Closed by @charliermarsh on 2024-09-04 15:23_

---
