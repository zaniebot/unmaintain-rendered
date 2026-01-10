```yaml
number: 17151
title: "Consider which dist related metadata should be included in `--output-format=json` when a dist is included in the output"
type: issue
state: open
author: EliteTK
labels:
  - enhancement
  - needs-design
  - preview
assignees: []
created_at: 2025-12-16T15:55:44Z
updated_at: 2026-01-06T16:40:28Z
url: https://github.com/astral-sh/uv/issues/17151
synced_at: 2026-01-10T03:11:35Z
```

# Consider which dist related metadata should be included in `--output-format=json` when a dist is included in the output

---

_Issue opened by @EliteTK on 2025-12-16 15:55_

### Summary

With https://github.com/astral-sh/uv/pull/16981, information about changes to installed packages is being added to the `--output-format=json` output of `uv sync` and inevitably this information will likely be added to other similar outputs.

Right now the idea is to include the package name, version (and URL if present) in the output, to match the output on stderr for the relevant commands.

But it may be worth including more information.

The current design of the `Changelog` used to generate the JSON report is such that it gives access directly to `Arc<Dist>` and `LocalDist` which means that there could potentially be quite a lot of information such as the type of dist, the location on disk, etc.

### Example

N/A?

---

_Label `enhancement` added by @EliteTK on 2025-12-16 15:55_

---

_Label `needs-design` added by @EliteTK on 2025-12-16 15:55_

---

_Label `preview` added by @EliteTK on 2025-12-16 15:55_

---

_Comment by @EliteTK on 2025-12-18 13:58_

Also worth considering the format too:

e.g. current:

```json
"changes": [
  {
    "name": "anyio",
    "version": "4.3.0",
    ...,
    "action": "installed"
  }
]
```

Alternative 1:

```json
"changes": [
  {
    "dist": {
      "name": "anyio",
      "version": "4.3.0",
      ...
    },
    "action": "installed"
  }
]
```

Alternative 2:

```json
"changes": [
  {
    "name": "anyio",
    "dist": {
      "version": "4.3.0",
      ...
    },
    "action": "installed"
  }
]

---

_Comment by @EliteTK on 2026-01-06 16:40_

#17323 seems like it might be relevant, or at least it should also affect this format.

---
