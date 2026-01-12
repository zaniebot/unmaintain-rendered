```yaml
number: 8290
title: "Cannot install with `uv pip install -r requirements.txt` when `--extra-index-url` is present"
type: issue
state: closed
author: NF-ada
labels:
  - question
assignees: []
created_at: 2024-10-17T15:18:15Z
updated_at: 2024-10-17T15:27:12Z
url: https://github.com/astral-sh/uv/issues/8290
synced_at: 2026-01-12T15:59:23Z
```

# Cannot install with `uv pip install -r requirements.txt` when `--extra-index-url` is present

---

_@NF-ada_

Hey, 

We have our own package index and when I specify deps in a `requirements.txt` I have: 

```
--extra-index-url <link>
--trusted-host <host>
my_pkg
```
I am able to use regular `pip` like `pip install -r requirements.txt` but when I try to use `uv pip install -r requirements.txt` I get: 

```
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at requirements.txt:3:1)
```
Which seems to imply the file isn't parsed properly.

For compatibility with other tools, I can't do away with the `requirements.txt` sadly. Is this a bug or is there a way to fix it? 

Thanks!


---

_Comment by @charliermarsh on 2024-10-17 15:18_

We don't support `--trusted-host` in a `requirements.txt` file.

---

_Comment by @NF-ada on 2024-10-17 15:23_

> We don't support `--trusted-host` in a `requirements.txt` file.

Is there a plan to support it in the future or an alternative? 

---

_Comment by @charliermarsh on 2024-10-17 15:24_

We do support the behavior -- just not in a `requirements.txt` file. You can use `--allow-insecure-host` on the command-line or in a `uv.toml` file (https://docs.astral.sh/uv/reference/settings/#allow-insecure-host).

---

_Label `question` added by @charliermarsh on 2024-10-17 15:24_

---

_Comment by @zanieb on 2024-10-17 15:25_

(And no we don't intend to support it in the file https://github.com/astral-sh/uv/issues/7702)

---

_Comment by @NF-ada on 2024-10-17 15:26_

Fair enough, thanks for the quick answer guys!

---

_Closed by @NF-ada on 2024-10-17 15:27_

---
