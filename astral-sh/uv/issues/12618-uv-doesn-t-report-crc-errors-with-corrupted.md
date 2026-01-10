---
number: 12618
title: "uv doesn't report crc errors with corrupted packages where pip does"
type: issue
state: closed
author: jabbera
labels:
  - bug
assignees: []
created_at: 2025-04-02T03:17:06Z
updated_at: 2025-04-07T03:18:50Z
url: https://github.com/astral-sh/uv/issues/12618
synced_at: 2026-01-10T01:25:22Z
---

# uv doesn't report crc errors with corrupted packages where pip does

---

_Issue opened by @jabbera on 2025-04-02 03:17_

### Summary

Summary: uv pip install is not reporting corrupted packages. This resulted in a segfault trying to run my app. When installing with native pip a CRC failure is reported on install which made the issue extremely clear. I really don't know how long it would have taken for me to figure this out if I wasn't still using pip in my build pipeline.

First osqp seems to have published a bad wheel (https://files.pythonhosted.org:443/packages/00/04/5959347582ab970e9b922f27585d34f7c794ed01125dac26fb4e7dd80205/osqp-1.0.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)

<img width="844" alt="Image" src="https://github.com/user-attachments/assets/5eee0751-c2d4-4d78-a0a8-71b68d50be8e" />

When installing using pip with the following steps on an ubuntu vm:

```
python3.11 -m venv .venv
source .venv/bin/activate
pip install osqp==1.0.2 --no-cache
```

you get the following error:

```
Installing collected packages: numpy, MarkupSafe, joblib, scipy, jinja2, osqp
ERROR: Exception:
Traceback (most recent call last):
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/cli/base_command.py", line 180, in exc_logging_wrapper
    status = run_func(*args)
             ^^^^^^^^^^^^^^^
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/cli/req_command.py", line 245, in wrapper
    return func(self, options, args)
           ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/commands/install.py", line 452, in run
    installed = install_given_reqs(
                ^^^^^^^^^^^^^^^^^^^
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/req/__init__.py", line 72, in install_given_reqs
    requirement.install(
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/req/req_install.py", line 856, in install
    install_wheel(
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/operations/install/wheel.py", line 725, in install_wheel
    _install_wheel(
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/operations/install/wheel.py", line 585, in _install_wheel
    file.save()
  File "/home/adminmike/.venv/lib/python3.11/site-packages/pip/_internal/operations/install/wheel.py", line 384, in save
    shutil.copyfileobj(f, dest)
  File "/usr/lib/python3.11/shutil.py", line 197, in copyfileobj
    buf = fsrc_read(length)
          ^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/zipfile.py", line 966, in read
    data = self._read1(n)
           ^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/zipfile.py", line 1056, in _read1
    self._update_crc(data)
  File "/usr/lib/python3.11/zipfile.py", line 984, in _update_crc
    raise BadZipFile("Bad CRC-32 for file %r" % self.name)
zipfile.BadZipFile: Bad CRC-32 for file 'osqp/ext_builtin.cpython-311-x86_64-linux-gnu.so'
```

When doing the equivalent with uv:

```
uv venv --python=3.11
source .venv/bin/activate
uv pip install osqp==1.0.2 --reinstall
```

osqp installs successfully but when running my app I get this obscure runtime error:

```
Fatal Python error: Segmentation fault

Current thread 0x00007f319247bc40 (most recent call first):
  File "<frozen importlib._bootstrap>", line 241 in _call_with_frames_removed
  File "<frozen importlib._bootstrap_external>", line 1233 in create_module
  File "<frozen importlib._bootstrap>", line 573 in module_from_spec
  File "<frozen importlib._bootstrap>", line 676 in _load_unlocked
  File "<frozen importlib._bootstrap>", line 1147 in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 1176 in _find_and_load
  File "<frozen importlib._bootstrap>", line 1204 in _gcd_import
  File "/opt/hostedtoolcache/Python/3.11.11/x64/lib/python3.11/importlib/__init__.py", line 126 in import_module
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/interface.py", line 33 in algebra_available
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/interface.py", line 48 in default_algebra
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/interface.py", line 59 in default_algebra_module
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/interface.py", line 97 in construct_enum
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/interface.py", line 102 in <module>
  File "<frozen importlib._bootstrap>", line 241 in _call_with_frames_removed
  File "<frozen importlib._bootstrap_external>", line 940 in exec_module
  File "<frozen importlib._bootstrap>", line 690 in _load_unlocked
  File "<frozen importlib._bootstrap>", line 1147 in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 1176 in _find_and_load
  File "/agent/_work/_temp/.tox/test_gurobi/lib/python3.11/site-packages/osqp/__init__.py", line 4 in <module>
  File "<frozen importlib._bootstrap>", line 241 in _call_with_frames_removed
  File "<frozen importlib._bootstrap_external>", line 940 in exec_module
  File "<frozen importlib._bootstrap>", line 690 in _load_unlocked
```


### Platform

ubuntu 24.04

### Version

0.6.11

### Python version

3.11.11

---

_Label `bug` added by @jabbera on 2025-04-02 03:17_

---

_Comment by @charliermarsh on 2025-04-02 03:34_

That’s surprising, thanks.

---

_Comment by @jabbera on 2025-04-02 04:04_

That async zip lib seems like it might be the dodgy bit. Seems like the maintainer might have gotten busy with other things.

---

_Referenced in [Qiskit/documentation#2922](../../Qiskit/documentation/pulls/2922.md) on 2025-04-02 12:42_

---

_Assigned to @Gankra by @zanieb on 2025-04-02 13:29_

---

_Referenced in [astral-sh/uv#12623](../../astral-sh/uv/pulls/12623.md) on 2025-04-02 14:31_

---

_Closed by @Gankra on 2025-04-02 15:21_

---

_Comment by @hauntsaninja on 2025-04-06 21:00_

With uv==0.6.12 I see we start hitting this somewhere in some build: `Bad CRC (got 9da8edae, expected 00000000) for file:`

I'll look into it more eventually, but `expected 00000000` is a little suspicious to me
(the whl is maybe not fully public)

---

_Comment by @samypr100 on 2025-04-06 23:03_

Should the feature added in https://github.com/astral-sh/uv/pull/12623 be moved to be behind an env var for the time being given its seemingly breaking under some scenarios? e.g. `UV_CRC_MODE` with enforce, lax values? Defaulting to `lax` (previous behavior)

---

_Comment by @charliermarsh on 2025-04-07 00:48_

Yeah, I think we should make this opt-in for now. PR welcome.

---

_Referenced in [astral-sh/uv#12694](../../astral-sh/uv/issues/12694.md) on 2025-04-07 01:26_

---

_Referenced in [astral-sh/uv#12706](../../astral-sh/uv/pulls/12706.md) on 2025-04-07 03:12_

---

_Comment by @samypr100 on 2025-04-07 03:18_

Made a quick fix PR to make it opt-in https://github.com/astral-sh/uv/pull/12706, although just noticed #12694 so feel free to ignore this PR if there's a better short-term solution.

---

_Referenced in [astral-sh/uv#12722](../../astral-sh/uv/pulls/12722.md) on 2025-04-07 17:26_

---
