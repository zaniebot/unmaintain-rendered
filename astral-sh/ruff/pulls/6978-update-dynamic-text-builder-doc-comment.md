```yaml
number: 6978
title: "Update `dynamic_text` builder doc comment"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: chore
created_at: 2023-08-29T13:27:53Z
updated_at: 2023-08-29T14:51:34Z
url: https://github.com/astral-sh/ruff/pull/6978
synced_at: 2026-01-12T02:45:38Z
```

# Update `dynamic_text` builder doc comment

---

_Pull request opened by @cnpryer on 2023-08-29 13:27_

This PR adds a little more detail to `dynamic_text`'s documentation.

This isn't *really* necessary since I'm sure 'dynamic' implies this logic in some way that I'm not familiar with yet, but I'm always looking for even subtle improvements to help the next reader.

Since I didn't read the implementation I failed to realize that this builder allocates a new string -- which would have influenced my decision using it for some use cases. See https://github.com/astral-sh/ruff/pull/6901#discussion_r1308276680 for more context.

Idk if this is just me, but I tend to start out by running `cargo doc -p ruff_formatter --open` to gain some familiarity with the project (I should start using the `source` button in addition to reading the documentation lol :)).

---

_@MichaReiser approved on 2023-08-29 14:29_

---

_Label `formatter` added by @MichaReiser on 2023-08-29 14:29_

---

_Merged by @MichaReiser on 2023-08-29 14:29_

---

_Closed by @MichaReiser on 2023-08-29 14:29_

---

_Branch deleted on 2023-08-29 14:51_

---
