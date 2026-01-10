```yaml
number: 1176
title: "Deduplicate `pep440_rs`"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-01-29T21:01:36Z
updated_at: 2024-01-29T21:11:43Z
url: https://github.com/astral-sh/uv/issues/1176
synced_at: 2026-01-10T05:40:31Z
```

# Deduplicate `pep440_rs`

---

_Issue opened by @charliermarsh on 2024-01-29 21:01_

Right now, `pyproject-toml` is pulling in the version from crates.io, while the rest of the project gets the local version:

```text
❯ cargo tree -p puffin -i pep440_rs
error: There are multiple `pep440_rs` packages in your project, and the specification `pep440_rs` is ambiguous.
Please re-run this command with one of the following specifications:
  file:///Users/crmarsh/workspace/guffin/crates/pep440-rs#pep440_rs@0.3.12
  https://github.com/rust-lang/crates.io-index#pep440_rs@0.3.12
```


---

_Label `internal` added by @charliermarsh on 2024-01-29 21:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-29 21:04_

---

_Closed by @charliermarsh on 2024-01-29 21:11_

---
