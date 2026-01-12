```yaml
number: 520
title: "`unresolved-import` for `google.cloud` packages when `types-protobuf` is installed"
type: issue
state: closed
author: galah92
labels:
  - imports
assignees: []
created_at: 2025-05-27T07:20:59Z
updated_at: 2025-08-19T20:34:40Z
url: https://github.com/astral-sh/ty/issues/520
synced_at: 2026-01-12T15:54:23Z
```

# `unresolved-import` for `google.cloud` packages when `types-protobuf` is installed

---

_@galah92_

### Summary

I continuation of https://github.com/astral-sh/ty/issues/363, which was fixed in `0.0.1-alpha.7` but doesn't work with my dependencies.

I also use generate Protobuf in my code (which `google-cloud` packages heavily rely on), and use `mypy` until `ty` will be more stable.

When `uv run mypy .` with Protobuf code I get the following errors:
```
➜  python git:(main) uv run mypy .
src/main.py:11: error: Library stubs not installed for "google.protobuf"  [import-untyped]
src/main.py:12: error: Library stubs not installed for "google.protobuf.message"  [import-untyped]
src/main.py:12: note: Hint: "python3 -m pip install types-protobuf"
src/main.py:12: note: (or run "mypy --install-types" to install all missing stub packages)
src/main.py:12: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
src/main.py:39: error: Returning Any from function declared to return "bytes"  [no-any-return]
```

So I also `uv install --dev types-protobuf`. In total:
```
➜  Downloads uv init tytest
Initialized project `tytest` at `/Users/galah92/Downloads/tytest`
➜  Downloads cd tytest
➜  tytest git:(main) ✗ uv add ty google-cloud-pubsub mypy types-protobuf
Using CPython 3.13.3 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Resolved 28 packages in 406ms
Installed 27 packages in 54ms
...
```

And again I'm getting the following error when running `uv run ty check`:
> error[unresolved-import]: Cannot resolve imported module `google.cloud`


### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Comment by @MichaReiser on 2025-05-27 07:32_

Thanks. This is due to `google-stubs` using partial stubs, which ty currently doesn't support 

---

_Closed by @MichaReiser on 2025-05-27 07:32_

---

_Reopened by @carljm on 2025-08-19 18:50_

---

_Label `imports` added by @carljm on 2025-08-19 18:50_

---

_Comment by @carljm on 2025-08-19 18:50_

Per https://github.com/astral-sh/ty/issues/184#issuecomment-3201683384 there's still an issue here when `types-protobuf` is installed, we should look into it.

---

_Added to milestone `Beta` by @carljm on 2025-08-19 18:51_

---

_Closed by @Gankra on 2025-08-19 20:34_

---
