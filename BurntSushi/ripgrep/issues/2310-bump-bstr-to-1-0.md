```yaml
number: 2310
title: bump bstr to 1.0
type: issue
state: closed
author: pascalkuthe
labels: []
assignees: []
created_at: 2022-09-21T16:49:29Z
updated_at: 2023-03-04T16:02:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2310
synced_at: 2026-01-12T16:13:24Z
```

# bump bstr to 1.0

---

_@pascalkuthe_

Bstr version 1.0 was released with a new MSRV of 1.60.
I understand from the README that ripgrep has a MSRV of 1.34 so upgrading isn't directly an option..
However many rust project that depend on components of ripgrephave an MSRV of 1.60 (or above).
It would be nice if a new version of the ripgrep crates were released that bump bstr 1.0.

Currently any crate that depends on ripgrep components can not update bstr (or any crate that depends on it) without compiling two versions of bstr.

This problem occurred in https://github.com/helix-editor/helix/pull/3890.
Here we wanted to update gitoxide to 0.24, which depends on bstr 1.0 but chose to postpone because globset still depends on bstr 0.2.

I originally wanted to open a PR, however as ripgrep is developed in a workspace bumping bstr would mean bumping the MSRV of ripgrep aswell. Might it be possible to add an optional feature to switch to bstr 1.0?

---

_Comment by @stmcginnis on 2023-03-04 15:09_

Appears this issue can now be closed too.

---

_Closed by @pascalkuthe on 2023-03-04 16:02_

---
