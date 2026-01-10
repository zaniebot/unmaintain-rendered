---
number: 16030
title: Prevent accidentally publishing multiple versions of the same package
type: issue
state: open
author: konstin
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-09-25T19:22:49Z
updated_at: 2025-10-16T07:54:29Z
url: https://github.com/astral-sh/uv/issues/16030
synced_at: 2026-01-10T01:26:02Z
---

# Prevent accidentally publishing multiple versions of the same package

---

_Issue opened by @konstin on 2025-09-25 19:22_

We regularly get questions where users accidentally try to publish more versions of a package because they had an old `dist` around (https://github.com/astral-sh/uv/issues/8033, https://github.com/astral-sh/uv/issues/13185, https://github.com/astral-sh/uv/issues/16028, https://python.plainenglish.io/overcoming-uvs-own-shortcomings-with-uv-02fe47885358). We should try to safeguard better against this.

There is a question whether this fits better into `uv build` or `uv publish`. One option is to have `uv build` remove old versions of the same package, unless `--keep-previous` is passed. This is a breaking change. Another option is that `uv publish` checks if there are multiple versions, and if so, prompts for confirmation, unless `--all-versions` is passed, similar to `uv pip install requirements.txt` or `uv venv`.

---

_Label `enhancement` added by @konstin on 2025-09-25 19:22_

---

_Label `needs-design` added by @konstin on 2025-09-25 19:22_

---

_Comment by @m1guelperez on 2025-09-26 18:51_

I think I would like to tackle this issue, as my first uv contribution. Could I just go with the --keep-previous approach? 

---

_Comment by @konstin on 2025-09-29 11:09_

@m1guelperez I'm afraid this issue isn't yet a good first contribution opportunity, we first need to internally figure out which of the approaches we want to take, it's a difficult part of the uv design. Note that `--keep-previous` is a breaking change, so we'd only be able to merge this for the 0.9.0 release anyway.

---

_Comment by @m1guelperez on 2025-09-29 19:41_

@konstin alright. Maybe you could keep me up to date when you have internally decided on an approach? I would than eventually take up the issue again :) 

---

_Comment by @joukewitteveen on 2025-10-16 07:54_

Another option would be to add an option to `uv build` for producing machine-readable output. If every package produced would be printed on a line of its own (or separated by `\0` if no human-readability is needed), it becomes possible to iterate over the built packages and upload them. Additionally, no script would need to hardcode `dist/` anymore and that default could in principle be changed.

---
