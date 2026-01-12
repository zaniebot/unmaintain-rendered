```yaml
number: 9218
title: "error: Failed to install cpython-3.11.10-macos-aarch64-none"
type: issue
state: closed
author: AmineDjeghri
labels:
  - needs-mre
assignees: []
created_at: 2024-11-19T09:15:57Z
updated_at: 2024-11-20T05:02:15Z
url: https://github.com/astral-sh/uv/issues/9218
synced_at: 2026-01-12T15:59:44Z
```

# error: Failed to install cpython-3.11.10-macos-aarch64-none

---

_@AmineDjeghri_

`uv python install 3.11                       
`

```
error: Failed to install cpython-3.11.10-macos-aarch64-none
  Caused by: Failed to download https://github.com/indygreg/python-build-standalone/releases/download/20241016/cpython-3.11.10%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://objects.githubusercontent.com/github-production-release-asset-2e65be/162334160/384c4dfe-dbb7-4a3b-a558-f0955d233681?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=releaseassetproduction%2F20241119%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20241119T091402Z&X-Amz-Expires=300&X-Amz-Signature=329d86740ab7a39c6b41a5c7bba1efa2b9c88fe405aac8e14a9c43c89c248848&X-Amz-SignedHeaders=host&response-content-disposition=attachment%3B%20filename%3Dcpython-3.11.10%2B20241016-aarch64-apple-darwin-install_only_stripped.tar.gz&response-content-type=application%2Foctet-stream)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer

```

---

_Comment by @AmineDjeghri on 2024-11-19 09:24_

Yesterday it was working, this morning no, but now it's working again .

Maybe something was down / being updated on your side/ Github's side ? 

---

_Comment by @konstin on 2024-11-19 14:06_

I can't reproduce this from my end, are you using a proxy?

---

_Label `needs-mre` added by @konstin on 2024-11-19 14:06_

---

_Closed by @charliermarsh on 2024-11-20 05:02_

---
