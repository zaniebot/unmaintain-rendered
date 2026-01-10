---
number: 990
title: Re-enable Windows builds in release pipeline
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-19T01:58:27Z
updated_at: 2024-01-27T01:12:27Z
url: https://github.com/astral-sh/uv/issues/990
synced_at: 2026-01-10T01:23:05Z
---

# Re-enable Windows builds in release pipeline

---

_Issue opened by @charliermarsh on 2024-01-19 01:58_

The ARM build will require using native-tls, like in Maturin:

```txt
# ring doesn't support aarch64 windows yet
- name: Build wheel (windows aarch64)
  if: matrix.target == 'aarch64-pc-windows-msvc'
  run: cargo run -- build --release -b bin -o dist --target ${{ matrix.target }} --no-default-features --features full,native-tls
```


---

_Label `bug` added by @charliermarsh on 2024-01-19 01:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 17:16_

---

_Referenced in [astral-sh/uv#1129](../../astral-sh/uv/pulls/1129.md) on 2024-01-26 17:31_

---

_Closed by @charliermarsh on 2024-01-27 01:12_

---
