---
number: 3271
title: "`PROJECT_ROOT` in `requirements` uses the working directory"
type: issue
state: open
author: jamesbraza
labels:
  - needs-design
assignees: []
created_at: 2024-04-25T20:34:22Z
updated_at: 2025-10-12T21:16:49Z
url: https://github.com/astral-sh/uv/issues/3271
synced_at: 2026-01-10T01:23:26Z
---

# `PROJECT_ROOT` in `requirements` uses the working directory

---

_Issue opened by @jamesbraza on 2024-04-25 20:34_

I have a `setuptools.build_meta` backend monorepo with a `pyproject.toml` package named `packagename` in a subfolder. I specify its dependency in a `requirements.txt` at the repo root: `package-name @ file://${PROJECT_ROOT}/packagename`

In `/Users/user/code/repo-root`, running `uv pip install --no-cache .` works fine.

Moving up one directory to `/Users/user/code` and running `uv pip install --no-cache ./repo` fails:

```none
error: Distribution not found at: file:///Users/user/code/packagename
```

Can we somehow know `PROJECT_ROOT` should be `./repo`, instead of `./` in this case?

---

I am using `uv 0.1.38 (0b23caa18 2024-04-24)`

---

_Comment by @charliermarsh on 2024-04-25 23:27_

Hmm, right now `PROJECT_ROOT` is just the current working directory.

---

_Comment by @charliermarsh on 2024-04-25 23:32_

Slightly tempted to set it to the directory of the containing file.

---

_Comment by @jamesbraza on 2024-04-25 23:37_

Lol give in to your temptations 😈 🔥 🚀 🦾 😆 

---

_Comment by @zanieb on 2024-04-25 23:39_

Is that "breaking" ?

---

_Comment by @charliermarsh on 2024-04-25 23:41_

I'd consider it breaking.

---

_Comment by @zanieb on 2024-04-25 23:41_

Would we support `$PWD` too or whatever?

---

_Comment by @charliermarsh on 2024-04-25 23:42_

I believe we already do.

---

_Comment by @charliermarsh on 2024-04-25 23:42_

We support any env var, and then `PROJECT_ROOT` is special on top of those.

---

_Comment by @charliermarsh on 2024-04-25 23:43_

Using the directory that contains the file definitely seems right for `pyproject.toml`. I could see it being less convenient for requirements.txt in some rare cases?

---

_Comment by @zanieb on 2024-04-26 01:53_

Yeah the latter seems more problematic, e.g. `requirements/dev.txt` — but it seems quite correct for `pyproject.toml`.

I wonder if we should scan for `git` boundaries for `requirements.txt`...

---

_Comment by @charliermarsh on 2024-04-26 02:00_

> Yeah the latter seems more problematic, e.g. requirements/dev.txt — but it seems quite correct for pyproject.toml.

Yeah. Is it even that bad though? It just means you need to make the path relative to the file rather than the root directory. But it would still work consistently regardless of where you invoked uv from, unlike the current implementation.

---

_Comment by @zanieb on 2024-04-26 02:16_

I'm curious if anyone else will weigh in, but yeah having it work consistently seems like an improvement.

---

_Comment by @jamesbraza on 2024-04-26 17:19_

Maybe `PACKAGE_ROOT` can have a fallback (or a configuration option `--package-root-is-not-cwd`) to maintain compatibility:

1. Looks in target directory (in this case, `./repo`)
2. Falls back on current working directory

That being said, imo the most straightforward/intuitive route is to support two environment variables:

```none
# Uses input uv command to resolve to ./repo/foo
file://${PROJECT_ROOT}/foo

# Always resolves to ./foo
file://${CURRENT_DIRECTORY}/foo
```


---

_Comment by @charliermarsh on 2024-04-27 12:19_

For `${CURRENT_DIRECTORY}` you can likely already do `${PWD}`.

---

