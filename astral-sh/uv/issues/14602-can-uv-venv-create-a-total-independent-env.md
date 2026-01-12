```yaml
number: 14602
title: "Can `uv venv` create a total independent env without $HOME/.local/share/uv"
type: issue
state: open
author: ansvver
labels:
  - question
assignees: []
created_at: 2025-07-14T09:41:13Z
updated_at: 2025-07-14T14:47:51Z
url: https://github.com/astral-sh/uv/issues/14602
synced_at: 2026-01-12T16:01:52Z
```

# Can `uv venv` create a total independent env without $HOME/.local/share/uv

---

_@ansvver_

### Question

I tried to create an independent env that I could copy to other machine, but failed (bin/python is a soft link) with:

`UV_CACHE_DIR=standard-vllm-server-to-b-test-uv UV_TOOLCHAIN_DIR=/data/qgpu-modelapp-conda/standard-vllm-server-to-b-test-uv uv venv standard-vllm-server-to-b-test-uv --python 3.10 --relocatable -v`

AND

`UV_LINK_MODE=copy uv venv standard-vllm-server-to-b-test-uv --python 3.10 --relocatable -v`

```
$ ll -tr
total 44
lrwxrwxrwx 1 dauser dauser   83 Jul 14 16:33 python -> /home/dauser/.local/share/uv/python/cpython-3.10.18-linux-x86_64-gnu/bin/python3.10
lrwxrwxrwx 1 dauser dauser    6 Jul 14 16:33 python3 -> python
lrwxrwxrwx 1 dauser dauser    6 Jul 14 16:33 python3.10 -> python
-rw-rw-r-- 1 dauser dauser 4110 Jul 14 16:33 activate
-rw-rw-r-- 1 dauser dauser 2634 Jul 14 16:33 activate.csh
-rw-rw-r-- 1 dauser dauser 4212 Jul 14 16:33 activate.fish
-rw-rw-r-- 1 dauser dauser 3883 Jul 14 16:33 activate.nu
-rw-rw-r-- 1 dauser dauser 2762 Jul 14 16:33 activate.ps1
-rw-rw-r-- 1 dauser dauser 2646 Jul 14 16:33 activate.bat
-rw-rw-r-- 1 dauser dauser 1728 Jul 14 16:33 deactivate.bat
-rw-rw-r-- 1 dauser dauser 1215 Jul 14 16:33 pydoc.bat
-rw-rw-r-- 1 dauser dauser 2383 Jul 14 16:33 activate_this.py
```

Any advice pls? Thx a lot!

### Platform

Linux 5.4.119-19.0009.56 x86_64 GNU/Linux

### Version

uv 0.7.20

---

_Label `question` added by @ansvver on 2025-07-14 09:41_

---

_Comment by @mabdullahsoyturk on 2025-07-14 12:42_

As far as I know, `uv` does not currently support totally independent environments (when I say totally independent, I mean even the python interpreter is included in the environment like in conda). Please correct me if I'm wrong. 

What you can do as an alternative is as follows (I don't recommend doing this unless you have to):

1. Get the Python interpreter:

```bash
uv python install ${PYTHON_VERSION} -i ${PYTHON_DIR}
```

2. Install dependencies to the global `site-packages` of the interpreter:

```bash
export UV_PYTHON=$(echo $PYTHON_DIR/cpython-${PYTHON_VERSION}*)
uv pip install -p ${UV_PYTHON} -r requirements.txt --target ${UV_PYTHON}/lib/site-packages
```

3. Now, you can copy the interpreter:

```bash
scp -r username@remote:${PYTHON_DIR} /path/to/local/folder
```

4. You can use the copied interpreter with: 

```bash
/path/to/local/folder/bin/python <your_script>.py
```

---

_Comment by @zanieb on 2025-07-14 14:47_

Similar to

- https://github.com/astral-sh/uv/issues/7865
- https://github.com/astral-sh/uv/issues/6970

---
