---
number: 8248
title: "Proposal: HTML output file"
type: issue
state: open
author: ryan-cpi
labels:
  - wish
  - linter
assignees: []
created_at: 2023-10-26T10:04:49Z
updated_at: 2024-11-11T18:58:32Z
url: https://github.com/astral-sh/ruff/issues/8248
synced_at: 2026-01-07T13:12:15-06:00
---

# Proposal: HTML output file

---

_Issue opened by @ryan-cpi on 2023-10-26 10:04_

Would it be possible to support an output-format of HTML where the rules ID/description link back to Ruff rules page e.g [F401](https://docs.astral.sh/ruff/rules/unused-import/) and the lines/files have clickable links going into the files its raising issues against. Having a summary at the end of this file might also be nice here.

Something blended between the grouped output and HTML would be fitting for CI artefacts. 


---

_Label `wish` added by @MichaReiser on 2023-10-26 23:44_

---

_Label `linter` added by @MichaReiser on 2023-10-26 23:45_

---

_Comment by @Dalenir on 2024-02-28 14:19_

Supporting this request: sometimes fully-contained html report might be much more prefferable to other formats.

---

_Comment by @JCHacking on 2024-04-24 07:50_

I would love this feature!!!
I currently use flake8 with quite a few plugins, and I don't want to switch until I can generate an HTML report.

---

_Comment by @aardjon on 2024-11-11 18:58_

Until this feature is implemented, you may want to try https://pypi.org/project/ciqar/ which can create HTML reports from ruff output.

---
