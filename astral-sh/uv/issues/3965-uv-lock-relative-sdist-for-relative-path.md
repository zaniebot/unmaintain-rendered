```yaml
number: 3965
title: "uv lock: relative sdist for relative path dependencies"
type: issue
state: closed
author: considerate
labels: []
assignees: []
created_at: 2024-06-02T12:28:22Z
updated_at: 2024-06-02T16:48:10Z
url: https://github.com/astral-sh/uv/issues/3965
synced_at: 2026-01-12T15:58:47Z
```

# uv lock: relative sdist for relative path dependencies

---

_@considerate_

Given a `requirements.txt` file that includes a relative path to a directory containing a distribution:

```
../my-package
```

With `uv pip compile --unstable-uv-lock-file requirements.txt` the generated lock has the package resolved to absolute paths like the following:
```toml
[[distribution]]
name = "my-package"
version = "0.1.0"
source = "directory+file:///home/me/path/to/my-package"

[distribution.sdist]
url = "file:///home/me/path/to/my-package"
```

This is not ideal since I want to commit the lock file to a git repo shared with other people and they won't have the same directory structure I have.

1. Is it possible to update the lock files to instead retain the relative path that was specified in the requirements.txt file?
2. Is there any other way to declare relative path dependencies that would not have this absolute path resolution?

---

_Comment by @considerate on 2024-06-02 12:40_

Note:
Specifying the dependency as `file:../my-package` results in the same output. 

---

_Comment by @considerate on 2024-06-02 12:52_

Possibly related to https://github.com/astral-sh/uv/issues/3506

---

_Comment by @considerate on 2024-06-02 12:56_

For reference, poetry with a `my-package = {path = "../my-package"}` dependency resolves to:

```toml
[[package]]
name = "my-package"
version = "0.1.0"

...

[package.source]
type = "directory"
url = "../my-package"
```


---

_Comment by @charliermarsh on 2024-06-02 16:47_

👍 Yeah this will certainly change. I am going to merge into https://github.com/astral-sh/uv/issues/3506 since it's meant to cover the same problem.

---

_Closed by @charliermarsh on 2024-06-02 16:47_

---

_Comment by @charliermarsh on 2024-06-02 16:48_

I don't believe there's any way around this with the lockfile right now. (It's not really intended for public use yet.)

---
