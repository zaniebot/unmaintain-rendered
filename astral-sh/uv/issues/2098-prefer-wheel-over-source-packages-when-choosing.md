---
number: 2098
title: Prefer wheel over source packages when choosing between multiple indexes
type: issue
state: open
author: andrey-klochkov-liftoff
labels:
  - enhancement
assignees: []
created_at: 2024-03-01T02:34:34Z
updated_at: 2024-03-01T18:24:06Z
url: https://github.com/astral-sh/uv/issues/2098
synced_at: 2026-01-10T01:23:12Z
---

# Prefer wheel over source packages when choosing between multiple indexes

---

_Issue opened by @andrey-klochkov-liftoff on 2024-03-01 02:34_

We build wheels for packages that only have a source package on pypi.org, and put them on our private pypi repo. We then do `pip install --index-url https://pypi.org/simple/ --extra-index-url https://private-repo/simple/`, and `pip` always chooses to install the wheel package if it's available in our repo.

There are two different related issues with using `uv pip install` in this mode. 

First, if we pass `--index-url` and `--extra-index-url`, `uv` would always prefer the repo passed with `--index-url` (as explained in #2083), and so our repo contains an older version of the package only but we want to install a newer version from pypi.org, `uv` won't find it. If we instead pass both indexes using `--extra-index-url`, `uv` would work in this scenario but it will fail in the second (below).

The second scenario is that we have a wheel package in our private repo, and `pypi.org` only contains the source repo. Here `uv` for some reason attempts to install the source repo (and we don't want to), and there doesn't seem to be a way to make it pick the wheel. The only way is to pass our repo through the `--index-url` argument, but then we get the 1st scenario failing.

Sorry if this has been discussed before but I haven't found this exact issue (or a combination of two?) being discussed previously.

Thanks for such a great tool! I wish we could just switch to doing `uv pip install`, but these corner cases are blocking us from that for now.

---

_Comment by @charliermarsh on 2024-03-01 03:12_

Thanks for the clear issue!

One thing for clarity: in the most recent release, we switched the priority (per your link: https://github.com/astral-sh/uv/pull/2083).

So, as of the most recent release, if you provide `--index-url https://pypi.org/simple/ --extra-index-url https://private-repo/simple/`, `uv` will first look in `https://private-repo/simple/`; if the package exists, it will stop, and return those versions. If it doesn't, it will look in PyPI.

Unlike `pip`, we don't currently look in _both_ indexes and merge the versions. We're considering making that change, but it also brings with it a bunch of security issues.

Based on that, my read of the above is:

- If you want to install a newer version from PyPI of a repo that exists in your private index -- we don't currently support that, since we'll never look in PyPI if a package exists in your private index. We may support it in the future.
- On the second scenario: I'm not sure what this would be. uv will only look at one index. (Prior to the most recent release, it would look at PyPI, not your custom index; we just reversed that.)

---

_Label `enhancement` added by @charliermarsh on 2024-03-01 05:38_

---

_Comment by @andrey-klochkov-liftoff on 2024-03-01 18:24_

@charliermarsh ,
Thanks! Is there an issue that tracks the 1st change from your list that I can subscribe to?

---
