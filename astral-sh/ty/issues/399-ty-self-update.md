```yaml
number: 399
title: "`ty self update`"
type: issue
state: open
author: billy-doyle
labels:
  - cli
  - wish
assignees: []
created_at: 2025-05-15T02:02:12Z
updated_at: 2025-11-13T07:31:57Z
url: https://github.com/astral-sh/ty/issues/399
synced_at: 2026-01-10T02:06:24Z
```

# `ty self update`

---

_Issue opened by @billy-doyle on 2025-05-15 02:02_

### Question

Having used uv since 0.1.x, when `uv self update` was added, made it so easy to test all the new and fun changes in each new version released at breakneck pace.

Unsure if `ty self` is a desired api, but if it is like in uv https://github.com/astral-sh/uv/blob/b326bb92a0a2aabc1fb4f12b8d5fa8265a3b3693/crates/uv-cli/src/lib.rs#L572, would enjoy having this added in the early dev.

### Version

ty 0.0.1-alpha.2 (59c45cc60 2025-05-14)

---

_Label `question` added by @billy-doyle on 2025-05-15 02:02_

---

_Comment by @MichaReiser on 2025-05-15 06:20_

Related to https://github.com/astral-sh/ruff/issues/13017

The way this is implemented in uv is by fetching the binary over HTTP. This is something that uv can do easily because it already includes a HTTP stack, unlike ruff and ty

Unlike uv, the primary installation method for ty and ruff is a package manager like uv or pip that both support updating mechanisms.

---

_Label `question` removed by @MichaReiser on 2025-05-15 06:20_

---

_Label `wish` added by @MichaReiser on 2025-05-15 06:20_

---

_Label `cli` added by @carljm on 2025-11-13 07:31_

---
