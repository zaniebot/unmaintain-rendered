```yaml
number: 21589
title: "shift-clicking website links doesn't open in new window"
type: issue
state: closed
author: injust
labels: []
assignees: []
created_at: 2025-11-23T10:37:53Z
updated_at: 2025-11-23T11:28:04Z
url: https://github.com/astral-sh/ruff/issues/21589
synced_at: 2026-01-12T15:54:57Z
```

# shift-clicking website links doesn't open in new window

---

_@injust_

### Summary

shift-clicking links on https://docs.astral.sh/ruff/ opens them in the same tab. Instead, it should open in a new window.

If you shift-click the `Docs` link on https://docs.astral.sh/ruff/ (which goes to the same page), it is a no-op.

### Version

_No response_

---

_Comment by @MichaReiser on 2025-11-23 11:00_

I don't think this is something we can fix. Cmd clicking the `Docs` link opens it in a new page. Shift clicking does indeed do "nothing" but this seems a browser decision.

---

_Closed by @MichaReiser on 2025-11-23 11:00_

---

_Comment by @injust on 2025-11-23 11:10_

This is not a browser decision; this happens because the docs site is _overriding_ the browser decision, likely a side effect of how the SPA implements page navigation (instead of loading the entire page, the content is changed).

Try shift-clicking the `Playground` link on https://docs.astral.sh/ruff/. It works correctly because the link is external to https://docs.astral.sh/ruff/.

---

_Comment by @MichaReiser on 2025-11-23 11:26_

Maybe, I don't know. I did a quick search for any javascript and couldn't find any that would intercept the link. It could also be something from our documentation framework. I don't think this is worth me spending much time on hunting down as we're considering switching the documentation framework anyway.

---