_Label `needs-decision` added by @charliermarsh on 2024-04-27 12:27_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-06-01 16:58_

---

_Comment by @charliermarsh on 2024-06-01 16:58_

We'll ship this in v0.3.0.

---

_Removed from milestone `v0.3.0` by @zanieb on 2024-11-25 20:24_

---

_Added to milestone `v0.6.0` by @zanieb on 2024-11-25 20:24_

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Comment by @ShapelessCat on 2025-03-14 15:27_


@jamesbraza In the title of this issue and in one of your comment, you used the `PACKAGE_ROOT`, not `PROJECT_ROOT`. Are those typos or because of some historical reason?

---

_Comment by @jamesbraza on 2025-03-14 17:09_

I believe I used `PACKAGE_ROOT` and `PROJECT_ROOT` interchangeably in [this comment](https://github.com/astral-sh/uv/issues/3271#issuecomment-2079785370), both to refer to a path to the repo root containing the `pyproject.toml`.

I think `PROJECT_ROOT` makes more sense than `PACKAGE_ROOT`, given [workspaces](https://docs.astral.sh/uv/concepts/projects/workspaces/#workspace-sources) can have 2+ packages. Really `PROJECT_ROOT` is the path to the repo root

---

_Comment by @ShapelessCat on 2025-03-17 13:52_

Hi, @charliermarsh, I noticed the 'needs-decision' label is still applied. Does this indicate that a final plan for this issue hasn’t been determined? I ask this because I can see this issue has been moved from one milestone to the next milestone several times. I just encountered the same `PROJECT_ROOT` issue in my project.

---

_Referenced in [astral-sh/uv#12973](../../astral-sh/uv/issues/12973.md) on 2025-04-21 01:56_

---

_Renamed from "Bug (?): `PACKAGE_ROOT` not working one level up" to "`PROJECT_ROOT` in `requirements` uses the working directory" by @zanieb on 2025-04-30 17:33_

---

_Label `needs-decision` removed by @zanieb on 2025-04-30 17:33_

---

_Label `needs-design` added by @zanieb on 2025-04-30 17:33_

---

_Referenced in [astral-sh/uv#13239](../../astral-sh/uv/issues/13239.md) on 2025-04-30 17:36_

---

_Removed from milestone `v0.7.0` by @zanieb on 2025-04-30 17:36_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-30 17:36_

---

_Removed from milestone `v0.8.0` by @zanieb on 2025-06-20 15:15_

---

_Added to milestone `v0.9.0` by @zanieb on 2025-06-20 15:15_

---

_Comment by @m1cr0man on 2025-07-10 12:35_

Hi there. I just ran into this myself. I fully expected `PROJECT_ROOT` to refer to the directory containing the pyproject.toml. This is particularly problematic when using `uv run --script` in a shebang. If I call the script with the shebang from a working directory other than the project root (example `../myscript.py`), I get a `Distribution not found` error on the requirements utilising `PROJECT_ROOT` as the path is incorrect.

I understand there are concerns about this being a breaking change, but there is no workaround in this scenario. I cannot use `uv run --script` in shebangs and resolve the project root without introducing my own environment variable which I really do not want to do. With the behaviour corrected, any configurations relying on the old behaviour can simply switch to `${PWD}` as pointed out in previous comments.

---

_Comment by @zanieb on 2025-07-10 13:39_

We'll need to drive https://github.com/astral-sh/uv/issues/13239 to consensus in order to reach a solution here, unfortunately. The pip compatibility issue is a major concern, on top of this being a breaking change for our existing users.

---

_Comment by @m1cr0man on 2025-07-18 21:43_

I see. Are you looking for user feedback in that issue or is it more a matter for maintainers? I'm personally happy with what you have proposed.

---

_Referenced in [pebble-dev/pebble-firmware#255](../../pebble-dev/pebble-firmware/pulls/255.md) on 2025-08-01 11:19_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:16_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:16_

---
