```yaml
number: 2069
title: "fix: pin rustpython to the same revision to fix cargo vendor"
type: pull_request
state: merged
author: figsoda
labels: []
assignees: []
merged: true
base: main
head: pin
created_at: 2023-01-21T19:32:01Z
updated_at: 2023-01-21T19:41:37Z
url: https://github.com/astral-sh/ruff/pull/2069
synced_at: 2026-01-12T15:55:07Z
```

# fix: pin rustpython to the same revision to fix cargo vendor

---

_@figsoda_

I was trying to update ruff in nixpkgs and ran into this error when it was running `cargo vendor`
```
error: failed to sync

Caused by:
  found duplicate version of package `rustpython-ast v0.2.0` vendored from two sources:

        source 1: https://github.com/RustPython/RustPython.git?rev=62aa942bf506ea3d41ed0503b947b84141fdaa3c#62aa942b
        source 2: https://github.com/RustPython/RustPython.git?rev=ff90fe52eea578c8ebdd9d95e078cc041a5959fa#ff90fe52
```

---

_Comment by @charliermarsh on 2023-01-21 19:33_

Ah thank you, sorry, total oversight on my part. Will you be able to update it without a new PyPI release? (Assuming yes, if you're running `cargo vendor`.)

---

_Comment by @figsoda on 2023-01-21 19:38_

We use the github tagged version, so maybe something like a `v0.0.229-vendor` tag would help. I can always just wait for the next release when there are more notable changes since ruff seems to get updates pretty frequently

---

_Merged by @charliermarsh on 2023-01-21 19:40_

---

_Closed by @charliermarsh on 2023-01-21 19:40_

---

_Comment by @charliermarsh on 2023-01-21 19:40_

Okay sounds good. Yeah, we'll probably release again tomorrow.

---

_Branch deleted on 2023-01-21 19:41_

---
