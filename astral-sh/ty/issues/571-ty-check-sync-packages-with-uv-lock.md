```yaml
number: 571
title: "`ty check` sync packages with `uv.lock`"
type: issue
state: closed
author: dkang-quora
labels:
  - needs-info
assignees: []
created_at: 2025-06-03T06:37:28Z
updated_at: 2025-06-03T07:47:03Z
url: https://github.com/astral-sh/ty/issues/571
synced_at: 2026-01-12T15:54:23Z
```

# `ty check` sync packages with `uv.lock`

---

_@dkang-quora_

### Summary

`.venv/bin/python -m ty check` modifies the existing installation and runs the check on the old version.
I use `uv pip sync requirements.txt` and `ty check` seems to sync with uv.lock which is out-of-sync in my usage. There should be an option to disable the sync.

.venv/lib/python3.12/site-packages/some_package-0.1.1255443.dist-info/
->
.venv/lib/python3.12/site-packages/some_package-0.1.1097115.dist-info/


### Version

ty 0.0.1-alpha.8

---

_Comment by @MichaReiser on 2025-06-03 06:56_

I don't think I follow the problem you're running into. ty doesn't have a uv integration. It simply attempts to locate your local virtual environment by respecting the `VIRTUAL_ENV` environment variable or checking if a `.venv` directory exists (and is a virtual environment). It then uses the packages that are installed into the virtual environment. 

Can you try running ty with `-vv`. It will then print additiona logs telling you which virtual environment it uses:

```
2025-06-03 08:54:43.43533 DEBUG Discovering virtual environment in `/Users/micha/astral/ruff-vscode`
2025-06-03 08:54:43.435343 DEBUG Found `.venv` folder at `/Users/micha/astral/ruff-vscode/.venv`
2025-06-03 08:54:43.435375 DEBUG Attempting to parse virtual environment metadata at '/Users/micha/astral/ruff-vscode/.venv/pyvenv.cfg'
2025-06-03 08:54:43.435409 DEBUG Searching for site-packages directory in `sys.prefix` path `/Users/micha/astral/ruff-vscode/.venv`
2025-06-03 08:54:43.436033 DEBUG Resolved site-packages directories for this virtual environment are: SitePackagesPaths({"/Users/micha/astral/ruff-vscode/.venv/lib/python3.12/site-packages"})
2025-06-03 08:54:43.436202 DEBUG Adding site-packages search path '/Users/micha/astral/ruff-vscode/.venv/lib/python3.12/site-packages'
```

> .venv/lib/python3.12/site-packages/some_package-0.1.1255443.dist-info/
> ->
> .venv/lib/python3.12/site-packages/some_package-0.1.1097115.dist-info/

Does that mean you've the same package installed twice in the same virtual environment with different versions?

---

_Label `needs-info` added by @MichaReiser on 2025-06-03 06:56_

---

_Comment by @dkang-quora on 2025-06-03 07:47_

Sorry, I think it's `uv run` which modified the env.

```
❯ uv run ty check
Uninstalled 2 packages in 94ms
Installed 2 packages in 14ms
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
```

---

_Closed by @dkang-quora on 2025-06-03 07:47_

---
