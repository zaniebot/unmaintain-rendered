```yaml
number: 12507
title: problem with kaleido package
type: issue
state: closed
author: alireza-hariri
labels:
  - bug
  - resolver
assignees: []
created_at: 2025-03-27T07:42:55Z
updated_at: 2025-04-14T08:57:50Z
url: https://github.com/astral-sh/uv/issues/12507
synced_at: 2026-01-12T16:01:05Z
```

# problem with kaleido package

---

_@alireza-hariri_

### Summary

Hello astral team 
Thank you for your amazing tool

i have a problem with installing `kaleido` (i don't know what it is. it was a dependency for `ptlflow`)


the `uv add kaleido` command give me this error
> Resolved 2 packages in 2.06s
error: Distribution `kaleido==0.2.1.post1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
hint: You're on Linux (`manylinux_2_40_x86_64`), but `kaleido` (v0.2.1.post1) only has wheels for the following platform: `manylinux2014_armv7l`

but the  `uv pip install kaleido` command works without a problem (it installs `kaleido-0.2.1-py2.py3-none-manylinux1_x86_64.whl`)
maybe there is a problem in the `kaleido` package metadata but i think at least uv should give me more hints on what can i do to solve this.

here is the page of this package on pypi: 
https://pypi.org/simple/kaleido/

here is the list of files that uv created in its cache  ~/.cache/uv/wheels-v3/pypi/kaleido
```
kaleido-0.0.3.post1-py2.py3-none-manylinux1_x86_64.msgpack
kaleido-0.1.0.post1-py2.py3-none-win32.msgpack
kaleido-0.1.0-py2.py3-none-macosx_10_10_x86_64.msgpack
kaleido-0.1.0-py2.py3-none-manylinux1_x86_64.msgpack
kaleido-0.2.0-py2.py3-none-macosx_10_11_x86_64.msgpack
kaleido-0.2.0-py2.py3-none-manylinux1_x86_64.msgpack
kaleido-0.2.1.post1-py2.py3-none-manylinux2014_armv7l.msgpack
kaleido-0.2.1-py2.py3-none-macosx_10_11_x86_64.msgpack
kaleido-0.2.1-py2.py3-none-manylinux1_x86_64 -> /home/alireza/.cache/uv/archive-v0/9rw-oxvr7_iuYHds6d47V
kaleido-0.2.1-py2.py3-none-manylinux1_x86_64.http
kaleido-0.2.1-py2.py3-none-manylinux1_x86_64.msgpack
```
### Platform

fedora 41

### Version

v0.6.10

### Python version

3.11.11

---

_Label `bug` added by @alireza-hariri on 2025-03-27 07:42_

---

_Comment by @zanieb on 2025-04-01 19:20_

It looks like `uv add` is resolving to `kaleido==0.2.1.post1` (as the newest version) but that platform only has wheels for `armv7`.

If you set a required environment (https://docs.astral.sh/uv/concepts/resolution/#required-environments), we won't use that version:

```
[tool.uv]
required-environments = [
    "sys_platform == 'linu' and platform_machine == 'x86_64'"
]
```

---

_Comment by @zanieb on 2025-04-01 19:20_

cc @konstin as an interesting resolution problem.

---

_Label `resolver` added by @zanieb on 2025-04-01 19:20_

---

_Comment by @alireza-hariri on 2025-04-02 03:24_

How can I force it to use specific wheel file ? 

---

_Comment by @zanieb on 2025-04-02 13:48_

You could add a URL source for it https://docs.astral.sh/uv/concepts/projects/dependencies/#url

You shouldn't need to do that though, did my solution not work?

---

_Comment by @konstin on 2025-04-08 09:04_

We have some prior reports for kaleido (https://github.com/astral-sh/uv/issues?q=is%3Aissue%20kaleido), I think this one is a duplicate of #10464, can you try the proposed workarounds in that issue?

---

_Comment by @alireza-hariri on 2025-04-14 08:56_

hi @konstin 
I didn't tried the proposed workarounds. but this workaround solved it for me
```
 override-dependencies = [
    "kaleido==0.2.1" 
]
```

---

_Closed by @konstin on 2025-04-14 08:57_

---
