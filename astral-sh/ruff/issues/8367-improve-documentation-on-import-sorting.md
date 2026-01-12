```yaml
number: 8367
title: Improve documentation on import sorting
type: issue
state: closed
author: zanieb
labels:
  - documentation
  - configuration
assignees: []
created_at: 2023-10-30T20:56:35Z
updated_at: 2023-12-15T10:47:20Z
url: https://github.com/astral-sh/ruff/issues/8367
synced_at: 2026-01-12T15:54:48Z
```

# Improve documentation on import sorting

---

_@zanieb_

We've seen a lot of confusion about the relationship of import sorting with the linter and formatter. In the future, this situation may be improved i.e. by #8232 but until then it'd be great to have some clear guides in the documentation about how these currently interact and a recipe for enabling import sorting in projects.


---

_Label `documentation` added by @zanieb on 2023-10-30 20:56_

---

_Label `configuration` added by @zanieb on 2023-10-30 20:57_

---

_Comment by @bluthej on 2023-12-09 17:54_

I ran into that a few days ago and ended up tweaking my helix config to have both format and import sorting on save. I'd very much like to help out on this issue if you're looking for some hands on this! 

Right now I'm following what I've found on some other thread, which consists in piping the result of a call to `ruff check` (which sorts imports) to a call to `ruff format` (which does the rest of the formatting). Is that the kind of config you would like to be documented? Or did you have something a bit cleaner in mind? Ideally if there's a way to do both in a single call to Ruff that'd be great but AFAIK this is not doable right now.

---

_Comment by @zanieb on 2023-12-11 15:32_

I think we'd just want to document that `ruff format` does not sort imports and then give a minimal setup that does both e.g. maybe

```
ruff check --select I --fix
ruff format
```


---

_Closed by @MichaReiser on 2023-12-15 10:47_

---
