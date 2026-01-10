```yaml
number: 16672
title: Do not error when no environment file is found
type: issue
state: open
author: alanmun
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-11-10T17:25:00Z
updated_at: 2025-11-10T19:26:42Z
url: https://github.com/astral-sh/uv/issues/16672
synced_at: 2026-01-10T03:23:55Z
```

# Do not error when no environment file is found

---

_Issue opened by @alanmun on 2025-11-10 17:25_

### Summary

In my bashrc I set

`export UV_ENV_FILE=.env # Tell uv it should always try to load .env variables if they are found`

If I am in a context where I 

1. Want to use `uv run`
2. Don't have a `.env` file

I then get:
```sh
uv run python
error: No environment file found at: `.env`
```

IMO that export should be best-effort, not command killing. If it fails to load an .env file, a warning at most makes sense, but the command should still proceed.


### Example

_No response_

---

_Label `enhancement` added by @alanmun on 2025-11-10 17:25_

---

_Comment by @zanieb on 2025-11-10 18:19_

Maybe we should have a `UV_DEFAULT_ENV_FILE` setting for this purpose? I'm sort of hesitant to not error if a general request for a file cannot be fulfilled.

---

_Comment by @alanmun on 2025-11-10 18:25_

> Maybe we should have a `UV_DEFAULT_ENV_FILE` setting for this purpose? I'm sort of hesitant to not error if a general request for a file cannot be fulfilled.

That would work for me and my use cases. I also want to point out that pipenv's behavior was to do best effort env file fetching. In my experience trying to switch from pipenv this is the biggest pain point I've hit with uv. Other than that I'm happy with uv

---

_Label `configuration` added by @zanieb on 2025-11-10 19:26_

---
