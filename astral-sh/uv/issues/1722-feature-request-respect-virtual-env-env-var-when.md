```yaml
number: 1722
title: "[Feature request] Respect `VIRTUAL_ENV` env var when running `uv venv` without an arg"
type: issue
state: closed
author: dwreeves
labels:
  - enhancement
assignees: []
created_at: 2024-02-19T23:42:12Z
updated_at: 2024-02-20T00:15:03Z
url: https://github.com/astral-sh/uv/issues/1722
synced_at: 2026-01-12T15:58:31Z
```

# [Feature request] Respect `VIRTUAL_ENV` env var when running `uv venv` without an arg

---

_@dwreeves_

Running `uv venv` does not respect the `VIRTUAL_ENV` env var, even though it feels like it should.

# Example

```console
$ mkdir testproject && cd testproject
$ uv --version
uv 0.1.5
$ VIRTUAL_ENV=customvenv uv venv --verbose
 uv_interpreter::python_query::find_default_python platform=Platform { os: Macos { major: 14, minor: 1 }, arch: X86_64 }, cache=Cache { root: "~/Library/Caches/uv", refresh: None, _temp_dir_drop: None }
      0.004652s   2ms DEBUG uv_interpreter::interpreter Using cached markers for: ~/.pyenv/versions/3.12.0/bin/python3.12
Using Python 3.12.0 interpreter at ~/.pyenv/versions/3.12.0/bin/python3.12
Creating virtualenv at: .venv
```

With the above, I expected that it would create a venv at `customvenv`, not at `.venv`.


---

_Renamed from "Respect `VIRTUAL_ENV` env var when running `uv venv` without an arg" to "[Feature request] Respect `VIRTUAL_ENV` env var when running `uv venv` without an arg" by @dwreeves on 2024-02-19 23:42_

---

_Comment by @zanieb on 2024-02-19 23:56_

Hm we generally use a `VIRTUAL_ENV` path to find the Python interpreter to use. It seems like it'd be pretty surprising to use it for a name here?

Related:
- https://github.com/astral-sh/uv/issues/1422

---

_Label `enhancement` added by @zanieb on 2024-02-19 23:56_

---

_Comment by @dwreeves on 2024-02-20 00:04_

Sounds good.

I'm not sure that this is surprising behavior per se, as setting the `VIRTUAL_ENV` variable shows intent to use it for the purposes of a venv. (I ran into this when trying to rely on `VIRTUAL_ENV` to do `uv venv && uv pip install`.)

That said, `VIRTUAL_ENV` is a bit of a hack, and this isn't a big deal in any event.

Want me to close this issue?

---

_Comment by @zanieb on 2024-02-20 00:15_

Yeah I'd say `VIRTUAL_ENV` is actually intended to use an existing environment elsewhere on the system.

Thanks for the feedback though. Hopefully we can tackle this more holistically. 

---

_Closed by @zanieb on 2024-02-20 00:15_

---
