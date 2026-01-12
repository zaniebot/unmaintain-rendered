```yaml
number: 21337
title: "[ruff+ty] Add colour to `--help`"
type: pull_request
state: merged
author: hugovk
labels:
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-08T10:43:39Z
updated_at: 2025-11-08T16:17:14Z
url: https://github.com/astral-sh/ruff/pull/21337
synced_at: 2026-01-12T15:57:21Z
```

# [ruff+ty] Add colour to `--help`

---

_@hugovk_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The output of `uv --help` uses green and cyan to improve readability. But ruff and ty do not:

<img width="1470" height="919" alt="image" src="https://github.com/user-attachments/assets/4b25cd33-5da7-432f-930a-43fe5ad16fce" />

Let's add similar styling to ruff and ty. This is the main bit in uv:

https://github.com/astral-sh/uv/blob/85c5d32284522eb958d9cfc10ff62f0956eab113/crates/uv-cli/src/lib.rs#L88-L93

## Test Plan

<!-- How was it tested? -->

Compare and check they have similarly styled output:

```sh
./target/debug/ruff --help
./target/debug/ty --help
uv --help
```

<img width="1470" height="922" alt="image" src="https://github.com/user-attachments/assets/1391bc1e-bab3-4631-853d-0bbc05b85829" />


Also run with `NO_COLOR=1` to confirm there's no colour or bold text:

```sh
NO_COLOR=1 ./target/debug/ruff --help
NO_COLOR=1 ./target/debug/ty --help
NO_COLOR=1 uv --help
```

<img width="1470" height="921" alt="image" src="https://github.com/user-attachments/assets/60259e17-b66b-46ae-8284-267c79e57b01" />




---

_Review requested from @carljm by @hugovk on 2025-11-08 10:43_

---

_Review requested from @MichaReiser by @hugovk on 2025-11-08 10:43_

---

_Review requested from @sharkdp by @hugovk on 2025-11-08 10:43_

---

_Review requested from @dcreager by @hugovk on 2025-11-08 10:43_

---

_Label `cli` added by @MichaReiser on 2025-11-08 16:16_

---

_Comment by @MichaReiser on 2025-11-08 16:17_

Thank you, this is great

---

_Merged by @MichaReiser on 2025-11-08 16:17_

---

_Closed by @MichaReiser on 2025-11-08 16:17_

---
