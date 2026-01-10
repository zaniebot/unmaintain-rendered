```yaml
number: 6996
title: Remove unused import.
type: pull_request
state: merged
author: garthk
labels:
  - compatibility
assignees: []
merged: true
base: main
head: tolerate-python3.6
created_at: 2024-09-04T04:32:32Z
updated_at: 2024-09-04T22:24:40Z
url: https://github.com/astral-sh/uv/pull/6996
synced_at: 2026-01-10T12:53:37Z
```

# Remove unused import.

---

_Pull request opened by @garthk on 2024-09-04 04:32_

`_virtualenv.py` doesn't need to import `__future__.annotations`, as it has none.

Removing the import:

* Restores the action of the VIRTUALENV_PATCH on Python 3.6

* Eliminates 24 lines of error messages displayed by Python 3.6 when it starts in an environment created by uv:

```plaintext
Error processing line 1 of /tmp/tmp.ENwqZ0oeyb/lib/python3.6/site-packages/_virtualenv.pth:

  Traceback (most recent call last):
    File "~/.pyenv/versions/3.6.15/lib/python3.6/site.py", line 168, in addpackage
      exec(line)
    File "<string>", line 1, in <module>
    File "/tmp/tmp.ENwqZ0oeyb/lib/python3.6/site-packages/_virtualenv.py", line 3
      from __future__ import annotations
                                       ^
  SyntaxError: future feature annotations is not defined

Remainder of file ignored
```

(Python displays the errors above twice.)

I appreciate the Python team no longer support Python 3.6, but RedHat-style Linux distributions will support Python 3.6 in their `/usr/libexec/platform-python` until [releasever 8 expires in 2029](https://access.redhat.com/support/policy/updates/errata#RHEL8_Planning_Guide). I'm happy for the community to move on, in general, but don't see the harm in helping those who can't. 

I'm not yet sure what in the “remainder of file ignored” is necessary for my project's build, as I haven't yet finished digging that from under Hatch. I'll follow up on #6426 when I do, so we can concentrate on getting to the happy cow.

## Test Plan

```sh
( set -eu
  export VIRTUAL_ENV="$(mktemp -d)"
  ./target/release/uv venv "$VIRTUAL_ENV" --python=python3.6
  ./target/release/uv pip install cowsay        
  $VIRTUAL_ENV/bin/python -m cowsay --text 'Look, a talking cow!' )
  ```
  
Happy output:

```plaintext
Using Python 3.6.15 interpreter at: ~/.local/bin/python3.6
Creating virtualenv at: /tmp/tmp.VHl4XNi3oI
Activate with: source /tmp//tmp.VHl4XNi3oI/bin/activate
Resolved 1 package in 929ms
Installed 1 package in 17ms
 + cowsay==6.0
  ____________________
| Look, a talking cow! |
  ====================
                    \
                     \
                       ^__^
                       (oo)\_______
                       (__)\       )\/\
                           ||----w |
                           ||     ||
```


---

_@charliermarsh approved on 2024-09-04 13:40_

Thanks, while we don't support Python 3.6 (as you mention) this still seems like a reasonable thing to do.

---

_Merged by @charliermarsh on 2024-09-04 13:40_

---

_Closed by @charliermarsh on 2024-09-04 13:40_

---

_Label `compatibility` added by @charliermarsh on 2024-09-04 13:41_

---

_Comment by @garthk on 2024-09-04 22:24_

Thanks, @charliermarsh, and thanks @zanieb for fixing the snapshot.

---

_Branch deleted on 2024-09-04 22:24_

---
