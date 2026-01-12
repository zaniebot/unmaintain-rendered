```yaml
number: 340
title: clippy should be a dev-dependency
type: issue
state: closed
author: nagisa
labels: []
assignees: []
created_at: 2015-11-09T07:04:55Z
updated_at: 2018-08-02T03:29:46Z
url: https://github.com/clap-rs/clap/issues/340
synced_at: 2026-01-12T16:14:09Z
```

# clippy should be a dev-dependency

---

_@nagisa_

clippy probably should be a dev-dependency. There’s no reason to pull in clippy for every build of clap even if optionally, if clap is not build for development purposes.


---

_Comment by @nagisa on 2015-11-09 07:23_

Never mind, I guess.


---

_Closed by @nagisa on 2015-11-09 07:23_

---

_Comment by @kbknapp on 2015-11-09 07:32_

I've also considered trying to use something like`cargo-clippy` instead of listing as an optional dep. But this way has the added benefit of running the lints as part of the Travis setup and blocking a merge if it finds something. 


---
