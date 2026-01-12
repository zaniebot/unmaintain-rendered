```yaml
number: 9814
title: "Remove deprecated configuration '--show-source`"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - breaking
  - configuration
  - cli
assignees: []
merged: true
base: ruff-0.5
head: remove-deprecated-configuration
created_at: 2024-02-04T19:00:13Z
updated_at: 2024-06-25T18:14:09Z
url: https://github.com/astral-sh/ruff/pull/9814
synced_at: 2026-01-12T15:55:30Z
```

# Remove deprecated configuration '--show-source`

---

_@tibor-reiss_

Fixes parts of https://github.com/astral-sh/ruff/issues/7650:

<details>
<summary>Extracted from this PR</summary>
1. removes the deprecated configuration 'format'. It was deprecated in https://github.com/astral-sh/ruff/pull/8203 in favor of 'output-format'.
Update: Was merged in https://github.com/astral-sh/ruff/pull/10170
Tests:
- `cargo run -p ruff -- --explain RET505` works
- `cargo run -p ruff -- --explain RET505 --output-format json` works
- `cargo run -p ruff -- --explain RET505 --format json` does not work anymore
</details>

2. removes deprecated configurations 'show-source' and 'no-show-source'.
Tests:
- `cargo run -p ruff -- --show-source` does not work anymore
- `cargo run -p ruff -- --no-show-source` does not work anymore 

resolves: #7349 
Part of https://github.com/astral-sh/ruff/issues/7650

---

_Comment by @github-actions[bot] on 2024-02-04 19:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-02-04 21:00_

Thanks for your contribution! We won't be able to release this until v0.3.0 so it's going to be a bit before we can merge

---

_Added to milestone `v0.3.0` by @zanieb on 2024-02-04 21:00_

---

_Label `breaking` added by @zanieb on 2024-02-05 01:59_

---

_Comment by @tibor-reiss on 2024-02-05 07:09_

Hi @zanieb, since there are more of these deprecations in the codebase, do you mind if I keep adding commits to this PR, or shall I open a separate one for each of them?

---

_Comment by @zanieb on 2024-02-08 19:38_

Hey @tibor-reiss — I'm unsure. Ideally these things would be in separate pull requests for our changelog and review but we're talking pretty far out here and it could be a significant amount to keep your changes up to date with `main`. In general, I'd recommend pursuing issues that aren't breaking changes since they aren't timing sensitive.

Another thing we can do is have deprecated options _error_ in **preview** mode first but retain a _warning_ in stable.

---

_Comment by @tibor-reiss on 2024-02-08 20:03_

Hi @zanieb, I understand. I added the 2nd commit just before your answer - talk about timing. And as you predicted, merge conflict.
Since the timeline is unclear, and this is anyways a low-prio change, I would recommend gathering the commits here and not polluting the board with more PRs. Once we get closer, it's a minimal change to pull out the commits into separate PRs so you can ping me about your preferences about this change. What do you think?

---

_Removed from milestone `v0.3.0` by @MichaReiser on 2024-02-29 13:52_

---

_Comment by @MichaReiser on 2024-02-29 13:58_

@tibor-reiss I extracted the first breaking change into its own PR and ship it as part of 0.3. We want to wait a little longer with the other options because it isn't that long ago that we've deprecated them.

---

_Renamed from "Remove deprecated configuration 'format'" to "Remove deprecated configuration '--show-source`" by @MichaReiser on 2024-03-04 13:14_

---

_Added to milestone `v0.4.0` by @MichaReiser on 2024-03-04 13:17_

---

_Removed from milestone `v0.4.0` by @dhruvmanila on 2024-04-18 19:49_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-04-18 19:49_

---

_@MichaReiser approved on 2024-06-24 08:51_

---

_Label `configuration` added by @MichaReiser on 2024-06-24 08:51_

---

_Label `cli` added by @MichaReiser on 2024-06-24 08:51_

---

_Merged by @MichaReiser on 2024-06-24 08:52_

---

_Closed by @MichaReiser on 2024-06-24 08:52_

---

_Branch deleted on 2024-06-25 18:14_

---
