```yaml
number: 8944
title: "Bug(?): uv publish fails if unknown files are present in ./dist"
type: issue
state: closed
author: 3j14
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-11-08T15:03:31Z
updated_at: 2024-11-13T12:02:05Z
url: https://github.com/astral-sh/uv/issues/8944
synced_at: 2026-01-12T15:59:38Z
```

# Bug(?): uv publish fails if unknown files are present in ./dist

---

_@3j14_

## Description

When a file is created in the `./dist` directory before running `uv publish`, this file causes `uv publish` to fail. [The documentation states](https://docs.astral.sh/uv/reference/cli/#:~:text=Selects%20only%20wheels,ignoring%20other%20files) that it **ignores** all other files.

https://github.com/astral-sh/uv/blob/04c445a3dbdefe530ced0228c424b3a6d9827edc/docs/reference/cli.md?plain=1#L7835

This issue is clearly minor and does not really impact the user much (the error message is quite clear).

However, I think there is no reason to fail if an unknown file is present – as long as a wheel and/or sdist file is found.

If this failure is deliberate, then I would recommend change the documentation.

## Steps to reproduce

1. Build the project (using `uv build`). The build files will be generated in `./dist`.
2. Create a file in the `./dist` directory. On macOS, this could happen e.g. when opening the directory in Finder, which might create the `.DS_Store` file. Example: `touch ./dist/.DS_Store`
3. Run `uv publish`.

This will fail with the error: `error: File is neither a wheel nor a source distribution: 'dist/.DS_Store'`

## Where does this error come from

The error is raised in `uv_publish::files_for_publishing`:
https://github.com/astral-sh/uv/blob/0b4e5cffa6f13a1ddefb6b9cc4751814aa8b469b/crates/uv-publish/src/lib.rs#L257-L258

`uv_distribution_filename::DistFilename::try_from_normalized_filename` in turn checks if the files are valid wheel or sdist files. Example for wheels:
https://github.com/astral-sh/uv/blob/0b4e5cffa6f13a1ddefb6b9cc4751814aa8b469b/crates/uv-distribution-filename/src/wheel.rs#L29-L34

## Meta

**uv version**:
```text
uv 0.5.0 (Homebrew 2024-11-07)
```

---

_Assigned to @konstin by @konstin on 2024-11-08 15:04_

---

_Label `bug` added by @charliermarsh on 2024-11-09 02:20_

---

_Comment by @charliermarsh on 2024-11-09 02:20_

Thanks for the report! We'll take a look -- does seem like an oversight.

---

_Label `great writeup` added by @konstin on 2024-11-10 10:48_

---

_Comment by @maliayas on 2024-11-12 09:04_

I also saw a similar error that may be related when running `uv sync`. It gave me:

```
error: Failed to install: ruff-0.7.3-py3-none-macosx_11_0_arm64.whl (ruff==0.7.3)
  Caused by: The wheel is invalid: Unknown wheel data type: ".DS_Store"
```

After doing a `find . -name .DS_Store -exec rm -rf {} +` in `/Users/USER/.cache/uv` the error was gone.

P.S. I'm usin `uv 0.5.1 (f399a5271 2024-11-08)`

---

_Comment by @charliermarsh on 2024-11-12 14:00_

Interesting, did you open the uv cache manually at some point?

---

_Comment by @maliayas on 2024-11-13 00:03_

> Interesting, did you open the uv cache manually at some point?

Yes, I inspected it a little bit.

---

_Comment by @maliayas on 2024-11-13 00:08_

Is the fix https://github.com/astral-sh/uv/commit/ecccfa0ee0cd3ddcb99488e008afafca6c6f8556 only for `uv publish`? FWIW the bug happened to me with `uv sync`.

---

_Closed by @konstin on 2024-11-13 11:58_

---

_Closed by @konstin on 2024-11-13 11:58_

---

_Comment by @konstin on 2024-11-13 12:02_

@maliayas your case is unrelated to the `uv publish` case, it looks like the uv cache got corrupted by adding a `.DS_Store` entry.

---
