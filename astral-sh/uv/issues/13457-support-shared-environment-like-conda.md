---
number: 13457
title: Support shared environment like conda
type: issue
state: closed
author: taoari
labels:
  - enhancement
assignees: []
created_at: 2025-05-14T16:56:15Z
updated_at: 2025-05-14T21:47:35Z
url: https://github.com/astral-sh/uv/issues/13457
synced_at: 2026-01-10T01:25:34Z
---

# Support shared environment like conda

---

_Issue opened by @taoari on 2025-05-14 16:56_

### Summary

Sometimes it can be cumbersome for uv to create an environment per script. This is especially true when involving large dependencies like pytorch, which could take several GB even for a hello world script. It would be great if uv could support shared environment like conda does. Something like:

conda create -n myenv python=3.12 <=> uv venv myproject --python 3.12 --global
conda activate myenv <=> uv activate myenv



### Example

_No response_

---

_Label `enhancement` added by @taoari on 2025-05-14 16:56_

---

_Comment by @konstin on 2025-05-14 18:03_

uv hardlinks dependency by defaults, which means that new environments are small: There is only one copy of torch stored on disk for all scripts.

---

_Comment by @taoari on 2025-05-14 21:42_

@konstin Thanks for your response. While using hard links is a possible workaround, it's not an ideal solution—it's rather clunky in practice. It would be much better if uv users had more control over how environments are organized. For example, I’d prefer all environments to be stored under ~/.uv/envs/ rather than having a separate .venv directory for each script (i.e. keep code and environment separation). Native support for configurable environment locations would make uv more attractive, especially to conda users.

---

_Comment by @charliermarsh on 2025-05-14 21:47_

It's not really a "workaround". I think hard links and some kind of centralized environment management are sort of orthogonal? If the concern is _space_, then hard links seem like a much better solution since they don't _require_ you to share an environment across projects. If the concern instead stems from particularities of your workflow, that's different. I think you want https://github.com/astral-sh/uv/issues/1495 though.


---

_Closed by @charliermarsh on 2025-05-14 21:47_

---
