```yaml
number: 12757
title: "Error when `tool.uv.sources` is set in system-level configuration file"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-04-08T19:41:51Z
updated_at: 2025-04-08T21:01:20Z
url: https://github.com/astral-sh/uv/pull/12757
synced_at: 2026-01-12T16:10:22Z
```

# Error when `tool.uv.sources` is set in system-level configuration file

---

_@charliermarsh_

## Summary

I think the lack of enforcement here is an oversight. We _do_ already enforce this for user-level configuration files (contrary to the issue -- at least, in my testing and from reading the code).

Closes https://github.com/astral-sh/uv/issues/12753.


---

_Label `error messages` added by @charliermarsh on 2025-04-08 19:41_

---

_Comment by @orishamir on 2025-04-08 20:05_

Hey, thank you so much for working on this! Your thoughtfulness and how much you care is incredible and highly appreciated
You mentioned user-level configuration is enforced, and after testing, I realized I misplaced `uv.toml` while testing user-level.
So you're correct
```dockerfile
FROM python:3.12-slim-bookworm
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

RUN uv init

COPY <<EOF /root/.config/uv/uv.toml
[sources]
doesntmatter = { index = "doesnt_matter"}
EOF

CMD ["uv", "sync"]
```
Errors as it should

---

_Comment by @orishamir on 2025-04-08 20:06_

Would you like me to edit the original issue? or add a note?

---

_Comment by @zanieb on 2025-04-08 20:12_

Wasn't this intentional? Since the system-level configuration could be shared by multiple uv versions we wanted to avoid validating it?

---

_Comment by @zanieb on 2025-04-08 20:14_

I don't see commentary about it in https://github.com/astral-sh/uv/pull/7851

---

_Comment by @charliermarsh on 2025-04-08 20:42_

> Wasn't this intentional? Since the system-level configuration could be shared by multiple uv versions we wanted to avoid validating it?

Can you explain what you mean by "avoid validating it"? We validate it each way, it's just that at present, we don't error if you include fields that we'll ultimately ignore. (Also, is this different than user-level configuration?)


---

_Comment by @zanieb on 2025-04-08 21:00_

> Also, is this different than user-level configuration?

Yeah my understanding was we would expect a user to control their own uv config but at the system level erroring on unknown fields could be problematic since it's administered by someone else and needs broader compatibility.

It sounds like my understanding is wrong though, and we always error on unknown fields in settings files? I looked at `validate_uv_toml` and it just validates that we're not silently ignoring _known_ fields that aren't supported in this file — that's fine with me.

---

_@zanieb approved on 2025-04-08 21:00_

---

_Merged by @charliermarsh on 2025-04-08 21:01_

---

_Closed by @charliermarsh on 2025-04-08 21:01_

---

_Branch deleted on 2025-04-08 21:01_

---

_Comment by @charliermarsh on 2025-04-08 21:01_

👍 Agreed, makes sense.

---
