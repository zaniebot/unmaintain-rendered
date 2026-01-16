```yaml
number: 22612
title: "--exclude glob patterns don't match on Windows"
type: issue
state: open
author: ahochheiden
labels: []
assignees: []
created_at: 2026-01-15T23:48:03Z
updated_at: 2026-01-15T23:48:03Z
url: https://github.com/astral-sh/ruff/issues/22612
synced_at: 2026-01-16T00:02:55Z
```

# --exclude glob patterns don't match on Windows

---

_@ahochheiden_

### Summary

Glob patterns passed to `--exclude` work correctly on `macOS` but fail to match on `Windows`. 

**Context:**
I discovered this while migrating Firefox from `black` to `ruff format`. The [pyproject.toml](https://searchfox.org/firefox-main/source/pyproject.toml#82) exclusions use glob patterns that work correctly on `macOS` but not on `Windows`.

**Steps to reproduce:**

Given a file that has formatting issues at  `build/moz.configure/bindgen.configure` :

- Run `ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure`

Result:
```
/d/mozilla-source/firefox
$ ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure
unformatted: File would be reformatted
   --> build\moz.configure\bindgen.configure:1:1
30  |     Please update using 'cargo install cbindgen --force' or running
31  |     './mach bootstrap', after removing the existing executable located at
32  |     {}.
    -     """.format(
    -                 version, cbindgen_min_version, cbindgen
    -             )
33  +     """.format(version, cbindgen_min_version, cbindgen)
34  |         )
35  |     )
36  |
```

Expected result:
```
warning: No Python files found under the given path(s)
```

Note: Explicit path works fine:
```
/d/mozilla-source/firefox
$ ruff format --check --force-exclude --exclude "build/moz.configure" build/moz.configure/bindgen.configure
warning: No Python files found under the given path(s)
```


### Version

ruff 0.14.9

---
