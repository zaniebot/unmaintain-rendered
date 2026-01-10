---
number: 8315
title: "`uv add --index` does not accept special characters in the index name"
type: issue
state: closed
author: vinibrsl
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2024-10-18T00:44:44Z
updated_at: 2024-10-18T17:24:17Z
url: https://github.com/astral-sh/uv/issues/8315
synced_at: 2026-01-10T01:24:27Z
---

# `uv add --index` does not accept special characters in the index name

---

_Issue opened by @vinibrsl on 2024-10-18 00:44_

uv throws an error when adding a custom index with a name that includes characters like hyphens or underscores. While the equal sign (=) is understandably restricted as the separator, other characters should be allowed in the index name.

```
$ uv add --index my_custom_index=https://customindex.test/path/to/index my_package
error: invalid value 'my_custom_index=https://customindex.test/path/to/index' for '--index <INDEX>': relative URL without a base

For more information, try '--help'.
```

If I rename my_custom_index to mycustomindex, the command works as expected, e.g. `uv add --index mycustomindex=https://customindex.test/path/to/index my_package`.

## Version

```
$ uv --version
uv 0.4.24 (b9cd54913 2024-10-17)
```

Related: https://github.com/astral-sh/uv/pull/7746

---

_Renamed from "`uv add --index` does not accept special characters as the index name" to "`uv add --index` does not accept special characters in the index name" by @vinibrsl on 2024-10-18 00:44_

---

_Comment by @zanieb on 2024-10-18 01:34_

Yeah right now they must be alphanumeric https://github.com/astral-sh/uv/blob/2153c6ac0d692b9eccedbd7567087907667645fb/crates/uv-distribution-types/src/index.rs#L161

Underscores and dashes seem pretty reasonable to me?

---

_Label `needs-decision` added by @zanieb on 2024-10-18 01:34_

---

_Assigned to @charliermarsh by @zanieb on 2024-10-18 01:35_

---

_Comment by @charliermarsh on 2024-10-18 02:49_

Yeah that seems fine.

---

_Label `needs-decision` removed by @zanieb on 2024-10-18 04:01_

---

_Label `bug` added by @charliermarsh on 2024-10-18 12:58_

---

_Label `help wanted` added by @charliermarsh on 2024-10-18 12:59_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-10-18 13:55_

---

_Label `good first issue` added by @charliermarsh on 2024-10-18 14:49_

---

_Referenced in [astral-sh/uv#8339](../../astral-sh/uv/pulls/8339.md) on 2024-10-18 16:49_

---

_Assigned to @vinibrsl by @charliermarsh on 2024-10-18 17:01_

---

_Closed by @charliermarsh on 2024-10-18 17:24_

---

_Closed by @charliermarsh on 2024-10-18 17:24_

---
