```yaml
number: 8052
title: Support for wildcard in UV_INSECURE_HOST
type: pull_request
state: merged
author: fasterthanlime
labels:
  - configuration
assignees: []
merged: true
base: main
head: more-insecure-proxy
created_at: 2024-10-09T18:20:36Z
updated_at: 2024-10-12T12:55:26Z
url: https://github.com/astral-sh/uv/pull/8052
synced_at: 2026-01-12T16:08:08Z
```

# Support for wildcard in UV_INSECURE_HOST

---

_@fasterthanlime_

Allow '*' as a value to match all hosts, and provide `reqwest_blocking_get` for uv tests, so that they also respect UV_INSECURE_HOST (since they respect `ALL_PROXY`).

This lets those tests pass with a forward proxy - we can think about setting a root certificate later so that we don't need to disable certificate verification at all.

---

I tested this locally by running:

```bash
GIT_SSL_NO_VERIFY=true ALL_PROXY=localhost:8080 UV_INSECURE_HOST="*" cargo nextest run sync_wheel_path_source_error
```

With my forward proxy showing:

```
2024-10-09T18:20:16.300188Z  INFO fopro: Proxied GET https://files.pythonhosted.org/packages/08/fd/cc2fedbd887223f9f5d170c96e57cbf655df9831a6546c1727ae13fa977a/cffi-1.17.1-cp310-cp310-macosx_11_0_arm64.whl (headers 480.024958ms + body 92.345666ms)
2024-10-09T18:20:16.913298Z  INFO fopro: Proxied GET https://pypi.org/simple/pycparser/ (headers 509.664834ms + body 269.291µs)
2024-10-09T18:20:17.383975Z  INFO fopro: Proxied GET https://files.pythonhosted.org/packages/62/d5/5f610ebe421e85889f2e55e33b7f9a6795bd982198517d912eb1c76e1a53/pycparser-2.21-py2.py3-none-any.whl.metadata (headers 443.184208ms + body 2.094792ms)
```

---

_Comment by @fasterthanlime on 2024-10-09 18:21_

Note that other tests require adapting from `reqwest::blocking::get` to `reqwest_blocking_get` (the shim this PR introduces) — I'll do that if I get approvals on the approach here.

---

_@zanieb approved on 2024-10-09 18:25_

I don't know if I'd declare this as stronger semantics, it's just "Allow disabling SSL for all hosts", right?

---

_@zanieb reviewed on 2024-10-09 18:26_

---

_Review comment by @zanieb on `crates/uv-configuration/src/trusted_host.rs`:28 on 2024-10-09 18:26_

Can we avoid the subsequent comparison in this case? i.e. just do `if self.host == "*" { return true }`?

---

_Renamed from "Stronger UV_INSECURE_HOST semantics" to "Support for wildcard in UV_INSECURE_HOST" by @fasterthanlime on 2024-10-09 18:30_

---

_Review comment by @fasterthanlime on `crates/uv-configuration/src/trusted_host.rs`:28 on 2024-10-09 18:51_

For sure — in fact I’d like to go one step further and make TrustedHost an enumerated with a “Wildcard” variant if y’all like that direction.

---

_@fasterthanlime reviewed on 2024-10-09 18:51_

---

_@fasterthanlime reviewed on 2024-10-10 09:52_

---

_Review comment by @fasterthanlime on `crates/uv-configuration/src/trusted_host.rs`:28 on 2024-10-10 09:52_

> I’d like to go one step further and make TrustedHost an enumerated with a “Wildcard” variant

I ended up doing just that.

---

_Label `configuration` added by @charliermarsh on 2024-10-12 12:55_

---

_Merged by @charliermarsh on 2024-10-12 12:55_

---

_Closed by @charliermarsh on 2024-10-12 12:55_

---
