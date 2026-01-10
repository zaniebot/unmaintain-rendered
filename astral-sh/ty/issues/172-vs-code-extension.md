```yaml
number: 172
title: VS Code extension
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2025-03-14T10:15:18Z
updated_at: 2025-05-07T15:53:53Z
url: https://github.com/astral-sh/ty/issues/172
synced_at: 2026-01-10T02:34:09Z
```

# VS Code extension

---

_Issue opened by @MichaReiser on 2025-03-14 10:15_

We currently use Ruff's VS code extension to test Red Knot's LSP. We should publish a dedicated extension

---

_Comment by @InSyncWithFoo on 2025-03-14 19:38_

Do you actually want to do that? One of Ruff's great points is that it unifies things that used to be separate.

I can see a few advantages to publishing only one extension for both (having experience writing one myself):

* Most, if not all, users of Red Knot will also want to use Ruff. It is easier to make Red Knot optional for users of Ruff who don't want to use Red Knot than to develop two extensions simultaneously and ask users of both to install them.
* A number of underlying infrastructures (helpers, test frameworks, etc.) can be shared, as do bugfixes and feature requests, thereby lowering the maintenance cost. Having them in the same repository could help.

Disadvantages include:

* Red Knot is supposed to be a <em>standalone</em> type checker, so coupling them to one extension effectively hides Red Knot within Ruff's shadow. The repurposing cost will be high too: One might not understand why <i>Ruff</i> suddenly turns into <i>Ruff & Red Knot</i>.
* Currently, Ruff's extension gets one release for each release of Ruff. If Red Knot were to have a separate versioning scheme, the extension would need to account for both, increasing user update frequency; I have seen some reviews saying they are disturbed by this.

Then again, I'm speaking from the viewpoint of a JetBrains plugin developer who makes users do tool updates on their own. Perhaps VS Code users are more tolerant about installing different extensions?


---

_Renamed from "[red-knot] VS Code extension" to "VS Code extension" by @MichaReiser on 2025-05-07 15:26_

---

_Closed by @MichaReiser on 2025-05-07 15:53_

---
