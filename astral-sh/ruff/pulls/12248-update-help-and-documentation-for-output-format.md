```yaml
number: 12248
title: "Update help and documentation for `--output-format` to reflect `\"full\"` default"
type: pull_request
state: merged
author: DaniBodor
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix_output_format_help
created_at: 2024-07-08T17:08:52Z
updated_at: 2024-07-09T07:20:27Z
url: https://github.com/astral-sh/ruff/pull/12248
synced_at: 2026-01-10T21:47:02Z
```

# Update help and documentation for `--output-format` to reflect `"full"` default

---

_Pull request opened by @DaniBodor on 2024-07-08 17:08_

fix #12247 

changed help to list "full" as the default for --output-format and removed "text" as an option (as this is no longer supported).

EDIT: I am trying to see why some of the checks below are failing, but don't know how to interpret the errors.

---

_Comment by @zanieb on 2024-07-08 19:30_

Thanks for contributing! You need to run `cargo dev generate-json-schema` to update the JSON schema for the docs changes

---

_@charliermarsh approved on 2024-07-09 02:38_

Thanks!

---

_Renamed from "update help for output-format" to "Update help and documentation for `--output-format` to reflect `"full"` default" by @charliermarsh on 2024-07-09 02:38_

---

_Label `documentation` added by @charliermarsh on 2024-07-09 02:38_

---

_Merged by @charliermarsh on 2024-07-09 02:45_

---

_Closed by @charliermarsh on 2024-07-09 02:45_

---

_Branch deleted on 2024-07-09 07:20_

---
