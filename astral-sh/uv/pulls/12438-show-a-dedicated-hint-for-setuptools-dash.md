```yaml
number: 12438
title: "Show a dedicated hint for `setuptools` dash-separator change"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-03-24T16:09:55Z
updated_at: 2025-03-24T16:37:40Z
url: https://github.com/astral-sh/uv/pull/12438
synced_at: 2026-01-10T11:10:40Z
```

# Show a dedicated hint for `setuptools` dash-separator change

---

_Pull request opened by @charliermarsh on 2025-03-24 16:09_

## Summary

See: https://github.com/astral-sh/uv/issues/12434.

## Test Plan

The error in-context:

![Screenshot 2025-03-24 at 12 10 11 PM](https://github.com/user-attachments/assets/48885d71-4222-4364-987f-65a4b776d91f)



---

_Review requested from @zanieb by @charliermarsh on 2025-03-24 16:10_

---

_Review requested from @konstin by @charliermarsh on 2025-03-24 16:10_

---

_Label `error messages` added by @charliermarsh on 2025-03-24 16:10_

---

_Marked ready for review by @charliermarsh on 2025-03-24 16:10_

---

_@zanieb reviewed on 2025-03-24 16:11_

---

_Review comment by @zanieb on `crates/uv-build-frontend/src/error.rs`:144 on 2025-03-24 16:11_

```suggestion
                    "setuptools removed support for dash-separated and uppercased keys in v78.0.0. Consider updating the relevant `setup.cfg`, or use `tool.uv.build-constraint-dependencies` to pin `setuptools<78`:\n```toml\n[tool.uv]\nbuild-constraint-dependencies = [\"setuptools<78\"]\n```"
```

Deprecation implies warning not erroring to me.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:10327 on 2025-03-24 16:11_

```suggestion
          hint: setuptools removed support for dash-separated and uppercased keys in v78.0.0. Consider updating the relevant `setup.cfg`, or use `tool.uv.build-constraint-dependencies` to pin `setuptools<78`:
```

---

_@zanieb reviewed on 2025-03-24 16:11_

---

_@zanieb approved on 2025-03-24 16:12_

---

_Comment by @zanieb on 2025-03-24 16:13_

Clippy:

```
error: unnecessary hashes around raw string literal
     --> crates/uv/tests/it/pip_install.rs:10279:33
      |
10279 |       setup_cfg.write_str(indoc! {r#"
      |  _________________________________^
10280 | |         [metadata]
10281 | |         name = foo
10282 | |         version = 0.1.0
10283 | |         requires-python = >=3.12
10284 | |         install_requires = iniconfig
10285 | |     "#})?;
      | |______^
```

---

_@konstin approved on 2025-03-24 16:37_

---

_Merged by @charliermarsh on 2025-03-24 16:37_

---

_Closed by @charliermarsh on 2025-03-24 16:37_

---

_Branch deleted on 2025-03-24 16:37_

---
