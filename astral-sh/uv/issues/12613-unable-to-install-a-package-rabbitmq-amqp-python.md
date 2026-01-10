---
number: 12613
title: Unable to install a package - rabbitmq-amqp-python-client
type: issue
state: closed
author: akshaybabloo
labels:
  - bug
assignees: []
created_at: 2025-04-01T23:19:03Z
updated_at: 2025-08-06T02:44:17Z
url: https://github.com/astral-sh/uv/issues/12613
synced_at: 2026-01-10T01:25:22Z
---

# Unable to install a package - rabbitmq-amqp-python-client

---

_Issue opened by @akshaybabloo on 2025-04-01 23:19_

### Summary

I am trying to install `rabbitmq-amqp-python-client` with `uv add` and I am getting this error:

```
Resolved 33 packages in 756ms
  × Failed to build `python-qpid-proton==0.39.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying cproton.py -> build/lib.linux-x86_64-cpython-312
      copying proton/_transport.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_common.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_url.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_delivery.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_selectable.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_condition.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_tracing.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_handlers.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_reactor.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_data.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_wrapper.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_handler.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/handlers.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_utils.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_io.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/__init__.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/tracing.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_message.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/reactor.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_exceptions.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_events.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/utils.py -> build/lib.linux-x86_64-cpython-312/proton
      copying proton/_endpoints.py -> build/lib.linux-x86_64-cpython-312/proton
      running build_ext
      generating cffi module 'build/temp.linux-x86_64-cpython-312/cproton_ffi.c'
      already up-to-date
      building 'cproton_ffi' extension
      cc -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fdebug-default-version=4 -fPIC -I/tools/deps/include
      -I/tools/deps/include/ncursesw -I/tools/deps/libedit/include -fPIC -DPROTON_DECLARE_STATIC -I./include -I./src -I/home/akshay/.cache/uv/builds-v0/.tmpLcYzg4/include
      -I/home/akshay/.local/share/uv/python/cpython-3.12.0-linux-x86_64-gnu/include/python3.12 -c ./src/compiler/gcc/start.c -o build/temp.linux-x86_64-cpython-312/src/compiler/gcc/start.o -std=c99

      [stderr]
      /home/akshay/.cache/uv/builds-v0/.tmpLcYzg4/lib/python3.12/site-packages/setuptools/dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: Apache Software License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      cc: error: unrecognized command-line option ‘-fdebug-default-version=4’
      error: command '/usr/bin/cc' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

Seems to be working fine if I use `uv pip install`, because it's using Miniconda. I have opened an issue with them - https://github.com/rabbitmq/rabbitmq-amqp-python-client/issues/60 - and looks like it was fixed with pip.

### Platform

Ubuntu 22.04

### Version

uv 0.6.11

### Python version

Python 3.12.8

---

_Label `bug` added by @akshaybabloo on 2025-04-01 23:19_

---

_Comment by @zanieb on 2025-04-01 23:26_

Does `uv pip install` use a different compiler? The failure is using `cc`: `error: command '/usr/bin/cc' failed with exit code 1`. You can set the `CC` environment variable to use a different one. What compiler does `/usr/bin/cc` point to?

Does `-fdebug-default-version=4` come from `python-qpid-proton`'s build system? I think we should be sanitizing it from ours. Can you try `uv python install --reinstall` to ensure you have our latest patches?

---

_Assigned to @zanieb by @zanieb on 2025-04-01 23:26_

---

_Comment by @lukebakken on 2025-04-02 17:32_

FWIW, `uv add` works as expected on my up-to-date Arch Linux system. I only have `gcc` installed, not `clang`. I have Python 3.13 installed via `pacman` as well as `asdf`. Here is some information, maybe it will be useful:
* Project - https://github.com/lukebakken/rabbitmq-amqp-python-client-60
* Transcript of `uv add --verbose --verbose rabbitmq-amqp-python-client 2>&1 | tee /tmp/uv-add-arch-linux.txt` - https://github.com/lukebakken/rabbitmq-amqp-python-client-60/blob/main/uv-add-arch-linux.txt
* Output of `pacman --query` - https://github.com/lukebakken/rabbitmq-amqp-python-client-60/blob/main/pacman-query.txt

---

_Comment by @akshaybabloo on 2025-08-06 02:44_

Just tested it with uv 0.8.5. Seems to be working now

---

_Closed by @akshaybabloo on 2025-08-06 02:44_

---
