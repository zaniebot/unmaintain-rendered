```yaml
number: 6878
title: uv crashes if the temporary directory does not exists
type: issue
state: closed
author: gaborbernat
labels:
  - bug
assignees: []
created_at: 2024-08-30T16:30:32Z
updated_at: 2024-09-03T23:18:07Z
url: https://github.com/astral-sh/uv/issues/6878
synced_at: 2026-01-12T15:59:08Z
```

# uv crashes if the temporary directory does not exists

---

_@gaborbernat_

```shell
❯ uv venv -p 3.12
Using Python 3.12.5 interpreter at: /Users/bgabor8/.pyenv/versions/3.12.5/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate.fish

❯ env TMPDIR=.tmp uv pip install httpx
thread 'main' panicked at crates/uv-client/src/registry_client.rs:144:34:
called `Result::unwrap()` on an `Err` value: Custom { kind: NotFound, error: PathError { path: "/Users/bgabor8/git/testing/.tmp/.tmpwFhPkz", err: Os { code: 2, kind: NotFound, message: "No such file or directory" } } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

I think uv should create the folder if it does not exist rather than crash.

---

_Label `bug` added by @charliermarsh on 2024-08-30 16:30_

---

_Closed by @konstin on 2024-09-02 07:32_

---

_Comment by @gaborbernat on 2024-09-03 23:18_

Is there a reason why this needs to be an error and why we wouldn't instead just create this directory if it does not exist? That would be a much better user experience from my point of view.

---
