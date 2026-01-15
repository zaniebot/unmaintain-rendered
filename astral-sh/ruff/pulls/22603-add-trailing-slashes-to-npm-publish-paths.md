```yaml
number: 22603
title: "Add trailing slashes to `npm publish` paths"
type: pull_request
state: merged
author: ntBre
labels:
  - release
  - ci
assignees: []
merged: true
base: main
head: brent/wasm-build
created_at: 2026-01-15T16:56:33Z
updated_at: 2026-01-15T17:18:25Z
url: https://github.com/astral-sh/ruff/pull/22603
synced_at: 2026-01-15T17:50:24Z
```

# Add trailing slashes to `npm publish` paths

---

_@ntBre_

Summary
--

The WASM builds in the 0.14.12 release were failing with an error about trying
to publish over an existing version 5.8.1 ([example]). This seems to be because
it's trying to publish a package named `pkg` instead of the `pkg` directory.

Micha found that a trailing slash causes `npm` to interpret the path correctly.

Test Plan
--

I think we can test this with a dry run of the release workflow, otherwise
another release attempt for 0.14.12

[example]: https://github.com/astral-sh/ruff/actions/runs/21037103249/job/60492735499#step:5:281


---

_Label `release` added by @ntBre on 2026-01-15 16:56_

---

_Label `ci` added by @ntBre on 2026-01-15 16:56_

---

_Comment by @ntBre on 2026-01-15 17:00_

Well I kicked off a dry run, but I guess the publish step doesn't run, as seen in https://github.com/astral-sh/ruff/pull/22476.

---

_@amyreese approved on 2026-01-15 17:08_

---

_@MichaReiser approved on 2026-01-15 17:17_

---

_Merged by @ntBre on 2026-01-15 17:18_

---

_Closed by @ntBre on 2026-01-15 17:18_

---

_Branch deleted on 2026-01-15 17:18_

---
