---
number: 8895
title: "Formatter: `allow_form_feeds` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - needs-decision
  - preview
assignees: []
created_at: 2023-11-29T02:39:04Z
updated_at: 2023-12-22T05:55:27Z
url: https://github.com/astral-sh/ruff/issues/8895
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: `allow_form_feeds` preview style

---

_Issue opened by @MichaReiser on 2023-11-29 02:39_

Implement Black's [`allow_form_feeds`](https://github.com/psf/black/pull/4021) style as a preview style in Ruff. 

Black now retains form feed characters on module level on otherwise empty lines. Supporting this would be nice but adds some complexity because we need to check the source text to identify from feed characters. 

I consider this a nice to have but not a must for the stable release of the formatter.

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 02:39_

---

_Renamed from "`allow_form_feeds`" to "Formatter: `allow_form_feeds` preview style" by @MichaReiser on 2023-11-29 02:39_

---

_Label `formatter` added by @MichaReiser on 2023-11-29 02:39_

---

_Label `needs-decision` added by @MichaReiser on 2023-11-29 02:39_

---

_Label `preview` added by @MichaReiser on 2023-11-29 02:39_

---

_Comment by @MichaReiser on 2023-12-22 05:55_

This is a neat feature but rather nish. I'm happy to provide help if someone's interested in implementing this. I'll close this in the meantime to clarify that it is unplanned.

---

_Closed by @MichaReiser on 2023-12-22 05:55_

---
