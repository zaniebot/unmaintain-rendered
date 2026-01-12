```yaml
number: 8046
title: Ability to Configure uv to Fetch Python from a Mirrored python-build-standalone Repository
type: issue
state: closed
author: PandyaDarshit
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-09T13:13:46Z
updated_at: 2024-10-09T14:03:00Z
url: https://github.com/astral-sh/uv/issues/8046
synced_at: 2026-01-12T15:59:19Z
```

# Ability to Configure uv to Fetch Python from a Mirrored python-build-standalone Repository

---

_@PandyaDarshit_

Hi,
I am using uv for Python package management in my Dockerfile. Specifically, I use the following command to install Python and set up a virtual environment:

`RUN uv python install 3.8 && uv venv <virtual_env> --python 3.8
`

In a corporate environment where internet access is restricted, uv tries to fetch Python binaries from the [python-build-standalone repository on GitHub](https://github.com/indygreg/python-build-standalone). This often results in DNS or connection failures due to the restricted access. Below is the error message I receive:

`error: Failed to install cpython-3.8.20-linux-x86_64-gnu
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://github.com/indygreg/python-build-standalone/releases/download/20241002/cpython-3.8.20%2B20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
`

Feature Request
Is it possible to configure uv to pull Python from a mirrored repository (e.g., a corporate-hosted mirror of python-build-standalone)? If not, could this functionality be added in a future release?

This would allow us to maintain a mirror of the required Python versions within our network and bypass the need to access GitHub for downloading binaries.

Any guidance or alternative solutions would be greatly appreciated!

Thank you for your work on this project!



---

_Comment by @zanieb on 2024-10-09 13:43_

Please see https://github.com/astral-sh/uv/issues/8015

---

_Label `duplicate` added by @zanieb on 2024-10-09 13:43_

---

_Label `question` added by @zanieb on 2024-10-09 13:43_

---

_Closed by @charliermarsh on 2024-10-09 13:56_

---

_Comment by @PandyaDarshit on 2024-10-09 14:02_

Thanks @zanieb for quick response.

---
