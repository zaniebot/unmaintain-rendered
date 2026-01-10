```yaml
number: 19504
title: "Derive `Clone` and `Debug` for tokenizer structs"
type: pull_request
state: closed
author: ShaharNaveh
labels: []
assignees: []
base: main
head: tokenizer-derive-clone-debug
created_at: 2025-07-23T09:14:37Z
updated_at: 2025-10-03T21:15:53Z
url: https://github.com/astral-sh/ruff/pull/19504
synced_at: 2026-01-10T17:34:34Z
```

# Derive `Clone` and `Debug` for tokenizer structs

---

_Pull request opened by @ShaharNaveh on 2025-07-23 09:14_

## Summary
When tried to use `SimpleTokenizer` as an attribute inside a struct that derives both `Clone` and `Debug` I got an error, so I thought I'll fix it upstream.

---

Feel free to close this PR if this an unwanted change

---

_Comment by @MichaReiser on 2025-07-23 09:23_

Hi. I'm not sure it's worth discussing much, but the `Debug` implementations are mainly missing because the output would be extremely verbose. 

We also never really had a use case for cloning a tokenizer. Can you tell us more about your use case?

---

_Comment by @ShaharNaveh on 2025-07-23 11:41_

I tried to solve something internal by patching it upstream, sorry for your waste of time

---

_Closed by @ShaharNaveh on 2025-07-23 11:41_

---

_Branch deleted on 2025-10-03 21:15_

---
