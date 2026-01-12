```yaml
number: 264
title: Apply formatting to hover information
type: issue
state: closed
author: InSyncWithFoo
labels:
  - server
assignees: []
created_at: 2025-05-08T08:04:56Z
updated_at: 2025-08-15T12:43:04Z
url: https://github.com/astral-sh/ty/issues/264
synced_at: 2026-01-12T15:54:22Z
```

# Apply formatting to hover information

---

_@InSyncWithFoo_

Here's what ty has to say about the type of `pydantic.Field`:

![](https://github.com/user-attachments/assets/fdfbcd42-e160-4b34-b5bd-b895e8110923)

For reference, here's Pyright's:

![](https://github.com/user-attachments/assets/685105da-4d6e-44b1-857c-951d5399481a)

There are two problems here:

* There is no syntax highlighting.

  [The code block's language is `text`](https://github.com/astral-sh/ruff/blob/d566636ca5b86b90b9ff5e292f7fbb2598691c47/crates/ty_ide/src/hover.rs#L124), but it should be `python`. I understand [ty's type information is not always valid Python](https://github.com/astral-sh/ruff/pull/17057#discussion_r2028151621), but for all intents and purposes it might as well be considered as such. I'd also like to add that ty should put more trust in syntax highlighters, which are often written with invalid code in mind.

* The formatting is unreadable.

  I feel like it should be possible to run Ruff's formatter on ty's output, especially syntactically valid ones like `Literal["some", "very", "long", "names"]`.


---

_Label `server` added by @MichaReiser on 2025-05-08 08:06_

---

_Comment by @Gankra on 2025-08-13 16:34_

<img width="960" height="317" alt="Image" src="https://github.com/user-attachments/assets/8d8c3409-db77-47e1-a937-ee7429da5505" />

We've since added python highlighting but are still dropping the ball on pretty-printing the type (this scrolls for a long while, eventually you also hit docs).

---

_Assigned to @Gankra by @Gankra on 2025-08-13 16:34_

---

_Comment by @InSyncWithFoo on 2025-08-13 22:11_

@Gankra Appreciate the update, and thanks for your recent work on new server features. I have been seeing syntax highlighting, but no docstrings anywhere. Is there something I need to set client-side to get that to work? Neither [the docs](https://docs.astral.sh/ty/reference/editor-settings/) nor [the PR](https://github.com/astral-sh/ruff/pull/19882) mentions such a thing.

I'm using [a self-built version of ty](https://github.com/InSyncWithFoo-Sandbox/ty/actions/runs/16950026260) on PyCharm, if that matters.

Here's a screenshot, for debugging and entertainment:

<img width="1345" height="742" alt="Image" src="https://github.com/user-attachments/assets/1610e0b3-db9a-4b70-85e5-19289d9e4ed2" />

---

_Comment by @Gankra on 2025-08-14 13:53_

The docstring stuff landed like, a day or two ago so unless you have the latest build of main you won't see it, but otherwise it should Just Work?

---

_Comment by @InSyncWithFoo on 2025-08-14 15:54_

@Gankra I did in fact have the latest build of the main branch, unless something was wrong with `gh run download`. On the other hand, 0.0.1-alpha.18 does work:

<img width="902" height="530" alt="Image" src="https://github.com/user-attachments/assets/01389cf6-bc01-409c-ba9a-56ba0da1ce3d" />

Not pretty by any means, but certainly an improvement over Pyright's:

<img width="1197" height="628" alt="Image" src="https://github.com/user-attachments/assets/27a1bb3d-abab-4069-b1ed-3ae758ea3ef8" />

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:04_

---

_Comment by @MichaReiser on 2025-08-15 12:43_

I'll merge this into https://github.com/astral-sh/ty/issues/1000, now that we do apply some basic formatting (I'll create another issue for docstring formatting)

---

_Closed by @MichaReiser on 2025-08-15 12:43_

---
