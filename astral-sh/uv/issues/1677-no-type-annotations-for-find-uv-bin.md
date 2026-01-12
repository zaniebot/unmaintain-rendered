```yaml
number: 1677
title: No type annotations for find_uv_bin
type: issue
state: closed
author: gaborbernat
labels:
  - good first issue
  - question
assignees: []
created_at: 2024-02-19T01:55:40Z
updated_at: 2024-02-20T15:21:59Z
url: https://github.com/astral-sh/uv/issues/1677
synced_at: 2026-01-12T15:58:31Z
```

# No type annotations for find_uv_bin

---

_@gaborbernat_

I'm using `find_uv_bin` inside https://github.com/tox-dev/tox-uv/blob/main/src/tox_uv/_venv.py#L14, but mypy complain that this method is not exposed (no `__all__` containing it) and also no type annotations for it (because https://peps.python.org/pep-0561/#packaging-type-information is missing from the Python package, plus actual type annotations for it)

---

_Comment by @zanieb on 2024-02-19 02:04_

Hi! This is not a public function, `__main__` is intended for internal use only.

We could consider exposing `find_uv_bin` in `__init__` but I would say in general we are very hesitant to commit to a Python API at this stage.

---

_Label `question` added by @zanieb on 2024-02-19 02:04_

---

_Comment by @gaborbernat on 2024-02-19 02:11_

In the world of Python `__main__` is a public module... You might want to move it to something like _uv... But I'd ask for this one method to be public 😅

---

_Comment by @zanieb on 2024-02-19 02:27_

I mean, it's Python everything is "public". I don't think `__main__` is a module you're meant to be importing things from. It's meant to be a script entrypoint for the module.

---

_Comment by @gaborbernat on 2024-02-19 02:41_

That's not true by convention anything starting with an underscore is considered private. 

Magic modules are also considered public as far as I understand, so yes, they are meant to be importable. They just also serve another functionality on top of that. 

---

_Comment by @zanieb on 2024-02-19 03:35_

I would strongly discourage you from importing from our `__main__` module. Whether by convention or not (I cannot find any documentation saying either way), this is not a part of our public API.

It looks like it does have type annotations already? I'm happy to add a `py.typed` marker and consider moving it to `__init__.py` though if it's useful. I'm surprised we haven't gotten similar requests in Ruff (which has an identical setup).

---

_Label `good first issue` added by @zanieb on 2024-02-19 04:48_

---

_Closed by @zanieb on 2024-02-20 15:21_

---
