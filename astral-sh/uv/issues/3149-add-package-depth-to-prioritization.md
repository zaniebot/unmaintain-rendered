```yaml
number: 3149
title: "Add \"package depth\" to prioritization"
type: issue
state: open
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-04-19T23:17:29Z
updated_at: 2024-11-07T14:49:30Z
url: https://github.com/astral-sh/uv/issues/3149
synced_at: 2026-01-12T15:58:42Z
```

# Add "package depth" to prioritization

---

_@charliermarsh_

After "directly pinned packages", pip breaks ties based on package depth: https://github.com/pypa/pip/blob/ef78c129b1a966dbbbdb8ebfffc43723e89110d1/src/pip/_internal/resolution/resolvelib/provider.py#L120. We don't have a straightforward measurement for this right now, so it's not included in the heuristics. (Instead, we track "the order in which we visit packages", which isn't the same thing.)

---

_Label `performance` added by @charliermarsh on 2024-04-19 23:17_

---

_Comment by @wakamex on 2024-06-18 13:19_

I'd love to see a slower "bleeding edge" resolution mode whose goal is to install latest versions as much as possible.  To do this reliably, you'd need to visit all direct dependencies and pull up the release date of their one-back version, in order to backtrack to the most recent.

It would be helpful for personal projects where my aim is to try out the latest version of everything and help teams with issues as they arise.

It might be useful more generally too, to avoid race-to-the-bottom backtracks. Basically switching from depth-first to breadth-might resolve faster and with a happier outcome of versions that work well together since they were released around the same time. 

See #4372 for an example of dependency hell that is happily resolved with a reliable "most recent" trackback.

---

_Comment by @notatallshaw on 2024-11-07 14:49_

FWIW, I don't think depth is a good heuristic in terms of finding a solution faster. It actually got [broken ](https://github.com/pdm-project/pdm/pull/3235#issuecomment-2451150062) in the recent [resolvelib release](https://github.com/sarugaku/resolvelib/blob/main/CHANGELOG.rst#110-2024-10-31) and my testing of fixing it I find it makes pip worse at resolving than just removing it.

> I'd love to see a slower "bleeding edge" resolution mode whose goal is to install latest versions as much as possible. To do this reliably, you'd need to visit all direct dependencies and pull up the release date of their one-back version, in order to backtrack to the most recent.

I would request this seperately as it's not really related to depth. A preference on "upload-time" might be possible without doing everything you say, but it would depend on the Simple Index 1.1 (i.e. PEP 700) and lots of non-PyPI indexes do not do that, so it might be fairly unreliable for lots of use cases.

---
