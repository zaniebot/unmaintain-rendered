```yaml
number: 1298
title: Support GitLab-compatible Code Climate output format
type: issue
state: closed
author: teucer
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2022-12-20T13:29:29Z
updated_at: 2022-12-28T15:10:44Z
url: https://github.com/astral-sh/ruff/issues/1298
synced_at: 2026-01-10T12:05:25Z
```

# Support GitLab-compatible Code Climate output format

---

_Issue opened by @teucer on 2022-12-20 13:29_

Is it possible to generate code climate json format? I would like to use it for GitLab code quality jobs.


---

_Label `enhancement` added by @charliermarsh on 2022-12-20 17:58_

---

_Label `configuration` added by @charliermarsh on 2022-12-20 17:58_

---

_Comment by @charliermarsh on 2022-12-20 17:58_

We can definitely support this (we already support `--format=github`, for GitHub Actions). Can you link me to any documentation on the format spec, to make sure I'm looking at the right thing?

---

_Comment by @teucer on 2022-12-20 19:23_

[Here](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implementing-a-custom-tool) and [here](https://github.com/codeclimate/platform/blob/master/spec/analyzers/SPEC.md#data-types) 

---

_Renamed from "Code climate" to "Support GitLab-compatible Code Climate output format" by @charliermarsh on 2022-12-20 20:41_

---

_Label `good first issue` added by @charliermarsh on 2022-12-20 20:41_

---

_Comment by @charliermarsh on 2022-12-20 20:41_

This just requires adding a new `SerializationFormat`, and implementing the format spec in `printer.rs#write_once`.

---

_Closed by @charliermarsh on 2022-12-28 15:10_

---
