---
number: 11472
title: "Feature request: linkcheck"
type: issue
state: open
author: nstarman
labels:
  - rule
assignees: []
created_at: 2024-05-19T22:57:38Z
updated_at: 2024-05-27T11:13:23Z
url: https://github.com/astral-sh/ruff/issues/11472
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: linkcheck

---

_Issue opened by @nstarman on 2024-05-19 22:57_

I'm not sure if this is out of scope for `ruff`, but it would be awesome to have a means to auto-fix links that now redirect.
Like many repos, we at Astropy use [Sphinx's link checker](https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-the-linkcheck-builder) to ensure links are working. They are, but our logs are chock full of warning about link redirects. No one has the bandwidth to manually fix them all, so it would be great if a CI tool could do this automatically! Even if a ruff-based fix was implemented only for Python docstrings, it would be immensely useful.

---

_Comment by @augustelalande on 2024-05-20 02:06_

Probably out of scope, since the only way to check it would be to actually lookup the link, which would be timely.

---

_Comment by @nstarman on 2024-05-20 13:08_

If it's out of scope for ruff, does anyone know of a tool that might accomplish this? 

---

_Comment by @santacodes on 2024-05-26 17:57_

> If it's out of scope for ruff, does anyone know of a tool that might accomplish this?

I think you should check out [lychee](https://github.com/lycheeverse/lychee). It can integrate with CI as well, but I am not sure if it auto-fixes links.

---

_Label `rule` added by @MichaReiser on 2024-05-27 11:13_

---
