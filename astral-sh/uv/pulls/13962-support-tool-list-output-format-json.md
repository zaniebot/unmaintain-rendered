---
number: 13962
title: "Support `tool list --output-format=json`"
type: pull_request
state: open
author: InSyncWithFoo
labels: []
assignees:
  - zanieb
base: main
head: tool-list-json
created_at: 2025-06-11T11:28:52Z
updated_at: 2025-12-06T13:50:58Z
url: https://github.com/astral-sh/uv/pull/13962
synced_at: 2026-01-10T01:26:17Z
---

# Support `tool list --output-format=json`

---

_Pull request opened by @InSyncWithFoo on 2025-06-11 11:28_

## Summary

Resolves #13633.

`tool list --output-format=json` outputs information about the installed tools in JSON. An example output looks like this:

```json
[
  {
    "name": "black",
    "version": "24.2.0",
    "version_specifiers": ["==24.2.0"],
    "extra_requirements": [],
    "with_requirements": [],
    "directory": "[...]/tools/black",
    "environment": {
      "python": "[...]/tools/black/bin/python3",
      "version": "3.12.1"
    },
    "entrypoints": [
      { "name": "black", "path": "[...]/bin/black" },
      { "name": "blackd", "path": "[...]/bin/blackd" }
    ]
  }
]
```

`--show-paths`, `--show-version-specifiers`, `--show-with` and `--show-extras` are allowed to be used with `--output-format`, but they are redundant; the output will always include those information.

## Test Plan

Unit tests.


---

_Assigned to @Gankra by @konstin on 2025-06-11 12:14_

---

_Review requested from @Gankra by @konstin on 2025-06-11 12:14_

---

_Marked ready for review by @InSyncWithFoo on 2025-06-11 15:24_

---

_Comment by @codspeed-hq[bot] on 2025-06-12 14:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/InSyncWithFoo%3Atool-list-json?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #13962 will **not alter performance**

<sub>Comparing <code>InSyncWithFoo:tool-list-json</code> (08953ab) with <code>main</code> (77c771c)</sub>



### Summary

`✅ 3` untouched  





---

_Unassigned @Gankra by @konstin on 2025-07-07 10:01_

---

_Review request for @Gankra removed by @konstin on 2025-07-07 10:01_

---

_Referenced in [astral-sh/uv#13695](../../astral-sh/uv/issues/13695.md) on 2025-07-09 10:59_

---

_@zanieb reviewed on 2025-07-14 16:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4448 on 2025-07-14 16:58_

I don't think we need to error if these are provided, they're just redundant.

---

_Comment by @zanieb on 2025-07-14 19:05_

Thanks for your patience here. 

I'm planning to do a pass on this to make it consistent with #13689 unless you're interested? In particular, I think we should:

- Attempt to use the same implementation and derive the display from the report rather than splitting the implementation
- Add a preview warning
- Reuse some of the report structs from there

---

_Comment by @InSyncWithFoo on 2025-07-14 19:22_

@zanieb I'm occupied at the moment, so please, go ahead and thanks.

---

_Assigned to @zanieb by @zanieb on 2025-07-14 20:22_

---

_Comment by @sigma67 on 2025-10-17 07:59_

Is there still interest in this PR? Would be a great feature to have ❤️

---
