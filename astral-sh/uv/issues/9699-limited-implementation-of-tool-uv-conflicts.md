---
number: 9699
title: "Limited Implementation of `tool.uv.conflicts`"
type: issue
state: closed
author: NellyWhads
labels: []
assignees: []
created_at: 2024-12-07T00:17:31Z
updated_at: 2024-12-07T00:27:05Z
url: https://github.com/astral-sh/uv/issues/9699
synced_at: 2026-01-10T01:24:44Z
---

# Limited Implementation of `tool.uv.conflicts`

---

_Issue opened by @NellyWhads on 2024-12-07 00:17_

Currently, `tool.uv.conflicts` only supports `project.optional-dependencies.{extra}` and `dependency-groups.{group}`. Can we have support for `project.optional-dependencies.{group}` as well?

Docs link: https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies

---

_Comment by @charliermarsh on 2024-12-07 00:18_

What is `project.optional-dependencies.{group}`?

---

_Comment by @NellyWhads on 2024-12-07 00:19_

Am I blind ... are they just extras ...? 

https://hatch.pypa.io/1.9/config/dependency/#self-referential

---

_Comment by @charliermarsh on 2024-12-07 00:20_

Haha. In that context, `awesome-project[crypto]` is referring to the `crypto` extra defined just above.

---

_Comment by @NellyWhads on 2024-12-07 00:21_

I think they are. You right. I'm blind.

In this case, where can I learn about the difference between groups and extras, and when each should be used?

---

_Comment by @NellyWhads on 2024-12-07 00:25_

Scratch that, figured it out: https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-tables

I've been staring at the screen too long. It's time for a nap. Thanks @charliermarsh 

---

_Closed by @NellyWhads on 2024-12-07 00:25_

---

_Comment by @charliermarsh on 2024-12-07 00:27_

No worries, glad it was easy to resolve :)

---
