```yaml
number: 222
title: "Add `--version` flag"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: add-version
created_at: 2022-09-18T05:07:07Z
updated_at: 2022-09-18T15:15:16Z
url: https://github.com/astral-sh/ruff/pull/222
synced_at: 2026-01-12T15:55:04Z
```

# Add `--version` flag

---

_@harupy_

```
> ruff -V
ruff (v0.0.40) 0.0.40

> ruff --version
ruff (v0.0.40) 0.0.40

> ruff --help
ruff (v0.0.40) 0.0.40
An extremely fast Python linter.

USAGE:
    ruff [OPTIONS] <FILES>...

ARGS:
    <FILES>...    

OPTIONS:
    -e, --exit-zero
            Exit with status code "0", even upon detecting errors

        --exclude <EXCLUDE>...
            List of paths, used to exclude files and/or directories from checks

        --extend-exclude <EXTEND_EXCLUDE>...
            Like --exclude, but adds additional files and directories on top of the excluded ones

    -f, --fix
            Attempt to automatically fix lint errors

        --format <FORMAT>
            Output serialization format for error messages [default: text] [possible values: text,
            json]

    -h, --help
            Print help information

        --ignore <IGNORE>...
            List of error codes to ignore

    -n, --no-cache
            Disable cache reads

    -q, --quiet
            Disable all logging (but still exit with status code "1" upon detecting errors)

        --select <SELECT>...
            List of error codes to enable

    -v, --verbose
            Enable verbose logging

    -V, --version
            Print version information

    -w, --watch
            Run in watch mode by re-running whenever files change
```

---

_Merged by @charliermarsh on 2022-09-18 15:15_

---

_Closed by @charliermarsh on 2022-09-18 15:15_

---
