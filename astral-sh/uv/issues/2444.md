```yaml
number: 2444
title: Version 0.1.19 breaks AWS CodeArtifact authentication
type: issue
state: closed
author: tom-engineering
labels:
  - bug
  - registry
assignees: []
created_at: 2024-03-14T02:43:49Z
updated_at: 2024-03-20T14:43:40Z
url: https://github.com/astral-sh/uv/issues/2444
synced_at: 2026-01-10T05:40:32Z
```

# Version 0.1.19 breaks AWS CodeArtifact authentication

---

_Issue opened by @tom-engineering on 2024-03-14 02:43_

`uv` version 0.1.19 gives a 401 error when connecting to a CodeArtifact repo, where 0.1.18 works fine.

uv version: 0.1.19
platform: macOS 14.4 (23E214)

Example:
`uv pip install -r requirements.txt --index-url https://aws:<redacted-key>@<redacted-subdomain>.codeartifact.us-east-1.amazonaws.com/pypi/eng/simple/`

```
error: Failed to download: tenacity==8.1.0
  Caused by: HTTP status client error (401 Unauthorized) for url (https://<redacted-subdomain>.codeartifact.us-east-1.amazonaws.com/pypi/eng/simple/tenacity/8.1.0/tenacity-8.1.0[...])
```

Can't give a repro but it does track with the changes to authentication between 0.1.18 and 0.1.19. The module that fails changes each time, so not specific to the module.

---

_Comment by @charliermarsh on 2024-03-14 02:49_

Did you try with `--native-tls`?

---

_Comment by @charliermarsh on 2024-03-14 02:51_

\cc @zanieb - my guess is this is related to the authentication store in some way?

---

_Comment by @tom-engineering on 2024-03-14 03:00_

> Did you try with `--native-tls`?

Just ran and same result ($UV_INDEX_URL is set as the `--index-url` flag was before):
```
uv pip install --native-tls -r requirements.txt
error: Failed to download and build: strict-rfc3339==0.7
  Caused by: HTTP status client error (401 Unauthorized) for url (https://<redacted-subdomain>.codeartifact.us-east-1.amazonaws.com/pypi/eng/simple/[...])
```

---

_Comment by @charliermarsh on 2024-03-14 03:00_

Okay, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-14 03:21_

---

_Label `bug` added by @charliermarsh on 2024-03-14 03:21_

---

_Label `registry` added by @charliermarsh on 2024-03-14 03:21_

---

_Closed by @charliermarsh on 2024-03-14 03:46_

---

_Comment by @zanieb on 2024-03-14 03:49_

Thanks for the report! We should have a fix released shortly

---

_Comment by @ealap on 2024-03-20 12:35_

Hi @charliermarsh, I can confirm that this issue was fixed on v0.1.20 (#2446) but the issue came back after v0.1.22 (probably due to #2449?)

---

_Comment by @charliermarsh on 2024-03-20 13:23_

Oh strange! Will take a look.

---

_Comment by @charliermarsh on 2024-03-20 14:00_

Unfortunately I'm not able to reproduce this when running against an authenticated CodeArtifact index. Are you certain that this is the issue you're running into? And certain that you're on the right uv version, etc.?

---

_Comment by @ealap on 2024-03-20 14:32_

I'm certain because I have specified the uv versions on a requirements.txt file I used for a project. The only versions where it doesn't worked are "uv==0.1.19" and "uv==0.1.22".

The index url I am trying to use points to a private JFrog artifactory PyPI repository.

---

_Comment by @zanieb on 2024-03-20 14:37_

@ealap can you open a new issue please and provide verbose logs (`-v`) with your password omitted?

This issue is for AWS CodeArtifact.

---

_Comment by @ealap on 2024-03-20 14:43_

Sorry, I thought the root issue was generally for basic HTTP authentication / in-URL credentials.

I will create a separate issue. 

---
