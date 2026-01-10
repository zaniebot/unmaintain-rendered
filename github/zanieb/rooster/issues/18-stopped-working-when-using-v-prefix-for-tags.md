---
number: 18
title: "Stopped working when using `v` prefix for tags "
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2024-02-28T13:02:03Z
updated_at: 2024-03-17T14:55:19Z
url: https://github.com/zanieb/rooster/issues/18
synced_at: 2026-01-10T00:09:01Z
---

# Stopped working when using `v` prefix for tags 

---

_Issue opened by @MichaReiser on 2024-02-28 13:02_

Ruff uses rooster for its release management (as you probably know ;)) and it uses `v` as prefix for its tags. 

I think https://github.com/zanieb/rooster/commit/4463324aafd351f78c63d0e657ed525928dd83d2 changed the `TAG_PREFIX` from `refs/tags/v` to `refs/tags` so that rooster can be used with `uv`. Unfortunately, this causes rooster to fail on ruff. 

```
Found last version tag 0.2.2.
Traceback (most recent call last):

  File "<frozen runpy>", line 198, in _run_module_as_main

  File "<frozen runpy>", line 88, in _run_code

  File "C:\Users\Micha\astral\ruff\.venv\Scripts\rooster.exe\__main__.py", line 8, in <module>
    sys.exit(app())
             ^^^^^

  File "C:\Users\Micha\astral\ruff\.venv\Lib\site-packages\rooster\_cli.py", line 49, in release
    changes = list(get_commits_between(config, repo, last_version))
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  File "C:\Users\Micha\astral\ruff\.venv\Lib\site-packages\rooster\_git.py", line 38, in get_commits_between
    repo.lookup_reference(

KeyError: 'refs/tags/0.2.2'
```

For now, I manually change the `TAG_PREFIX` and add the `v` prefix, but having a CLI option or similar would be nice.

---

_Comment by @zanieb on 2024-03-17 14:22_

Sorry I told @charliermarsh about this but should have mentioned it in a team channel. There's an option for this that Ruff is using now:

https://github.com/astral-sh/ruff/blob/861998612300520f2c18dbc7b8a6226300ceb508/pyproject.toml#L76

---

_Closed by @zanieb on 2024-03-17 14:22_

---

_Comment by @charliermarsh on 2024-03-17 14:55_

👍 I changed it here but it was after 0.3.0: https://github.com/astral-sh/ruff/commit/ea79f616bc0f75185723574c7032e340aa60941e#diff-50c86b7ed8ac2cf95bd48334961bf0530cdc77b5a56f852c5c61b89d735fd711R76

---
