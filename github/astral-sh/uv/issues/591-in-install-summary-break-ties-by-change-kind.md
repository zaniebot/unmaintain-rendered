---
number: 591
title: In install summary, break ties by change kind
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-12-08T03:21:52Z
updated_at: 2023-12-09T04:12:42Z
url: https://github.com/astral-sh/uv/issues/591
synced_at: 2026-01-07T13:12:16-06:00
---

# In install summary, break ties by change kind

---

_Issue opened by @charliermarsh on 2023-12-08 03:21_

Right now, it's inconsistent when a package is removed and installed:

```text
 + fastavro==1.9.1
 - fastavro==1.9.0
 + flask==1.1.2
 - fsspec==2023.12.0
 + fsspec==2023.12.1
 - google-api-core==2.14.0
 + google-api-core==2.15.0
 - google-auth==2.24.0
 + google-auth==2.25.1
 - google-cloud-compute==1.14.1
 + google-cloud-compute==1.15.0
 - google-cloud-core==2.3.3
 + google-cloud-core==2.4.1
 - google-cloud-iam==2.12.2
 + google-cloud-iam==2.13.0
 - google-cloud-resource-manager==1.10.4
 + google-cloud-resource-manager==1.11.0
 - google-cloud-secret-manager==2.16.4
 + google-cloud-secret-manager==2.17.0
 + googleapis-common-protos==1.62.0
 - googleapis-common-protos==1.61.0
 - grpc-google-iam-v1==0.12.7
 + grpc-google-iam-v1==0.13.0
 + pip==23.2.1
 - proto-plus==1.22.3
 + proto-plus==1.23.0
 + setuptools==69.0.2
 + yarl==1.9.4
 - yarl==1.9.3
```

---

_Label `bug` added by @charliermarsh on 2023-12-08 03:21_

---

_Comment by @charliermarsh on 2023-12-08 03:24_

It's possible we want to collapse these into a single entry, open to ideas. (It's obvious how to do that for registry distributions, but what about when a URL is involved?)

---

_Label `good first issue` added by @charliermarsh on 2023-12-08 03:24_

---

_Comment by @konstin on 2023-12-08 08:21_

I'd still go with `Updated {package} {version_or_url_old} to {version_or_url_new}`

---

_Referenced in [astral-sh/uv#598](../../astral-sh/uv/pulls/598.md) on 2023-12-09 04:09_

---

_Closed by @charliermarsh on 2023-12-09 04:12_

---
