```yaml
number: 16653
title: "Add install summary to `uv sync` JSON output"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-11-09T15:05:21Z
updated_at: 2025-12-18T19:38:34Z
url: https://github.com/astral-sh/uv/issues/16653
synced_at: 2026-01-12T16:02:35Z
```

# Add install summary to `uv sync` JSON output

---

_@zanieb_

### Summary

`uv sync --output-format json` does not include the install plan, i.e., which packages will be added or removed. We should add that.

There's a lot of information we could capture here, e.g., the package name, version, source, etc. The feature is in preview so it's fine to start with a minimal implementation and iterate as we need.

### Example

Currently:
```json
{
  "schema": {
    "version": "preview"
  },
  "target": "project",
  "project": {
    "path": "/Users/zb/workspace/uv/example",
    "workspace": {
      "path": "/Users/zb/workspace/uv/example"
    }
  },
  "sync": {
    "environment": {
      "path": "/Users/zb/workspace/uv/example/.venv",
      "python": {
        "path": "/Users/zb/workspace/uv/example/.venv/bin/python",
        "version": "3.14.0",
        "implementation": "cpython"
      }
    },
    "action": "update"
  },
  "lock": {
    "path": "/Users/zb/workspace/uv/example/uv.lock",
    "action": "check"
  },
  "dry_run": false
}
```

I think I'd expect another section under "sync", e.g., "packages", with a list of changes?

---

_Label `enhancement` added by @zanieb on 2025-11-09 15:05_

---

_Assigned to @EliteTK by @EliteTK on 2025-12-03 14:48_

---

_Comment by @EliteTK on 2025-12-03 17:35_

I noticed that --output-format json puts things on stdout, but the normal `text` output is always present (always on stderr).

Not quite related ...

---

_Comment by @zanieb on 2025-12-03 18:19_

It definitely should go to stdout. I'm not sure about the retention of the normal text output on stdr. I recently noticed that and was thinking we should revisit it. Long-term, we'll want an `--output-format json-lines` or `--output-format ndjson` where we emit JSON messages as we go (i.e., for progress updates and logs).

---

_Comment by @EliteTK on 2025-12-18 19:38_

Implemented in #16981.

Refer to #17151 for future design decisions.

---

_Closed by @EliteTK on 2025-12-18 19:38_

---
