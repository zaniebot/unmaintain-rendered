---
number: 12185
title: ruff format ignores notebooks unless specified
type: issue
state: closed
author: Wazarr
labels: []
assignees: []
created_at: 2024-07-04T09:41:08Z
updated_at: 2024-07-04T11:39:13Z
url: https://github.com/astral-sh/ruff/issues/12185
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff format ignores notebooks unless specified

---

_Issue opened by @Wazarr on 2024-07-04 09:41_

Hi,

I have noticed when using the CLI that doing `ruff format .` ignores Jupyter notebooks.
I believe this a deviation from black, which effectively formats notebooks with `black .`, with the right dependencies installed.

It's only possible to format notebooks when the filename is provided, which forces me to iterate over files to format all my notebooks.
I have looked at the documentation and other issues, and I'm on the latest version of Ruff (v0.5.0).

Would it be possible to add an option to fix this issue?
Thanks

---

_Comment by @MichaReiser on 2024-07-04 11:32_

Can you try using `extend-include = ["*.ipynb"]` in your configuration? This way, notebooks should also be included when running `ruff check` ([doc](https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery))

---

_Comment by @Wazarr on 2024-07-04 11:35_

Apologies, only looked at CLI.
Thanks for the quick answer.

---

_Closed by @Wazarr on 2024-07-04 11:35_

---

_Comment by @MichaReiser on 2024-07-04 11:39_

No worries. The notebook documentation is fairly hidden away. We do have an issue with improving the discoverability. 

---

_Referenced in [astral-sh/ruff#12456](../../astral-sh/ruff/issues/12456.md) on 2024-07-22 16:26_

---
