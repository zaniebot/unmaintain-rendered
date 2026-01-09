---
number: 9285
title: extra-index-url does not take precedence over keyring-provider
type: issue
state: closed
author: maxbrunet
labels: []
assignees: []
created_at: 2024-11-20T17:29:50Z
updated_at: 2024-11-21T23:56:50Z
url: https://github.com/astral-sh/uv/issues/9285
synced_at: 2026-01-07T13:12:18-06:00
---

# extra-index-url does not take precedence over keyring-provider

---

_Issue opened by @maxbrunet on 2024-11-20 17:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Maybe I am reading the doc wrong, to me it seems the URL should be the 1st credentials source considered:

https://github.com/astral-sh/uv/blob/a0de83001c0ec6ea85067b811c573d55bc907435/docs/configuration/authentication.md?plain=1#L35-L39

For my case, I set `keyring-provider=subprocess` in the `pyproject.toml` for convenience during development:

```toml
[tool.uv]
extra-index-url = [
    "https://oauth2accesstoken@<region>-python.pkg.dev/<project>/<repository>/simple/",
]
keyring-provider = "subprocess"
```

And in another system, which does not have `keyring` installed, I would like to use `UV_EXTRA_INDEX_URL` to pass the credentials, but `uv` still tries to use `keyring`:

```console
$ export UV_EXTRA_INDEX_URL='https://oauth2accesstoken:<token>@<region>-python.pkg.dev/<project>/<repository>/simple/'
$ uv lock
zsh: command not found: keyring
  × No solution found when resolving dependencies ...

      hint: An index URL (https://<region>-python.pkg.dev/<project>/<repository>/simple/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

If I remove `keyring-provider` from `pyproject.toml`, it works.

I wonder if it is similar to #8248

### Version

```
uv 0.5.3 (56d362208 2024-11-19)
```

---

_Referenced in [astral-sh/uv#9286](../../astral-sh/uv/issues/9286.md) on 2024-11-20 17:40_

---

_Comment by @maxbrunet on 2024-11-21 23:56_

Also confusion from my part, sorry for the noise again 🙇

---

_Closed by @maxbrunet on 2024-11-21 23:56_

---
