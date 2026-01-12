```yaml
number: 10896
title: Accidental edits in .venv carry over to other projects using the same dependency
type: issue
state: closed
author: thorbenk
labels:
  - question
assignees: []
created_at: 2025-01-23T13:20:24Z
updated_at: 2025-02-01T19:07:04Z
url: https://github.com/astral-sh/uv/issues/10896
synced_at: 2026-01-12T16:00:23Z
```

# Accidental edits in .venv carry over to other projects using the same dependency

---

_@thorbenk_

### Summary

Sometimes, I use go to definition in my editor and end up in a file in my project's `.venv`, make some edits and do not realize what I have done.

With package managers other than uv, this would be easily solved by deleting the `.venv` directory of that project and starting over.

However, due to the hardlinking going on with uv, my accidental change is now visible in all other projects I am working on. For people who do not know about uv's caching, this is hard to debug. To start over, the best way is probably to run `uv cache clean`.

Would it be possible to make the files read-only to prevent this scenario?

I verified the behavior with the following script:

It creates a library `venv_issue_dep` and publishes it as a wheel (using a local index on localhost).
It then creates two projects `project_a` and `project_b`, which use that dependency.
Finally, it makes an edit via `project_a`'s `.venv` and demonstrates that the change is visible also in `project_b`.

```bash
#!/bin/bash

# remove "issue" directory if it exists
# before running this script again!

mkdir issue
cd issue

mkdir project_a
cd project_a
uv init --python 3.12

cd ..

mkdir project_b
cd project_b
uv init --python 3.12

cd ..

mkdir venv_issue_dep
cd venv_issue_dep
uv init --lib --python 3.12
uv build

cd ..

mkdir -p local_index/venv-issue-dep
cp venv_issue_dep/dist/*.whl local_index/venv-issue-dep

cd local_index
nohup python3 -m http.server 9000 2>&1 &
PID=$!
cd ..

cd project_a
uv add --index local=http://127.0.0.1:9000 "venv_issue_dep==0.1.0"
echo "import venv_issue_dep; print('A', venv_issue_dep.hello())" > hello.py
cd ..

cd project_b
uv add --index local=http://127.0.0.1:9000 "venv_issue_dep==0.1.0"
echo "import venv_issue_dep; print('B', venv_issue_dep.hello())" > hello.py
cd ..

cd project_a
echo $PWD
uv run hello.py
# Prints "A Hello from venv-issue-dep!"
cd ..

cd project_b
echo $PWD
uv run hello.py
# Prints "B Hello from venv-issue-dep!"
cd ..

# Do an accidental edit
# Make sure not to destroy the hardlink.
# We just append a new version of hello()
echo "def hello(): return 'Hello World'" >> project_a/.venv/lib/python3.12/site-packages/venv_issue_dep/__init__.py

cd project_a
echo $PWD
uv run hello.py
cd ..
# prints "A Hello World"

cd project_b
echo $PWD
uv run hello.py
# prints "B Hello World"
#
# This is unexpected if you are unaware of the hard-linking going on.

kill $PID

find `uv cache dir` -path '*/venv_issue_dep/__init__.py' | xargs cat
# Shows that the new implementation is now in the cache.
```

### Platform

Ubuntu 24.04

### Version

uv 0.5.23

### Python version

Python 3.12

---

_Label `bug` added by @thorbenk on 2025-01-23 13:20_

---

_Comment by @charliermarsh on 2025-01-23 13:56_

No, unfrotunately I don't think we can prevent this kind of manual cache poisoning if you're using hard-links. We use copy-on-write links on macOS for this reason, but they're not as readily available on Linux (it varies by filesystem). Making these files read-only would almost certainly break some expectations for packages.

---

_Label `bug` removed by @charliermarsh on 2025-01-23 13:56_

---

_Label `question` added by @charliermarsh on 2025-01-23 13:56_

---

_Comment by @thorbenk on 2025-01-23 14:31_

Thanks @charliermarsh , I didn't know about copy-on-write links.

Can you expand on why read-only would break things? A package's files are read-only for a user if they are installed by a Linux system package manager, so I would have expected read-only would also work within virtual environments?

---

_Comment by @jonaslb on 2025-01-23 16:36_

I think the Rust reimplementation of the `cp` utility (which does `--reflink=auto` by default) could be a source of inspiration for how to attempt reflinking with a fallback on Linux, if someone ever wants to give it a go for `uv` (and if the maintainers don't see an issue with that impl, of course) :) Can be found here: https://github.com/uutils/coreutils/tree/main/src/uu/cp

---

_Comment by @charliermarsh on 2025-01-23 16:41_

(Nice, thank you for sharing that! I believe you can also set `--link-mode=clone` on Linux, and it _should_ work if they're supported.)

---

_Comment by @charliermarsh on 2025-01-23 19:16_

@thorbenk -- Totally. I'm not referring to any _specific_ breakage that I know of; it's more that it would be a significant deviation from / incompatibility with all other virtual environment-creators, and I'm very confident that it would lead to errors in some cases given the diversity in the ecosystem.

---

_Comment by @charliermarsh on 2025-02-01 19:07_

I'm going to close this as I think we're unlikely to change this. Perhaps we can use copy-on-write more as a default in the future though.

---

_Closed by @charliermarsh on 2025-02-01 19:07_

---
