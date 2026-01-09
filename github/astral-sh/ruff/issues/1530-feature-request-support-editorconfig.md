---
number: 1530
title: "[Feature request] Support `.editorconfig`"
type: issue
state: closed
author: rassie
labels:
  - configuration
assignees: []
created_at: 2023-01-01T15:10:59Z
updated_at: 2025-05-10T16:09:40Z
url: https://github.com/astral-sh/ruff/issues/1530
synced_at: 2026-01-07T13:12:14-06:00
---

# [Feature request] Support `.editorconfig`

---

_Issue opened by @rassie on 2023-01-01 15:10_

Would you be open to supporting `.editorconfig` for the `line-length` setting (called `max_line_length`)?

Currently, it's a small chore to setup the `line-length` correctly -- `black`, `isort` and `ruff` and also the editor need them separately (editor needs the setting for reflowing comments, which `black` doesn't do). Introducing `.editorconfig` to a project solves the editor and the `isort` problem and  if `ruff` supported it too, there would only be two settings to change if needed (one if `ruff` really becomes a formatter too).

(Caveat: `max_line_length` is somehow still not part of the EditorConfig spec a decade later, but is apparently widely supported by editors). 

---

_Label `configuration` added by @charliermarsh on 2023-01-02 18:37_

---

_Comment by @andersk on 2023-01-03 03:50_

Sort of a corresponding issue for Black:

- psf/black#2401

but maybe someone should open a better one.

---

_Comment by @silverwind on 2023-05-12 17:53_

`max_line_length` is unfortunately a [non-standard](https://editorconfig.org/#file-format-details) editorconfig option that may not be so widely supported.

But I do see editorconfig useful for formatting (specifically `indent_size`, but almost all standard properties are useful to a formatter) and it should be used when other configurations options are absent, e.g. at the lowest priority.

---

_Comment by @charliermarsh on 2023-05-12 19:21_

I'm currently hesitant to support `.editorconfig` since I'd really like to avoid supporting multiple configuration formats and having to merge settings defined across multiple configuration formats.


---

_Comment by @silverwind on 2023-05-12 19:24_

Fine with me, I don't mind a explicit separate config, even if it's going to be duplicate with the `.editorconfig`, which imho every repo must have.

---

_Comment by @charliermarsh on 2023-05-18 15:50_

Closing for now as "not planned".

---

_Closed by @charliermarsh on 2023-05-18 15:50_

---

_Referenced in [AIToolsLab/writing-tools#81](../../AIToolsLab/writing-tools/issues/81.md) on 2024-05-14 21:10_

---

_Comment by @dbarnett on 2024-08-23 21:31_

Hmm, I understand not wanting unnecessary complexity, but it's unfortunate to leave the situation where users can end up specifying conflicting settings in different industry-standard configs and have them silently warring each other (e.g. editor indenting to 4, then ruff format re-indenting everything to 2, then editor creating more 4-space indents).

---

_Referenced in [patrick-kidger/equinox#821](../../patrick-kidger/equinox/pulls/821.md) on 2024-08-30 11:57_

---

_Referenced in [psf/black#4487](../../psf/black/issues/4487.md) on 2024-10-16 23:27_

---

_Referenced in [data-apis/array-api-typing#5](../../data-apis/array-api-typing/pulls/5.md) on 2024-11-30 20:39_

---

_Referenced in [astral-sh/ruff#14865](../../astral-sh/ruff/issues/14865.md) on 2024-12-09 12:51_

---

_Referenced in [acdh-oeaw/apis-core-rdf#1811](../../acdh-oeaw/apis-core-rdf/pulls/1811.md) on 2025-04-23 07:16_

---

_Comment by @heygarrett on 2025-05-01 23:05_

Can I nominate this for reconsideration? I would love to avoid adding an additional configuration file to a project that already has an EditorConfig file and would benefit from using Python as an alternative to Bash scripts.

---

_Comment by @Qix- on 2025-05-10 16:09_

If nothing else, being able to generate an editorconfig based on the ruff formatting rules would be great. Was missing that today.

---
