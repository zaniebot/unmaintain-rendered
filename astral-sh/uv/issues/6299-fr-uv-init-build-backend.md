```yaml
number: 6299
title: "FR: uv init --build-backend"
type: issue
state: closed
author: henryiii
labels:
  - enhancement
  - help wanted
  - projects
assignees: []
created_at: 2024-08-21T05:03:33Z
updated_at: 2024-10-16T12:20:00Z
url: https://github.com/astral-sh/uv/issues/6299
synced_at: 2026-01-12T15:59:02Z
```

# FR: uv init --build-backend

---

_@henryiii_

I think it would be nice to support several common backends with `uv init`. Current hatchling is the default (which is a great default), but it would be nice to be able to support compiled backends (`maturin`, `scikit-build-core`, and `meson-python`), and maybe a few other popular PEP 621 pure-python backends like `setuptools`, `flit-core`, and `pdm-backend`. This could also have a "none"-like option for packages that don't have a library component at all, and are just an application like a website.

If implemented, there would be an optional argument like `--build-backend` (or `--backend` if you'd rather shorter names) that would default to `hatchling`. But it would take other options. Selecting one of these would generate from a slightly different template, providing at least a different `build-system.build-backend` and `build-system.requires`. Ideally, especially for the compiled backends, the example file would be adjusted a bit as well.

https://github.com/scientific-python/cookie is an example of a cookiecutter that supports lots of backends. I'd keep the template simple, but this is example of something supporting this without too much effort. 

---

_Label `enhancement` added by @konstin on 2024-08-21 06:23_

---

_Comment by @charliermarsh on 2024-08-21 15:39_

This makes sense to me.

---

_Label `projects` added by @zanieb on 2024-08-21 15:54_

---

_Label `help wanted` added by @charliermarsh on 2024-08-22 01:01_

---

_Comment by @samypr100 on 2024-10-02 04:05_

@henryiii I found myself wanting uv init with maturin recently 😅 , so I ended up making a PR #7857

---

_Closed by @konstin on 2024-10-16 12:20_

---
