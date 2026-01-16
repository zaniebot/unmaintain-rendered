```yaml
number: 22612
title: "--exclude glob patterns don't match on Windows"
type: issue
state: open
author: ahochheiden
labels:
  - bug
  - windows
assignees: []
created_at: 2026-01-15T23:48:03Z
updated_at: 2026-01-16T02:28:20Z
url: https://github.com/astral-sh/ruff/issues/22612
synced_at: 2026-01-16T03:04:37Z
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

_Label `question` added by @amyreese on 2026-01-16 02:01_

---

_Comment by @amyreese on 2026-01-16 02:01_

Is this a directory separator issue? What happens if you use backslashes in your exclude glob instead of forward slashes?  Eg, 

```
$ ruff format --check --force-exclude --exclude "build\moz.configure\*.configure" build\moz.configure\bindgen.configure
```

---

_Comment by @ahochheiden on 2026-01-16 02:26_

That yields the usual output, and this at the end:
```
io: D:\mozilla-source\firefox-fresh\buildmoz.configurebindgen.configure: The system cannot find the file specified. (os error 2)
--> buildmoz.configurebindgen.configure:1:1
```
Which is from MSYS eating the backslashes as escape sequences.

But we can get around that with `ruff format --check --force-exclude --exclude "build\\moz.configure\\*.configure" "build\\moz.configure\\bindgen.configure"`

But that also doesn't work:
```
/d/mozilla-source/firefox-fresh
$ ruff format --check --force-exclude --exclude "build\\moz.configure\\*.configure" 'build\moz.configure\bindgen.configure'
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

---

_Label `question` removed by @amyreese on 2026-01-16 02:28_

---

_Label `bug` added by @amyreese on 2026-01-16 02:28_

---

_Label `windows` added by @amyreese on 2026-01-16 02:28_

---
