```yaml
number: 1449
title: "Authentication when downloading package from private registry using `oauth2accesstoken` fails with 401 Unauthorized"
type: issue
state: closed
author: fellhorn
labels: []
assignees: []
created_at: 2024-02-16T07:58:09Z
updated_at: 2024-02-23T08:24:35Z
url: https://github.com/astral-sh/uv/issues/1449
synced_at: 2026-01-12T15:58:28Z
```

# Authentication when downloading package from private registry using `oauth2accesstoken` fails with 401 Unauthorized

---

_@fellhorn_

Thanks a lot for this project, happy to see the so much progress in the python packaging ecosystem!

I tried to use `uv` with a private pip repository hosted in Google artifact registry:

```bash
> uv pip compile --extra-index-url https://oauth2accesstoken:$(gcloud auth print-access-token)@my-region-python.pkg.dev/my-project/my-repo/simple/ requirements.in

error: Failed to download: my-package==0.7.1
  Caused by: HTTP status client error (401 Unauthorized) for url (https://my-region-python.pkg.dev/my-project/my-repo/my-package/my-package-0.7.1-cp38-abi3-manylinux_2_28_x86_64.whl#sha256=my-sha)
```

This is the same both for `uv pip install` and `uv pip compile`.
It seems like the authentication for listing works (version `0.7.1` is read from the repo, I did not provide it), but the token is not used when downloading.

---

_Comment by @gwdekker on 2024-02-16 08:22_

FWIW, I am seeing something very similar for a gitlab-hosted artifact registry. Same error.

---

_Comment by @tfcace on 2024-02-17 07:05_

Probably related to #1458 

---

_Comment by @zanieb on 2024-02-23 04:34_

Should be resolved in the latest version ref #1886 

---

_Closed by @zanieb on 2024-02-23 04:34_

---

_Comment by @fellhorn on 2024-02-23 08:24_

Thanks, I can confirm it works now :+1: 

---
