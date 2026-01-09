---
number: 1774
title: No changelog after 0.1.3
type: issue
state: closed
author: gaborbernat
labels:
  - documentation
assignees: []
created_at: 2024-02-20T18:40:58Z
updated_at: 2024-02-22T22:00:14Z
url: https://github.com/astral-sh/uv/issues/1774
synced_at: 2026-01-07T13:12:16-06:00
---

# No changelog after 0.1.3

---

_Issue opened by @gaborbernat on 2024-02-20 18:40_

Seems there's no longer a changelog anywhere past 0.1.3. https://github.com/astral-sh/uv/releases/tag/0.1.3. It would be nice to have a place to easily see what changes are in new versions.

---

_Comment by @charliermarsh on 2024-02-20 18:42_

We're gonna add the same changelog setup as in Ruff, hopefully this week!

---

_Comment by @zanieb on 2024-02-20 18:42_

We'll be adding a real changelog soon.

---

_Label `documentation` added by @zanieb on 2024-02-20 18:42_

---

_Assigned to @zanieb by @zanieb on 2024-02-20 18:42_

---

_Comment by @zanieb on 2024-02-20 18:42_

@charliermarsh should we leave the cargo-dist ones on until then?

---

_Comment by @orhun on 2024-02-20 18:48_

Hey! I also realized there was no changelog and found this issue.

You can consider [`git-cliff`](https://git-cliff.org/) for automated generation of the changelog. As the author of the tool, I'm happy provide any guidance about it! 🐻 

---

_Comment by @zanieb on 2024-02-21 00:16_

We don't follow conventional commits, instead we use pull request labels on GitHub to sort entries.

---

_Comment by @orhun on 2024-02-21 08:52_

With the [last version](https://git-cliff.org/blog/2.0.0#-github-integration) of `git-cliff` that's also possible! Are you using the GitHub's changelog format? I would like to give it a shot to generate the changelog in the style you prefer - I'm also curious how that would turn out.

---

_Referenced in [astral-sh/uv#1881](../../astral-sh/uv/pulls/1881.md) on 2024-02-22 19:42_

---

_Closed by @zanieb on 2024-02-22 22:00_

---
