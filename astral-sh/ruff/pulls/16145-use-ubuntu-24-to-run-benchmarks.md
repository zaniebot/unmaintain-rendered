```yaml
number: 16145
title: Use ubuntu-24 to run benchmarks
type: pull_request
state: merged
author: Glyphack
labels:
  - ci
assignees: []
merged: true
base: main
head: fix-ci
created_at: 2025-02-13T20:32:29Z
updated_at: 2025-05-21T20:26:36Z
url: https://github.com/astral-sh/ruff/pull/16145
synced_at: 2026-01-12T15:55:53Z
```

# Use ubuntu-24 to run benchmarks

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The CI benchmark step is broken with:
```
/home/runner/.cargo/bin/cargo-codspeed: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by /home/runner/.cargo/bin/cargo-codspeed)
```
[main CI](https://github.com/astral-sh/ruff/actions/runs/13314818925/job/37186139311)


It was also failing in my PR. I updated the ubuntu version in my PR and it succeeded:
https://github.com/astral-sh/ruff/pull/16144/commits/d335cef9ab334032f391120b280a832bd8447085

Since it fixed the issue I am changing this to benchmark to use `ubuntu-latest` (since their [actions repo](https://github.com/CodSpeedHQ/action) is using this version in the examples) I am sure between 22.04 and latest.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-02-13 20:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/c6a192bb461d0bc317b61659254ef76582b4ce9c/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `ci` added by @ntBre on 2025-02-13 20:46_

---

_Comment by @ntBre on 2025-02-13 20:55_

I guess codspeed must support ubuntu 24 now. We pinned to 22 in #13743.

Thanks for catching this! I guess my only question is if we should just bump to `ubuntu-24.04` or go back to `latest`. 

cc @MichaReiser

---

_Comment by @Glyphack on 2025-02-13 21:05_

I’m not sure either. Both versions seem to be working in this PR I used latest and in my original PR that is linked I used 24.04. I decided to use latest since I saw that's the version used in https://docs.codspeed.io/integrations/ci/github-actions.

---

_@MichaReiser reviewed on 2025-02-13 21:53_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:715 on 2025-02-13 21:53_

We had some issues in the past where GitHub upgrade and downgraded us between versions which broke CI (because of caching). Could you fix the runner to whatever latest resolves today (24?)

---

_Review comment by @Glyphack on `.github/workflows/ci.yaml`:715 on 2025-02-13 21:54_

Yes I am switching to 24.04 which worked for me.

---

_@Glyphack reviewed on 2025-02-13 21:54_

---

_Renamed from "Use ubuntu-latest to run benchmarks" to "Use ubuntu-24 to run benchmarks" by @MichaReiser on 2025-02-13 22:05_

---

_Merged by @MichaReiser on 2025-02-13 22:05_

---

_Closed by @MichaReiser on 2025-02-13 22:05_

---

_Branch deleted on 2025-05-21 20:26_

---
