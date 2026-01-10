```yaml
number: 14191
title: "Search in the user scheme scripts directory last in `find_uv_bin`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/user-scheme-last
created_at: 2025-06-21T18:41:07Z
updated_at: 2025-08-08T11:46:33Z
url: https://github.com/astral-sh/uv/pull/14191
synced_at: 2026-01-10T06:44:33Z
```

# Search in the user scheme scripts directory last in `find_uv_bin`

---

_Pull request opened by @zanieb on 2025-06-21 18:41_

We should definitely not pick up user-level installations unless we can't find uv anywhere else. Otherwise, e.g., we would find a uv installed with `pipx install uv` before the one matching the uv module.

---

_Label `bug` added by @zanieb on 2025-06-21 18:41_

---

_Comment by @zanieb on 2025-06-21 19:44_

Arguably breaking, but I'd be surprised if it had real-world implications. We can slot into v0.8 if we want though.

---

_Comment by @Flamefire on 2025-06-23 07:30_

I suggest is searching the relative paths first: When uv is installed into `<target>/../uv` or `<prefix>/lib/python/site-packages/uv` you want `<target>/bin` or `<prefix>/bin` not `/usr/bin`, don't you? The latter might be something e.g. installed with apt-get and the former an update installed where the system-prefix isn't writable.

I can't see where it would find an unrelated "wrong" uv with the first logic. At worst it won't find anything, does it?

Maybe a more general "fix" is to have the `uv` binary import the `uv` module instead of the other way round like other Python "binaries" (actually executable scripts) do. If it really needs an executable binary (i.e. not a library) I guess that could be put next to/into the module in which case it is trivial to find.

---

_@ssbarnea approved on 2025-06-23 15:13_

@zanieb I tested this change with 4 combinations and it seems to be working well:

- uv installed at system level for uv python and for mise python, had to use `uv run python -m pip install --break-system-packages uv.whl` and similar for mise.
- venv created with `uv env` and with `python -m venv`
- tested only on macOS

I did not test with `pip --user` level because that one is known that packages installed like this are not to be available to venvs regardless how they are created.

All these 4 combinations worked this way as `python -m uv` was able to return the correct uv. Obviously that the `uv` executable was not in path for most of them, but that was expected.


---

_Comment by @zanieb on 2025-06-26 12:53_

> I suggest is searching the relative paths first: When uv is installed into <target>/../uv or <prefix>/lib/python/site-packages/uv you want <target>/bin or <prefix>/bin not /usr/bin, don't you?

Hm.. I think both the `<target>` and `<prefix>` cases are not common and I'm pretty hesitant to prioritize them above the other look up locations, even if there's only a small chance of false positives.

---

_Comment by @Flamefire on 2025-06-26 13:16_

> Hm.. I think both the `<target>` and `<prefix>` cases are not common and I'm pretty hesitant to prioritize them above the other look up locations, even if there's only a small chance of false positives.

In which case could there be a false positive? It looks at the location adjacent to the python module. That would work for pretty much all scenarios as this is the same location the binary would be for e.g. user or system installations, isn't it?

---

_Comment by @zanieb on 2025-06-26 13:25_

I mean, when we look 4 parents up we could just be looking in an arbitrary directory at that point?

---

_Comment by @Flamefire on 2025-06-26 14:42_

> I mean, when we look 4 parents up we could just be looking in an arbitrary directory at that point?

I see. Maybe we can match the parents against `lib/python*/site-packages/uv` to make this safe?

---

_Comment by @zanieb on 2025-06-26 14:45_

I considered that, but I was sort of hesitant to add more complexity. I can mull it over some more / seek feedback from more of the team.

---

_Marked ready for review by @zanieb on 2025-08-07 21:48_

---

_@jtfmumm approved on 2025-08-08 11:43_

---

_Merged by @zanieb on 2025-08-08 11:46_

---

_Closed by @zanieb on 2025-08-08 11:46_

---

_Branch deleted on 2025-08-08 11:46_

---
