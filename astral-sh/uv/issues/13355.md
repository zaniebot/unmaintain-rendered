```yaml
number: 13355
title: "I would like to generate `uv` lock files and exclude optional dependencies. `uv sync` and `uv lock` don't seem to offer this"
type: issue
state: closed
author: andres-ito-traversal
labels:
  - question
assignees: []
created_at: 2025-05-08T21:22:08Z
updated_at: 2025-05-12T19:34:24Z
url: https://github.com/astral-sh/uv/issues/13355
synced_at: 2026-01-10T03:41:47Z
```

# I would like to generate `uv` lock files and exclude optional dependencies. `uv sync` and `uv lock` don't seem to offer this

---

_Issue opened by @andres-ito-traversal on 2025-05-08 21:22_

### Question

Am I able to run `uv lock` and exclude optional dependencies? My team uses some private artifactory(s) that we would not like to include in the lock file. We organize these deps into optional deps groups so it'd be nice to do something like `uv lock --all-extras --no-extra="optional-dep-to-exclude"`

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.17

---

_Label `question` added by @andres-ito-traversal on 2025-05-08 21:22_

---

_Comment by @ReinforcedKnowledge on 2025-05-09 05:23_

I don't think that's possible with `uv`'s lock file design. It's a universal resolution that's supposed to work no matter what the user decides to install later on.

I think you have two choices: 1/ remove the optional dependency from your `pyproject.toml`, or 2/ export to `pylock.toml`. The current `export` gives a single use [PEP 751](https://peps.python.org/pep-0751/) lock file.

And you can do `uv export --all-extras --no-extra="..." --format pylock.toml` (then save it in a PEP 751 compatible file name).

You can `uv pip install --requirements pylock.my-single-use-lock-file.toml`.

---

_Comment by @andres-ito-traversal on 2025-05-09 16:14_

I see. So `pylock.toml` would be the lock file in a toml format. 

A bit of a dumb question here, but it looks like there are three file formats for lock files: 
1) uv.lock
2) requirements.txt
3) pylock.toml

Is that right? My work around currently was using `uv pip compile` and using the (2) as my lock file format. I'll give 3 a shot and report back

---

_Comment by @konstin on 2025-05-09 16:17_

If you want to lock only specific dependencies, you can use `uv pip compile`. `uv sync` enforces consistency between all dependencies of a project, including optional dependencies and dependency groups, so it's not suited for this use case. `uv.lock` is uv's format, which is tightly coupled to `uv lock`. `pylock.toml` can hopefully replace `requirements.txt`, but it's still young and you might find bugs and no or barebones support in tools.

---

_Comment by @ReinforcedKnowledge on 2025-05-09 16:26_

To add on Konstin's reply, the three files you mention can all be seen as lock files, but there is no real consensus about what that term specifically means. We can all agree that the purposes is to lock in your dependencies. But there are various degrees of precision, security, and reproducibility that each of those files allow you to get.

`requirements.txt` has the widest support, but is also the most lacking in terms of the the proprieties above (in my opinion at least) compared to the other two.

`pylock.toml` can be used as a replacement of `uv.lock` in many cases but it lacks both support and some features that come with `uv`.

`uv.lock` seems to me the best out of the three, but the tool support argument can be made for it as well (you can only use it with `uv`). By the way, `uv.lock` is also written in the TOML format.

(We can get into more details but I don't this a GitHub comment is the appropriate place for that)

---

_Comment by @andres-ito-traversal on 2025-05-09 17:01_

This is all so helpful! Thanks - I discussed with my team, and we're going to proceed with using `requirements.txt` since it's been battle tested quite a bit. 

We have deployments across various environments where we have to install certain dependencies from private artifactories so this solution seems to scale to our use-case the best.

Many thanks guys :D By the way, uv reduced our build image stage from 5 hours to about 40 minutes (when there isn't a cache hit). We all love what you guys do (uv and ruff)!


---

_Closed by @konstin on 2025-05-12 19:34_

---

_Reopened by @konstin on 2025-05-12 19:34_

---

_Closed by @konstin on 2025-05-12 19:34_

---
