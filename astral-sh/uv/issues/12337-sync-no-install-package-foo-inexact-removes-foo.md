---
number: 12337
title: "`sync` `--no-install-package=foo --inexact` removes `foo`."
type: issue
state: closed
author: sanmai-NL
labels:
  - bug
assignees: []
created_at: 2025-03-20T10:34:59Z
updated_at: 2025-03-20T11:07:17Z
url: https://github.com/astral-sh/uv/issues/12337
synced_at: 2026-01-10T01:25:18Z
---

# `sync` `--no-install-package=foo --inexact` removes `foo`.

---

_Issue opened by @sanmai-NL on 2025-03-20 10:34_

### Summary

Here, an example with spaCy:

```
+ uv -v pip list
+ grep acy
DEBUG uv 0.6.8
DEBUG Searching for default Python interpreter in virtual environments or search path
DEBUG Found `cpython-3.12.9-linux-aarch64-gnu` at `/app/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
spacy              3.8.4
spacy-alignments   0.9.1
spacy-legacy       3.0.12
spacy-loggers      1.0.5
spacy-transformers 1.3.8
+ uv sync --frozen --inexact --no-dev --no-editable --no-install-project --no-install-package=blis --no-install-package=spacy --no-install-package=thinc --no-install-package=torch
Uninstalled 1 package in 29ms
Installed 44 packages in 362ms
Bytecode compiled 13503 files in 1.53s
 + anyio==4.9.0
 + cachetools==5.5.2
 + cffi==1.17.1
 + cryptography==44.0.2
 + diceware==1.0.1
 + dnspython==2.7.0
 + durationpy==0.9
 + email-validator==2.2.0
 + emoji==2.8.0
 + fastapi==0.115.11
 + fastapi-cli==0.0.7
 + google-auth==2.38.0
 + h11==0.14.0
 + httpcore==1.0.7
 + httptools==0.6.4
 + httpx==0.28.1
 + joblib==1.4.2
 + kubernetes==32.0.1
 + loguru==0.7.3
 + nltk==3.9.1
 + oauthlib==3.2.2
 + patternlite==3.6
 + pyasn1==0.6.1
 + pyasn1-modules==0.4.1
 + pycparser==2.22
 + python-dateutil==2.9.0.post0
 + python-dotenv==1.0.1
 + python-multipart==0.0.20
 + requests-oauthlib==2.0.0
 + rich-toolkit==0.13.2
 + rsa==4.9
 + ruamel-yaml==0.18.10
 + ruamel-yaml-clib==0.2.12
 + scipy==1.15.2
 - setuptools==77.0.1
 + setuptools==76.1.0
 + simplemma==1.0.0
 + sniffio==1.3.1
 + starlette==0.46.1
 + uvicorn==0.22.0
 + uvloop==0.21.0
 + watchfiles==1.0.4
 + websocket-client==1.8.0
 + websockets==15.0.1
--> 2da2d00f3142
[1/2] STEP 11/15: ADD . /app
--> 1da24b112264
[1/2] STEP 12/15: RUN --mount=type=cache,target=/root/.cache/uv     set -eux ;     uv -v pip list | grep acy;     uv sync --frozen --inexact --no-dev --no-editable --package=bep --no-install-package=blis --no-install-package=spacy --no-install-package=thinc --no-install-package=torch
+ uv -v pip list
+ grep acy
DEBUG uv 0.6.8
DEBUG Searching for default Python interpreter in virtual environments or search path
DEBUG Failed to inspect Python interpreter from virtual environment at `.venv/bin/python3` 
DEBUG Found `cpython-3.12.9-linux-aarch64-gnu` at `/usr/local/bin/python` (first executable in the search path)
Using Python 3.12.9 environment at: /usr/local
Error: building at STEP "RUN --mount=type=cache,target=/root/.cache/uv set -eux ;     uv -v pip list | grep acy;     uv sync --frozen --inexact --no-dev --no-editable --package=bep --no-install-package=blis --no-install-package=spacy --no-install-package=thinc --no-install-package=torch": while running runtime: exit status 1
```

### Platform

Linux, ghcr.io/astral-sh/uv:0.6.8-python3.12-bookworm-slim

### Version

0.6.8.

### Python version

3.12.9

---

_Label `bug` added by @sanmai-NL on 2025-03-20 10:34_

---

_Comment by @sanmai-NL on 2025-03-20 11:07_

It seems the `ADD` step somehow overwrote the `.venv`.

---

_Closed by @sanmai-NL on 2025-03-20 11:07_

---
