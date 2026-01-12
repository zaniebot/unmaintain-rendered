```yaml
number: 1424
title: Add Support for GitLab CI Code Quality Report Format
type: pull_request
state: merged
author: saadmk11
labels: []
assignees: []
merged: true
base: main
head: gitlab-error-output
created_at: 2022-12-28T14:20:47Z
updated_at: 2022-12-28T16:13:39Z
url: https://github.com/astral-sh/ruff/pull/1424
synced_at: 2026-01-12T15:55:06Z
```

# Add Support for GitLab CI Code Quality Report Format

---

_@saadmk11_

closes https://github.com/charliermarsh/ruff/issues/1298

---

_Comment by @charliermarsh on 2022-12-28 14:22_

You rock, thank you for this. Are you able to test the output format against Gitlab in practice? (I don't use Gitlab so a little difficult for me to test.)

---

_@saadmk11 reviewed on 2022-12-28 14:26_

---

_Review comment by @saadmk11 on `src/printer.rs`:264 on 2022-12-28 14:26_

Not sure if we should add `struct`'s to structure the JSON. It may be better to use struct's even if we will use it for just this one use-case. (I have recently started learning Rust)

---

_Comment by @saadmk11 on 2022-12-28 14:28_

@charliermarsh 

### Workflow:
```yaml
include:
  - template: Code-Quality.gitlab-ci.yml


code_quality:
  image: python:3.10-slim
  allow_failure: false
  script:
    - pip install ruff
    - ruff . --format gitlab > gl-code-quality-report.json
  artifacts:
    reports:
      codequality: gl-code-quality-report.json
```
### Output:
![Screenshot from 2022-12-28 19-41-01](https://user-images.githubusercontent.com/24854406/209826948-ce744c20-0ae2-43f2-82bb-4fc567138864.png)


---

_@saadmk11 reviewed on 2022-12-28 14:32_

---

_Review comment by @saadmk11 on `src/settings/types.rs`:168 on 2022-12-28 14:32_

`default()` may not run anymore because it not used anywhere. 
`format: options.format.unwrap_or_default()` was removed from `src/settings/configuration.rs` on https://github.com/charliermarsh/ruff/pull/1219/ 

---

_@charliermarsh reviewed on 2022-12-28 14:39_

---

_Review comment by @charliermarsh on `src/settings/types.rs`:168 on 2022-12-28 14:39_

Ahh yeah, that actually seems like a bug to me. The unwrap step got moved to `src/settings/mod.rs` (`format: config.format.unwrap_or(SerializationFormat::Text)`), but should likely still use `default`. If you want to change that here, that'd be welcome; or as a separate PR, or I can handle it later.

---

_@charliermarsh reviewed on 2022-12-28 14:39_

---

_Review comment by @charliermarsh on `src/printer.rs`:264 on 2022-12-28 14:39_

I think this is fine for now, since it's treated equivalently to the GitHub approach and colocated with that formatting anyway.

---

_Review comment by @saadmk11 on `src/settings/types.rs`:168 on 2022-12-28 14:40_

Sure, I can add it here as they are closely related. :)

---

_@saadmk11 reviewed on 2022-12-28 14:40_

---

_Merged by @charliermarsh on 2022-12-28 15:10_

---

_Closed by @charliermarsh on 2022-12-28 15:10_

---

_Comment by @charliermarsh on 2022-12-28 15:10_

Awesome, thank you!

---

_Branch deleted on 2022-12-28 16:13_

---
