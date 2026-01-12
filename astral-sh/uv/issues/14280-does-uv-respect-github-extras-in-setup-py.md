```yaml
number: 14280
title: "Does `uv` respect github extras in setup.py?"
type: issue
state: closed
author: lucharo
labels:
  - question
assignees: []
created_at: 2025-06-26T16:07:04Z
updated_at: 2025-07-11T01:55:11Z
url: https://github.com/astral-sh/uv/issues/14280
synced_at: 2026-01-12T16:01:46Z
```

# Does `uv` respect github extras in setup.py?

---

_@lucharo_

### Question

I am working with a python project that has a `setup.py` rather than a `pyproject.toml`. 

In it I have declares some optional dependency groups (`extra-requires`) to a github installed package, i.e.: 

```py
setup(
    ...,
    extras_require = {
        "optional" : [
             "pkg_from_gh @ git+https://github.com/some/package.git" 
         ]
    }
```

---

then, I am installing this library in the shebang of a local CLI I use, like:

```sh
#!/usr/bin/env -S uv run --script --index-strategy unsafe-best-match
# /// script
# dependencies = [
#   "typer",
#   "rich",
#   "mypkg[optional]",
# ] 
```

and I am getting: 

```txt
error: Package pkg_from_gh attempted to resolve via URL: git+https://github.com/some/package.git. URL dependencies must be expressed as direct requirements or constraints. Consider adding pkg_from_gh @ git+https://github.com/some/package.git to your dependencies or constraints file.
```

which again they are specified as such in my `setup.py`

Questions:
- is this intended behaviour? 
- is `uv` not able to read the `extra_requires` field in `setup.py`? do you not respect and only respect `pyproject.toml`? 



### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.6 (7f3e94a09 2025-05-19)

---

_Label `question` added by @lucharo on 2025-06-26 16:07_

---

_Comment by @konstin on 2025-06-26 20:53_

The problem here isn't the optional requirement, but that uv doesn't allow registry dependencies to point to URL dependencies. Only URL dependencies are allow to point to other URLs.

---

_Closed by @charliermarsh on 2025-07-11 01:55_

---
