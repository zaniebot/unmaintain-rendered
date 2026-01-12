```yaml
number: 539
title: Make columns indices 1-based in the text output format
type: pull_request
state: merged
author: fsouza
labels: []
assignees: []
merged: true
base: main
head: 1-indexed-columns-on-error-reporting
created_at: 2022-11-01T22:10:06Z
updated_at: 2022-11-03T02:00:15Z
url: https://github.com/astral-sh/ruff/pull/539
synced_at: 2026-01-12T05:48:45Z
```

# Make columns indices 1-based in the text output format

---

_Pull request opened by @fsouza on 2022-11-01 22:10_

Not changing the column in JSON errors. Not sure if we want to change that too.

Closes #537.

---

_Comment by @charliermarsh on 2022-11-01 22:15_

I think it'd make sense to change it in the JSON printer too if you're up for it. Should we even consider changing it when we convert from `Check` to `Message`, so that `Message` is always one-indexed?

---

_Comment by @fsouza on 2022-11-01 22:17_

@charliermarsh good call! Let me make that change.

---

_Comment by @fsouza on 2022-11-01 22:48_

@charliermarsh how does this look? I originally tried to make a copy of the `Location` directly, but the fields are private, so I ended-up copying it and then mutating with `go_right()`.

My Rust-fu is a bit limited here, let me know if I'm wasting your time 😁 

---

_Review comment by @fsouza on `src/message.rs`:26 on 2022-11-01 22:52_

this doesn't feel right 🙈 

---

_@fsouza reviewed on 2022-11-01 22:52_

---

_Review comment by @charliermarsh on `src/message.rs`:26 on 2022-11-01 23:01_

I think you can use Location::new and location.row() methods to create a new location here! (On subway so low-fidelity message…)

---

_@charliermarsh reviewed on 2022-11-01 23:01_

---

_@fsouza reviewed on 2022-11-01 23:03_

---

_Review comment by @fsouza on `src/message.rs`:26 on 2022-11-01 23:03_

Ohh that makes more sense hah I was too influenced by Go in my first attempt. Pushed a fix!

---

_Merged by @charliermarsh on 2022-11-01 23:06_

---

_Closed by @charliermarsh on 2022-11-01 23:06_

---

_@charliermarsh reviewed on 2022-11-01 23:06_

---

_Review comment by @charliermarsh on `src/message.rs`:26 on 2022-11-01 23:06_

Woo, thanks!

---

_Branch deleted on 2022-11-01 23:06_

---

_Comment by @charliermarsh on 2022-11-02 13:45_

\cc @Seamooo - we changed `Message` back to one-based indexing for columns. (`Check` still uses zero-based indexing, so if `ruffd` uses `Check` now as planned, it will be unaffected.)

---

_Comment by @Seamooo on 2022-11-03 02:00_

Awesome, ruffd also now uses location and end_location in `Patch` and this also appears unaffected

---
