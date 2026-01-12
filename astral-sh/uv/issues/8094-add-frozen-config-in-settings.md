```yaml
number: 8094
title: Add frozen config in settings
type: issue
state: closed
author: ygorpontelo
labels: []
assignees: []
created_at: 2024-10-10T14:35:51Z
updated_at: 2024-11-23T11:48:27Z
url: https://github.com/astral-sh/uv/issues/8094
synced_at: 2026-01-12T15:59:19Z
```

# Add frozen config in settings

---

_@ygorpontelo_

uv is an excellent project, thanks for developing it!

From what i read in the docs, the run and sync command will always try to update the lock file. This is not really the behaviour i want, so i need to pass the flag --frozen to those commands everytime. Other people involved in the project also need to remember to add that flag as well. It would be nice to have a setting i can put in pyproject.toml to indicate i want those two commands to always have that flag. Something like:

```
[tool.uv]
frozen = ["sync", "run"]
```

Maybe other flags could have this too? I mainly want the frozen because it's easy to forget and i had to revert the lock files a couple times. I'm also a bit concerned with the add command that also always updates the other deps, but the flag for that one would be "no-sync" right?

---

_Comment by @czechnology on 2024-10-31 19:26_

This seems related to #8321.

---

_Comment by @ygorpontelo on 2024-10-31 19:33_

@czechnology  you are right. That should solve the use case i want, closing it.

---

_Closed by @ygorpontelo on 2024-10-31 19:33_

---

_Comment by @provinzkraut on 2024-11-23 11:48_

I'm in the same boat, but I would actually prefer if this were a config option / "install without updating" a separate command. I prefer not having my uv config spread between env vars and `pyproject.toml`, and would rather it just be in one place. Maybe this can be reopened? :)

---
