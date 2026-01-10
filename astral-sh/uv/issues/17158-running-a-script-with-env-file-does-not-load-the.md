---
number: 17158
title: Running a script with --env-file does not load the env file
type: issue
state: closed
author: ac3673
labels:
  - question
assignees: []
created_at: 2025-12-17T05:53:19Z
updated_at: 2025-12-21T01:08:55Z
url: https://github.com/astral-sh/uv/issues/17158
synced_at: 2026-01-10T01:26:14Z
---

# Running a script with --env-file does not load the env file

---

_Issue opened by @ac3673 on 2025-12-17 05:53_

### Summary

Script like this test.py:
```python
# /// script
# requires-python = ">=3.13"
# dependencies = []
# ///
import os

def main() -> None:
    val = os.environ['HELLO']
    print(f"Hello from test.py {val}!")


if __name__ == "__main__":
    main()
```

An .env file:
```
HELLO=WORLD
```

```bash
ac3673@localhost [PROD]: ~ $ uv run --script test.py --env-file .env
Traceback (most recent call last):
  File "/home/ac3673/test.py", line 13, in <module>
    main()
    ~~~~^^
  File "/home/ac3673/test.py", line 8, in main
    val = os.environ['HELLO']
          ~~~~~~~~~~^^^^^^^^^
  File "<frozen os>", line 717, in __getitem__
KeyError: 'HELLO'
ac3673@localhost [PROD]: ~ $ export UV_ENV_FILE=.env
ac3673@localhost [PROD]: ~ $ uv run --script test.py
Hello from test.py WORLD!
```

### Platform

Ubuntu 22.04.2 LTS (GNU/Linux 5.19.0-35-generic x86_64)

### Version

0.9.18

### Python version

Python 3.13.0

---

_Label `bug` added by @ac3673 on 2025-12-17 05:53_

---

_Comment by @fatelei on 2025-12-21 01:06_

```shell
cargo run -p uv run --env-file .env a.py
   Compiling uv-cli v0.0.8 (/Users/fatelei/github/uv/crates/uv-cli)
   Compiling uv v0.9.18 (/Users/fatelei/github/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 11.97s
     Running `target/debug/uv run --env-file .env a.py`
Hello from test.py 1!
```

no problem, env file must be provided before the command to take effect, this is not a bug 

---

_Comment by @charliermarsh on 2025-12-21 01:08_

That's right -- arguments provided after `--script test.py` are considered arguments to `test.py`, rather than to uv.

---

_Closed by @charliermarsh on 2025-12-21 01:08_

---

_Label `bug` removed by @charliermarsh on 2025-12-21 01:08_

---

_Label `question` added by @charliermarsh on 2025-12-21 01:08_

---
