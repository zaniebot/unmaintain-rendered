```yaml
number: 11639
title: Add a uv wrapper
type: issue
state: closed
author: cliebBS
labels:
  - enhancement
assignees: []
created_at: 2025-02-19T20:37:00Z
updated_at: 2025-02-20T14:54:42Z
url: https://github.com/astral-sh/uv/issues/11639
synced_at: 2026-01-10T03:50:31Z
```

# Add a uv wrapper

---

_Issue opened by @cliebBS on 2025-02-19 20:37_

### Summary

With Gradle, there is `gradlew`, which acts as a Gradle launcher that reads a file in the project that specifies the version of Gradle that it should use, downloads and installs it if it is missing from the system, then executes the provided arguments using this specific version of Gradle.  SBT makes this even more streamlined by including this logic directly within the build tool, forking a subprocess with the correct SBT version from the original SBT process.

It would be very convenient for development if we could have something similar for `uv`.  I roughed out a simple `uvw` script that runs on MacOS with Homebrew that seems to work, with the caveat of relying on a non-standard file, `uv.version`, and of relying on the current directory to contain this file rather than using the current directory or its parents.

```bash
#!/bin/sh

# uvw: the uv wrapper
#
# Emulate similar tools like gradlew, where this script installs the version of uv specified by this
# project and executes the provided command using the correct version of uv.  This is performed by
# using pipx to do side-by-side installs of multiple uv versions, utilizing suffixes to prevent
# warring global installations of uv.  uv versions will be exposed to the system as `uv@version`,
# for example `uv@0.5.31`.

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )

. "${SCRIPT_DIR}/uv.version"

SUFFIX="@${UV_VERSION}"
BIN_NAME="uv${SUFFIX}"

# ensure that a global copy of uv is installed (should work with any version)
if ! command -v pipx &> /dev/null; then
  echo "! pipx not found on system, installing via homebrew"
  brew install pipx
fi

# ensure that a global copy of uv is installed (should work with any version)
if ! command -v ${BIN_NAME} &> /dev/null; then
  echo "! ${BIN_NAME} not found on system, installing via pipx"
  pipx install --suffix=${SUFFIX} uv==${UV_VERSION}
fi

${BIN_NAME} "$@"
```

Ideally, it would be `uv` itself that does this so that I don't need wrapper scripts everywhere like what happens with `gradlew`.  Just like how `uv` already automatically creates `.venv` when running `uv pip install` or `uv sync`, it would be nice if `uv` could read the uv version from `pyproject.toml` and then automatically download and use the correct version of uv without any changes to normal `uv` commands.

### Example

1. Install `uv` using official script, pip install, pipx install, brew install, etc.
2. uv 0.6.2 is now installed
3. `uv pip install -r requirements.txt`
4. `uv` sees that `pyproject.toml` specifies that uv 0.5.31 is specified for this project
5. uv 0.5.31 is not installed, so it is downloaded and installed (like how Python versions are installed by uv)
6. uv 0.5.31 is now installed into uv cache
7. uv then invokes 0.5.31 from cache to execute the `pip install -r requirements.txt` command

With this enhancement, the user doesn't need to worry about what version of `uv` they have installed or what version their current project is designed to work with.  This insulates them from the pain of breaking changes in `uv` causing different (or broken) behavior from what the original developer intended.

---

_Label `enhancement` added by @cliebBS on 2025-02-19 20:37_

---

_Comment by @konstin on 2025-02-20 09:17_

This sounds like the same proposal as #6662

---

_Closed by @cliebBS on 2025-02-20 14:53_

---

_Comment by @cliebBS on 2025-02-20 14:54_

Sorry, it appears my Google-foo failed me.  I've started contributing to the original discussion and closed this ticket.

---
