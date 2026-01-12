```yaml
number: 6848
title: "fix: correct inline suggestion for requirements.txt"
type: pull_request
state: closed
author: dimaqq
labels: []
assignees: []
base: main
head: fix-requirements-inline-help
created_at: 2024-08-30T01:17:25Z
updated_at: 2024-10-10T22:09:49Z
url: https://github.com/astral-sh/uv/pull/6848
synced_at: 2026-01-12T16:07:33Z
```

# fix: correct inline suggestion for requirements.txt

---

_@dimaqq_

Fixed #6845

## Summary

Passing a requiremenst.txt file via `--with` to uvx is wrong and there's a helpful inline suggestion to use xxxx instead.

The suggestion was wrong, suggested `-r` while the correct flag is `--with-requirements`.

## Test Plan

- create a simple requirements.txt
- [opt] run `cargo run -- --help` to work around #6847
- run `cargo run --bin uvx -- --with test-requirements.txt tox` or similar

## Full Disclosure

The "wrong" text was present in 4 lines of code and 1 assert, and so I've changes all 5 places.

I have no clue if all 5 places should have been changed. Maybe only some?

Please review 🙇🏻 

---

_Comment by @charliermarsh on 2024-08-30 02:19_

Thanks! I think we need to somehow make this dynamic based on the input command, since -r is correct for some commands (like uv pip install).

---

_Comment by @dimaqq on 2024-09-02 03:26_

I'll happily leave this to maintainers, as my crab claws have barely moulted...

---

_Comment by @charliermarsh on 2024-09-02 18:24_

Hah, no worries @dimaqq. I'll try to finish it.

---

_Comment by @charliermarsh on 2024-10-10 22:09_

Fixed in https://github.com/astral-sh/uv/pull/8112.

---

_Closed by @charliermarsh on 2024-10-10 22:09_

---
