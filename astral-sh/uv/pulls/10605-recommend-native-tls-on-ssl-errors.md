```yaml
number: 10605
title: "Recommend `--native-tls` on SSL errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/native-tls
created_at: 2025-01-14T17:33:49Z
updated_at: 2025-01-14T18:17:22Z
url: https://github.com/astral-sh/uv/pull/10605
synced_at: 2026-01-12T16:09:23Z
```

# Recommend `--native-tls` on SSL errors

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/10574.

## Test Plan

```
❯ SSL_CERT_FILE=a cargo run pip install flask -n
   Compiling uv v0.5.18 (/Users/crmarsh/workspace/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 8.33s
     Running `target/debug/uv pip install flask -n`
⠦ Resolving dependencies...                                                                                                                                                                                                                     × Failed to fetch: `https://pypi.org/simple/flask/`
  ├─▶ Request failed after 3 retries
  ├─▶ error sending request for url (https://pypi.org/simple/flask/)
  ├─▶ client error (Connect)
  ╰─▶ invalid peer certificate: UnknownIssuer
  help: Consider enabling native TLS support via the `--native-tls` command-line flag
```


---

_Label `error messages` added by @charliermarsh on 2025-01-14 17:34_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-14 17:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/diagnostics.rs`:284 on 2025-01-14 17:48_

Maybe like

> Consider enabling use of system TLS certificates with the ...

For a bit more exposition on what's happening

---

_@zanieb reviewed on 2025-01-14 17:48_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-14 18:02_

---

_@zanieb approved on 2025-01-14 18:05_

---

_Merged by @charliermarsh on 2025-01-14 18:17_

---

_Closed by @charliermarsh on 2025-01-14 18:17_

---

_Branch deleted on 2025-01-14 18:17_

---
