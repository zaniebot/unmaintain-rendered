---
number: 2322
title: Make some background for the benchmark image
type: issue
state: closed
author: chirizxc
labels:
  - documentation
assignees: []
created_at: 2026-01-04T20:51:30Z
updated_at: 2026-01-09T20:35:28Z
url: https://github.com/astral-sh/ty/issues/2322
synced_at: 2026-01-10T01:48:23Z
---

# Make some background for the benchmark image

---

_Issue opened by @chirizxc on 2026-01-04 20:51_

https://pypi.org/project/ty/

If we go to PyPi, we can't see any text in the benchmark at all

<img width="905" height="557" alt="Image" src="https://github.com/user-attachments/assets/10de895c-d25b-438a-8214-e59313bcf6be" />

with [Dark Reader](https://darkreader.org/):

<img width="880" height="710" alt="Image" src="https://github.com/user-attachments/assets/49716006-9f2c-4cc8-a873-58df6dface98" />

I think if the image is generated with a white background, it will look better.

---

_Comment by @chirizxc on 2026-01-04 20:53_

It seems that this photo is also used in the documentation, in which case two photos will need to be generated🤔

---

_Label `documentation` added by @AlexWaygood on 2026-01-04 21:34_

---

_Comment by @MichaReiser on 2026-01-05 08:59_

We solved this in ruff (and I think also uv) by rewriting the README before publishing, see 
https://github.com/astral-sh/ruff/blob/388658efdb088e517bb127dce3f016c976cdd273/scripts/transform_readme.py#L7-L44

We should update ty's `transform_readme.py` and add a similar transformation step.

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-05 08:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-09 19:09_

---

_Comment by @zanieb on 2026-01-09 19:24_

I can't reproduce this in Firefox or Vivaldi. The image has black text, not white? 

edit: Clarified in https://github.com/astral-sh/ty/pull/2422#issuecomment-3730282665

---

_Closed by @charliermarsh on 2026-01-09 20:35_

---
