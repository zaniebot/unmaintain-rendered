---
number: 7672
title: How to correctly automatically version bump
type: issue
state: closed
author: yannsartori
labels:
  - documentation
assignees: []
created_at: 2024-09-24T20:22:58Z
updated_at: 2025-01-24T19:32:31Z
url: https://github.com/astral-sh/uv/issues/7672
synced_at: 2026-01-10T01:24:17Z
---

# How to correctly automatically version bump

---

_Issue opened by @yannsartori on 2024-09-24 20:22_

Hello,

I currently use [bumpver](https://github.com/mbarkhau/bumpver) to automatically version bump my project in CI (realistically, all this tool does from a "versioning" perspective is search and replacing). In particular, it updates the `version` key in my `pyproject.toml` file.

The flow is thus merge new feature into main as its own commit -> automatically change these files with version specifiers (just `pyproject.toml`) -> commit the changes as a version bump change.

However, I noticed by manually inspecting my `uv.lock` that it also maintains a `version` key for my project. I try to run all my commands with `--frozen` to keep my `uv.lock` file as unchanged as possible, so this `version` key can be quite out of date from the one present in `pyproject.toml` after a series of version bumps.

My question for you all is how to best manage this? A couple of solutions I have thought of:

1. Just ignore this problem out right, and let the version in the `uv.lock` be out of date. I'm guessing this can lead to complications down the road
2. Also make `bumpver` update the `uv.lock` file manually. Since there  is a big disclaimer about not editing the lock file manually, I am hesitant to do this
3. Regenerate the lock file after I bump the version. This seems to defeat the purpose of using a lock file at all, if I am updating it in CI.
4.  I looked into `uv lock --upgrade-package NAME_OF_PROJECT`. My hope is it would just update the package information in the lock file, and NOT its dependencies, but I couldn't find any exact information on its behavior.

I know there is an issue to add capabilities for uv to manage versions for you automatically (https://github.com/astral-sh/uv/issues/6298) but my question is how to manage updating versions in general without this.

---

_Comment by @zanieb on 2024-09-24 20:25_

Yeah, I think you're looking for `uv lock --upgrade-package <project>`.

I think @charliermarsh recently fielded a similar question to this.

---

_Comment by @charliermarsh on 2024-09-25 17:38_

We can probably add some docs for this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-25 17:38_

---

_Label `documentation` added by @charliermarsh on 2024-09-25 17:38_

---

_Comment by @yannsartori on 2024-09-25 18:23_

Awesome, thanks for the quick reply @zanieb! I'll pursue that solution then. I'll leave this open if you guys want to create the documentation PR off this issue, but my question was answered.

---

_Comment by @charliermarsh on 2024-09-25 18:24_

👍 Thanks for following up. I'll improve the docs a bit and will use this issue to track that work.

---

_Referenced in [astral-sh/uv#7694](../../astral-sh/uv/pulls/7694.md) on 2024-09-25 22:14_

---

_Closed by @charliermarsh on 2024-09-25 22:17_

---

_Closed by @charliermarsh on 2024-09-25 22:17_

---

_Comment by @zkurtz on 2025-01-15 02:45_

For @yannsartori's particular use case, where the `uv.lock` is out of sync with `pyproject.toml` due to an update of the project version, isn't `uv sync` sufficient and equivalent to `uv lock --upgrade-package <project>`?

---

_Comment by @trallnag on 2025-01-24 19:32_

@zkurtz indeed it seems to be sufficient now. It happened somewhere between in > 0.5.20 and <= 0.5.24. Before that I had to run `uv lock --upgrade-package <project>`

---
