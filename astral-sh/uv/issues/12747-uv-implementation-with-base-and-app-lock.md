---
number: 12747
title: UV implementation with Base and app lock/requirement files
type: issue
state: open
author: ihelmer07
labels:
  - question
assignees: []
created_at: 2025-04-08T16:32:26Z
updated_at: 2025-04-09T21:00:36Z
url: https://github.com/astral-sh/uv/issues/12747
synced_at: 2026-01-10T01:25:24Z
---

# UV implementation with Base and app lock/requirement files

---

_Issue opened by @ihelmer07 on 2025-04-08 16:32_

### Question

I am trying to transition to uv from pip.
We have multiple (40+) custom apps that all share a common core of dependencies.
I want those common dependencies to be listed in a central repo so I can preinstall into the base docker image, as well as make things easier to upgrade (one upgrade spot vs 40).

I can't figure out how to support this set up.
`uv lock` are out it seems.

I can use `uv pip compile -o app.lock pyproject.toml` to generate an app.lock file based on the dependencies listed in the pyproject.toml.
However that pyproject.toml is misleading as it only can have the app dependencies listed, as listing the common dependencies in the pyproject.toml causes the requirements generated to include the common.

Can anyone help me out?


### Platform

Ubuntu

### Version

0.6.13

---

_Label `question` added by @ihelmer07 on 2025-04-08 16:32_

---

_Comment by @zanieb on 2025-04-08 17:12_

So, you want to generate a combined list of all your direct dependencies (but not transitive dependencies) for all of your projects?

---

_Comment by @ihelmer07 on 2025-04-08 17:53_

I believe so, although I'm likely unknowing of the nuance.

For example:
base requirements for all apps:
```
#base.txt
- django
- celery
- django-storages

#app A requirement
-r base.txt
- polars

#app B requirement:
-r base.txt
- anotherlib
```
I use the `-r` functionality today using pip.
however I'm trying to translate that to the uv framework, which doesn't seem possible as `uv sync`

`base.txt` is in a central repo so a dedicated team can maintain it.
the app teams have their own `app.txt` which they can add to, but `base.txt` is 'fixed' for them.

I don't believe uv supports this structure with `uv sync` or `uv lock`. so i'm investigting if `uv pip compile` can.
or do I force the app teams to update `app.txt` manually and skip the `pyproject.toml`

---

_Comment by @zanieb on 2025-04-08 18:09_

`uv pip compile` supports references with `-r`, yeah. You'd have... 

`base.in`
```
foo
bar
```

`appA.in`
```
-r base.in
foobar
```

then `uv pip compile appA.in -o appA.txt` to lock requirements for an application


---

_Comment by @ihelmer07 on 2025-04-08 18:13_

Makes sense - but I wouldn't be able to use the pyproject.toml dependancies, `uv sync` or `uv lock` commands and still achieve this same functionality?
Or is there a way?

---

_Comment by @zanieb on 2025-04-08 18:15_

You could also do `uv pip compile appA/pyproject.toml -o appA.txt`

I'm not sure I fully understand what you want though.

You could add a `base/pyproject.toml` then in `appA/pyproject.toml` just create a dependency on `base` (e.g., `uv add ../base`)  — then you'd have those dependencies in `appA` and could use `uv sync` etc.

---

_Comment by @ihelmer07 on 2025-04-09 01:21_

@zanieb - what you described was super helpful. I never considered having a `base/pyproject.toml` and have the `app/pyproject.toml` depend on base so thanks for the suggestion.
That's exactly what I needed.
Time to fully implement `uv` everywhere.

---

_Closed by @ihelmer07 on 2025-04-09 01:21_

---

_Reopened by @ihelmer07 on 2025-04-09 21:00_

---
