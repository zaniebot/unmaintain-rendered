```yaml
number: 12200
title: "`uv pip install` insists on creating a .lock file in the current venv while `pip install` does not need this"
type: issue
state: closed
author: jiridanek
labels:
  - question
assignees: []
created_at: 2025-03-16T10:16:18Z
updated_at: 2025-03-16T22:24:41Z
url: https://github.com/astral-sh/uv/issues/12200
synced_at: 2026-01-12T16:00:57Z
```

# `uv pip install` insists on creating a .lock file in the current venv while `pip install` does not need this

---

_@jiridanek_

### Summary

There does not seem to be a way to deflect `uv` from creating a temporary directory (file?) in my `env::VIRTUAL_ENV` directory

```
(app-root) bash-5.1$ uv pip install cowsay -v
DEBUG uv 0.6.6
error: failed to create directory `/opt/app-root/src/.cache/uv`: Permission denied (os error 13)
```

```
(app-root) bash-5.1$ uv pip install --no-cache cowsay -v
DEBUG uv 0.6.6
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.11.9-linux-x86_64-gnu` at `/opt/app-root/bin/python3` (active virtual environment)
Using Python 3.11.9 environment at: /opt/app-root
error: Permission denied (os error 13) at path "/opt/app-root/.tmpQy7ZfT"
```

Setting `env::TMPDIR` does not change the behavior.

### Platform

podman 5.4.1 running ubi9 x86-64 image, on macOS 15.3 arm64 host

### Version

uv 0.6.6

### Python version

cpython-3.11.9-linux-x86_64-gnu

## Notes

The equivalent pip command does succeed

```
(app-root) bash-5.1$ pip install cowsay -v
Using pip 24.2 from /opt/app-root/lib64/python3.11/site-packages/pip (python 3.11)
Collecting cowsay
  Obtaining dependency information for cowsay from https://files.pythonhosted.org/packages/f1/13/63c0a02c44024ee16f664e0b36eefeb22d54e93531630bd99e237986f534/cowsay-6.1-py3-none-any.whl.metadata
  Downloading cowsay-6.1-py3-none-any.whl.metadata (5.6 kB)
Downloading cowsay-6.1-py3-none-any.whl (25 kB)
Installing collected packages: cowsay
  changing mode of /opt/app-root/bin/cowsay to 755
Successfully installed cowsay-6.1

[notice] A new release of pip is available: 24.2 -> 25.0.1
[notice] To update, run: pip install --upgrade pip
```

---

_Label `bug` added by @jiridanek on 2025-03-16 10:16_

---

_Renamed from "uv insists on creating a tmp dir in the current venv" to "uv insists on creating a tmp dir (file?) in the current venv" by @jiridanek on 2025-03-16 10:16_

---

_Comment by @jiridanek on 2025-03-16 10:58_

The name seems to be coming from

```
impl Default for Builder<'_, '_> {
    fn default() -> Self {
        Builder {
            random_len: crate::NUM_RAND_CHARS,
            prefix: OsStr::new(".tmp"),
            suffix: OsStr::new(""),
            append: false,
            permissions: None,
            keep: false,
        }
    }
}
```

https://github.com/Stebalien/tempfile/blob/167f544abe388d90a84aa4c2220d8c423dec2db1/src/lib.rs#L231-L242

---

_Comment by @jiridanek on 2025-03-16 11:38_

I think I understand what's happening.

uv (sometimes) creates new files by creating a tempfile and then moving it to the correct place, so the apperance of the `.tmpXXXXXX` name is a red herring. The line it actually crashes when I reproduced this outside of podman is

https://github.com/astral-sh/uv/blob/e843433b07db08bdabd64b4cfe822e57b3a31154/crates/uv/src/commands/pip/install.rs#L224

What I'm therefore looking for is a way to disable the venv dir lockfile. Which there does not seem to be one.

---

_Renamed from "uv insists on creating a tmp dir (file?) in the current venv" to "uv insists on creating a tmp file in the current venv while `pip install` does not need this" by @jiridanek on 2025-03-16 11:41_

---

_Renamed from "uv insists on creating a tmp file in the current venv while `pip install` does not need this" to "`uv pip install` insists on creating a tmp file in the current venv while `pip install` does not need this" by @jiridanek on 2025-03-16 11:41_

---

_Renamed from "`uv pip install` insists on creating a tmp file in the current venv while `pip install` does not need this" to "`uv pip install` insists on creating a .lock file in the current venv while `pip install` does not need this" by @jiridanek on 2025-03-16 11:42_

---

_Comment by @zanieb on 2025-03-16 14:14_

We create a file lock to prevent concurrent mutation of the environment because it is unsafe. pip just doesn't implement such safety.

You're encountering a permission error when writing to the environment — given that pip succeeds I can only presume you have different permissions for the root directory of the environment and the subdirectories? I'm not sure why you'd have that?

---

_Label `bug` removed by @zanieb on 2025-03-16 14:14_

---

_Label `question` added by @zanieb on 2025-03-16 14:14_

---

_Comment by @jiridanek on 2025-03-16 16:55_

> We create a file lock to prevent concurrent mutation of the environment because it is unsafe.

I'm hitting this in a Dockerfile build, where concurrent access is more or less precluded, but I do understand that it's a reasonable safety/robustness feature to have in general.

> I can only presume you have different permissions for the root directory of the environment and the subdirectories?

Yes, that is the case. And it seems to be done by mistake

Before,

```
podman run --entrypoint /bin/bash --rm -it 7616b6ee0ff8 -c 'ls -AlFd $VIRTUAL_ENV'
drwxrwxr-x. 1 default root 40 Mar 16 09:57 /opt/app-root/
```

then the Dockerfile does

```
USER 0
COPY ${CODESERVER_SOURCE_CODE}/nginx/root/ /
```

The permissions on the source (local) directory being the standard Git ones

```
ls -AlFd notebooks/codeserver/ubi9-python-3.11/nginx/root/opt/app-root
drwxr-xr-x@ 4 jdanek  staff  128 Mar 14 15:16 notebooks/codeserver/ubi9-python-3.11/nginx/root/opt/app-root/
```

And that results in `/opt/app-root/` (which is my `$VIRTUAL_ENV`) to end up

```
podman run --entrypoint /bin/bash --rm -it 237a5692c108 -c 'ls -AlFd $VIRTUAL_ENV'
drwxr-xr-x. 1 root root 38 Mar 14 14:16 /opt/app-root/
```

so running uv as the `default` user now no longer works.

## Conclusion

It appears that somebody was careless with permissions and because `pip install` did not require write access for the VIRTUAL_ENV dir itself after the venv is properly initialized, nobody needed to fix it. For uv, I'll go fix it.

Thank you for your help.



---

_Closed by @jiridanek on 2025-03-16 16:55_

---

_Comment by @zanieb on 2025-03-16 22:24_

Glad you got it worked out, thank you following up!

---
