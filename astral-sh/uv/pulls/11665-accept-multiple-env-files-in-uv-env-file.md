```yaml
number: 11665
title: "Accept multiple `.env` files in `UV_ENV_FILE`"
type: pull_request
state: merged
author: Seven4ME
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: Seven4ME/fix-multiple-env-files
created_at: 2025-02-20T13:21:06Z
updated_at: 2025-02-21T06:05:09Z
url: https://github.com/astral-sh/uv/pull/11665
synced_at: 2026-01-12T16:09:56Z
```

# Accept multiple `.env` files in `UV_ENV_FILE`

---

_@Seven4ME_

According to the [UV documentation](https://docs.astral.sh/uv/configuration/files/#env), the UV_ENV_FILE environment variable should support multiple .env files, separated by spaces. However, when I tried using this feature in my repository, it didn’t work as expected.

To investigate, I checked the UV repository for relevant tests and found `run_with_multiple_env_files`. 
This test asserts the following `error: No environment file found at: .env1 .env2.`

This discrepancy could indicate either a mismatch between the documentation and the implementation or a bug in the code.

I decided to fix the issue in the code since the ability to pass multiple `.env` files is a valuable feature. 
If my fix isn’t appropriate, I’d be happy to make any necessary adjustments.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-20 22:28_

---

_Renamed from "fix: Multiple .env files in UV_ENV_FILE" to "Accept multiple `.env` files in `UV_ENV_FILE`" by @charliermarsh on 2025-02-21 05:29_

---

_Label `bug` added by @charliermarsh on 2025-02-21 05:29_

---

_Label `configuration` added by @charliermarsh on 2025-02-21 05:29_

---

_@charliermarsh approved on 2025-02-21 05:29_

Thank you! Looks like an oversight. I changed the fix to use Clap's built-in `value_delimiter` setting.

---

_Merged by @charliermarsh on 2025-02-21 05:37_

---

_Closed by @charliermarsh on 2025-02-21 05:37_

---

_Comment by @Seven4ME on 2025-02-21 06:05_

Oh, really...
Nice fix! Thanks for the quick reaction :) 

---
