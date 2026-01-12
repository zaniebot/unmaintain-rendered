```yaml
number: 1316
title: "[feature-request] Environment variable interpolation for config paths"
type: issue
state: closed
author: smackesey
labels:
  - configuration
assignees: []
created_at: 2022-12-21T23:03:06Z
updated_at: 2022-12-22T03:12:30Z
url: https://github.com/astral-sh/ruff/issues/1316
synced_at: 2026-01-12T15:54:41Z
```

# [feature-request] Environment variable interpolation for config paths

---

_@smackesey_

Environment variable interpolation in certain config settings could be very useful, specifically in the `extend` field.

Our use case is that we have a second internal repo where we've had to maintain a copy in `pyproject.toml` of the config from our main repo. But ideally we could just do this:

```
[tool.ruff]
extend` = "${MAIN_REPO}/pyproject.toml"
# ... a few internal-specific settings
```

This becomes more useful the more repos you have.

---

_Renamed from "Environment variable interpolation for config paths" to "[feature-request] Environment variable interpolation for config paths" by @smackesey on 2022-12-21 23:03_

---

_Comment by @charliermarsh on 2022-12-21 23:32_

This seems pretty straightforward, do you know of other tools that support this kind of thing?

---

_Label `enhancement` added by @charliermarsh on 2022-12-21 23:35_

---

_Label `configuration` added by @charliermarsh on 2022-12-21 23:35_

---

_Comment by @charliermarsh on 2022-12-21 23:36_

It looks like Mypy does this: https://mypy.readthedocs.io/en/stable/config_file.html#confval-files

---

_Comment by @charliermarsh on 2022-12-21 23:36_

Will add

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-21 23:36_

---

_Comment by @charliermarsh on 2022-12-22 01:46_

Starting with `extend` for expediency and simplicity, but let me know if you need this for other paths too.

---

_Closed by @charliermarsh on 2022-12-22 01:46_

---

_Comment by @charliermarsh on 2022-12-22 02:03_

(`src` also supports this now.)

---

_Comment by @smackesey on 2022-12-22 02:13_

Sweet-- just wanna say you're doing an a fantastic job building a great tool for teams working on monorepos (not to mention all the other great stuff about ruff). You've implemented basically everything on my wishlist to date.

I try to get the pyright maintainer (the other big tool we're going to be using) to implement a lot of this simple quality-of-life stuff but he rejects virtually every request and IMO pyright is consequently full of user-unfriendly APIs and blind-spots. In contrast you're typically knocking these out the day I raise them. Great job, this will help us a lot with Dagster when the transition PR merges. I'll definitely write a post on the Dagster blog about it.

---

_Comment by @charliermarsh on 2022-12-22 03:12_

Thank you! Really happy to hear it. From my perspective, it's been super beneficial for _Ruff_ too to have all of your feedback and input, and I'm grateful for it. Your issues are always very thoughtful and well-communicated, and the work we're doing here will be a huge boon to other projects (especially monorepos) in the future.

> I'll definitely write a post on the Dagster blog about it.

This would be extremely cool, I would of course love to see that :)


---
