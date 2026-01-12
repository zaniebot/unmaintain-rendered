```yaml
number: 523
title: "wincolor: Re-fetch the console on all calls"
type: pull_request
state: merged
author: alexcrichton
labels: []
assignees: []
merged: true
base: master
head: no-close-windows
created_at: 2017-06-19T15:45:42Z
updated_at: 2017-06-19T17:27:46Z
url: https://github.com/BurntSushi/ripgrep/pull/523
synced_at: 2026-01-12T18:23:13Z
```

# wincolor: Re-fetch the console on all calls

---

_@alexcrichton_

The primary motivation for this commit was rust-lang/cargo#4189 where dropping a
`wincolor::Console` would call `CloseHandle` to close the console handle. Cargo
creates a few `Console` instances so it ended up closing stdout a little
earlier as intended!

The `GetStdHandle` function returns handles I believe aren't intended to be
closed (as there's no refcounting). I believe libstd doesn't close these
handles.

This commit also moves to calling `GetStdHandle` on demand which libstd changed
to doing so recently as well, preventing caching of stale handles that change
over time with calls to `SetStdHandle`.

---

_Merged by @BurntSushi on 2017-06-19 16:57_

---

_Closed by @BurntSushi on 2017-06-19 16:57_

---

_Comment by @BurntSushi on 2017-06-19 16:58_

All right, I buy it!

I feel like there was a reason why I wasn't re-creating the console, but my guess is that it was an attempt at a premature optimization.

---

_Branch deleted on 2017-06-19 17:11_

---

_Comment by @alexcrichton on 2017-06-19 17:11_

Thanks! Mind publishing a new version as well?

---

_Comment by @BurntSushi on 2017-06-19 17:27_

Ah right, `0.1.4` is on crates.io!

---
