```yaml
number: 17525
title: "Allow comma-separated values for `--extra` option"
type: pull_request
state: open
author: terror
labels:
  - enhancement
assignees: []
base: main
head: extra-option-shorthand
created_at: 2026-01-16T15:42:40Z
updated_at: 2026-01-19T11:56:01Z
url: https://github.com/astral-sh/uv/pull/17525
synced_at: 2026-01-19T12:32:49Z
```

# Allow comma-separated values for `--extra` option

---

_@terror_

Resolves (partially) https://github.com/astral-sh/uv/issues/17511

This diff enables comma-separated values for `--extra`, allowing `uv sync --extra foo,bar` as an alternative to `uv sync --extra foo --extra bar`.

---

_Comment by @zanieb on 2026-01-16 16:37_

I think we might not want to add the short-flag for now, we tend to want to reserve those and I don't think Poetry parity is critical there. Comma separated values make sense to me though.

---

_Renamed from "Add `-E` short flag and comma-separated values for `--extra` option" to "Allow comma-separated values for `--extra` option" by @terror on 2026-01-16 16:47_

---

_Comment by @terror on 2026-01-16 16:49_

> I think we might not want to add the short-flag for now, we tend to want to reserve those and I don't think Poetry parity is critical there. Comma separated values make sense to me though.

Just re-purposed this PR!

---

_@zanieb approved on 2026-01-16 16:50_

cc @konstin in case I'm missing something here

---

_@konstin approved on 2026-01-19 11:55_

If we have no concerns about consistency with other arguments that are multiple use, then SGTM

---

_Label `enhancement` added by @konstin on 2026-01-19 11:56_

---
