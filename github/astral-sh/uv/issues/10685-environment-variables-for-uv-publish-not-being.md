---
number: 10685
title: Environment Variables for uv publish not being recognized
type: issue
state: closed
author: dvonessen
labels:
  - needs-mre
assignees: []
created_at: 2025-01-16T16:31:26Z
updated_at: 2025-02-18T20:12:44Z
url: https://github.com/astral-sh/uv/issues/10685
synced_at: 2026-01-07T13:12:18-06:00
---

# Environment Variables for uv publish not being recognized

---

_Issue opened by @dvonessen on 2025-01-16 16:31_

When publishing a package to a private repository, there are two environment variables that can be set: `UV_PUBLISH_PASSWORD` and `UV_PUBLISH_USERNAME` and `UV_PUBLISH_URL`.

While using `uv publish` with these environment variables, we get an "upload denied for user anonymous" error.

As soon as we use the command line flags `--password` and `--username`, the upload works as expected.

Am I missing something?

Thank you!
Daniel

---

_Assigned to @konstin by @zanieb on 2025-01-16 17:15_

---

_Comment by @charliermarsh on 2025-01-16 18:00_

Can you confirm that you're on the last version? These environment variables should be exactly equivalent to the command line arguments:

```
/// The username for the upload.
#[arg(short, long, env = EnvVars::UV_PUBLISH_USERNAME)]
pub username: Option<String>,

/// The password for the upload.
#[arg(short, long, env = EnvVars::UV_PUBLISH_PASSWORD)]
pub password: Option<String>,
```

---

_Unassigned @konstin by @konstin on 2025-02-18 20:10_

---

_Label `needs-mre` added by @konstin on 2025-02-18 20:11_

---

_Comment by @konstin on 2025-02-18 20:12_

The environment variables are working for me generally, please feel free to open a new issue if they aren't working in specific scenarios.

---

_Closed by @konstin on 2025-02-18 20:12_

---
