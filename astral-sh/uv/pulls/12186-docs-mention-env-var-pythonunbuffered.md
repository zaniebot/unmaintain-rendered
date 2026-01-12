```yaml
number: 12186
title: "docs: Mention env var PYTHONUNBUFFERED"
type: pull_request
state: open
author: quassy
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2025-03-15T13:37:26Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/12186
synced_at: 2026-01-12T16:10:09Z
```

# docs: Mention env var PYTHONUNBUFFERED

---

_@quassy_

## Summary

I did not find any info about whether uv (run in particular) supports `PYTHONUNBUFFERED` but it seems to be implemented in [env_vars.rs](https://github.com/astral-sh/uv/blob/2f28d60ba3a959101a109d5912f565da08a117ba/crates/uv-static/src/env_vars.rs#L521).

(Sadly still no CLI parameter `-u` exists.)

## Test Plan

No testing as is purely a change in the markdown documentation.

---

_Comment by @charliermarsh on 2025-03-15 17:40_

Thanks for raising this! I think we tend to avoid documenting environment variables that affect `python`, but that we don't read or write directly (which includes `PYTHONUNBUFFERED`). If we add `PYTHONUNBUFFERED`, then we should probably add every environment variable that impacts the `python` runtime, which seems sort of untenable.

(We do have `PYTHONUNBUFFERED` in `env_vars.rs`, but it's only used when we compile bytecode, where we _set_ it to "1" for a `python` process that we run within uv. That's why it's not exposed.) 

---

_Label `documentation` added by @charliermarsh on 2025-03-15 17:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-15 17:41_

---

_Comment by @quassy on 2025-03-23 15:04_

> If we add PYTHONUNBUFFERED, then we should probably add every environment variable that impacts the python runtime, which seems sort of untenable.

Makes sense! Are all Python env variables also supported when using `uv run (-m)`?

Is `uv run python -m ...` generally the same as `uv run -m ...`? I guess to use the other CLI arguments of Python one then just needs to use long form command with Python like this `uv run python -i`?

---

_Comment by @zanieb on 2025-04-01 17:36_

> Is uv run python -m ... generally the same as uv run -m ...?

Yes, we just invoke `python` in both cases.

---
