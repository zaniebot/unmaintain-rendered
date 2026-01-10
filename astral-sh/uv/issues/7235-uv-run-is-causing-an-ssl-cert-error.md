---
number: 7235
title: "`uv run` is causing an SSL_CERT error"
type: issue
state: closed
author: mattpodolak
labels:
  - question
assignees: []
created_at: 2024-09-09T23:35:40Z
updated_at: 2024-10-21T21:48:14Z
url: https://github.com/astral-sh/uv/issues/7235
synced_at: 2026-01-10T01:24:12Z
---

# `uv run` is causing an SSL_CERT error

---

_Issue opened by @mattpodolak on 2024-09-09 23:35_

I'm using uv from Git Bash on Windows, loaded in a conda env
```
$ uv --version
uv 0.4.1 (823f23e22 2024-08-30)
```

Whenever I do `uv run <script>` I get the following error:
```
$ uv run script.py 
warning: Ignoring invalid `SSL_CERT_FILE`. File does not exist: C:\Users\mattpnaconda3vs\lingocat-scraper\Library\ssl.
```

I think this mightve started happening after running `uv tool update-shell`. This error doesnt happen when doing `python script.py` from the same directory with the same conda env and venv activated.

Also worth noting, the path for the SSL file is wrong, it should be `C:\Users\mattp\anaconda3\envs\lingocat-scraper`

---

_Comment by @charliermarsh on 2024-09-09 23:37_

Is `SSL_CERT_FILE` set? Does `echo $SSL_CERT_FILE` show anything?

---

_Label `question` added by @charliermarsh on 2024-09-09 23:37_

---

_Comment by @mattpodolak on 2024-09-09 23:41_

thanks for the quick response again @charliermarsh 🙏🏽, yes it prints out the invalid path

---

_Comment by @mattpodolak on 2024-09-09 23:43_

Also interesting is that when I open Git Bash in another directory (no uv venv) `echo $SSL_CERT_FILE` doesnt print out anything

---

_Comment by @zanieb on 2024-10-21 21:48_

It sounds like something is setting `SSL_CERT_FILE`, but we don't do that ever.

---

_Closed by @zanieb on 2024-10-21 21:48_

---
