```yaml
number: 6690
title: uv python list shows duplicate symlinks
type: issue
state: closed
author: jooon
labels:
  - bug
  - good first issue
  - uv python
assignees: []
created_at: 2024-08-27T14:26:26Z
updated_at: 2024-08-28T23:09:26Z
url: https://github.com/astral-sh/uv/issues/6690
synced_at: 2026-01-10T04:45:09Z
```

# uv python list shows duplicate symlinks

---

_Issue opened by @jooon on 2024-08-27 14:26_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When I put uv managed python in $PATH, uv lists the same symlink twice. Likely once from uv knowing where its own python is and also finding it in $PATH.

```
$ uv --version
uv 0.3.4
$ uv python list
cpython-3.12.5-linux-x86_64-gnu     .local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python3.12
cpython-3.12.5-linux-x86_64-gnu     .local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python3 -> python3.12
cpython-3.12.5-linux-x86_64-gnu     .local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python -> python3.12
cpython-3.12.5-linux-x86_64-gnu     .local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python3 -> python3.12
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.10.12-linux-x86_64-gnu    /usr/bin/python3.10
cpython-3.10.12-linux-x86_64-gnu    /usr/bin/python3 -> python3.10
cpython-3.10.12-linux-x86_64-gnu    /bin/python3.10
cpython-3.10.12-linux-x86_64-gnu    /bin/python3 -> python3.10
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3.8
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3 -> python3.8
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python -> python3.8
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3 -> python3.8
pypy-3.7.13-linux-x86_64-gnu        <download available>
```

The reason I have put uv python is that I have finally replaced mise/pyenv with just uv.

I only ever used pyenv like this:

```
pyenv install 3.12 3.8
pyenv global 3.12 3.8
```

I mostly wanted `python3.12` and `python3.8` to end up in $PATH wherever I was and then the tools that when I needed a specific python I never configured `3.8` in a .python-version and then called `python3`, but rather called the minor version like `python3.8` directly. This works for instance great with tox.

My pyenv replacement is thus just:

```
uv python install 3.12 3.8
```

Instead of activating pyenv and putting a directory with shims in $PATH, I instead have this in my .bashrc which adds the bin directories in reverse version order.

```
UVPYTHON="$HOME/.local/share/uv/python"
case ":$PATH" in
    *:"$UVPYTHON"*)
    ;;
    *)
    PATH=$(ls -1dvr "$UVPYTHON"/*/bin 2> /dev/null | tr $'\n' ':')$PATH
    ;;
esac
```


---

_Label `bug` added by @charliermarsh on 2024-08-27 14:29_

---

_Comment by @zanieb on 2024-08-27 14:37_

Some of this behavior is intentional, per the discussion at https://github.com/astral-sh/uv/issues/5308 and change at https://github.com/astral-sh/uv/pull/5343

I think the only true duplicates here are, e.g., the first and third lines here:

```
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3 -> python3.8
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python -> python3.8
cpython-3.8.19-linux-x86_64-gnu     .local/share/uv/python/cpython-3.8.19-linux-x86_64-gnu/bin/python3 -> python3.8
```

The rest are "correct" though we could consider a better way to display interpreters with multiple symlinks.

---

_Comment by @jooon on 2024-08-27 16:25_

> I think the only true duplicates here are, e.g., the first and third lines here:

Yes. Those lines are what I referred to as duplicates.


---

_Label `good first issue` added by @zanieb on 2024-08-27 16:29_

---

_Label `uv python` added by @zanieb on 2024-08-27 16:29_

---

_Comment by @charliermarsh on 2024-08-28 23:09_

I believe this was closed by https://github.com/astral-sh/uv/pull/6740.

---

_Closed by @charliermarsh on 2024-08-28 23:09_

---
