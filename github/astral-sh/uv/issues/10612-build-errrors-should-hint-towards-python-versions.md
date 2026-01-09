---
number: 10612
title: Build errrors should hint towards python versions with a wheel
type: issue
state: open
author: konstin
labels:
  - error messages
assignees: []
created_at: 2025-01-14T19:08:20Z
updated_at: 2025-01-14T20:21:02Z
url: https://github.com/astral-sh/uv/issues/10612
synced_at: 2026-01-07T13:12:18-06:00
---

# Build errrors should hint towards python versions with a wheel

---

_Issue opened by @konstin on 2025-01-14 19:08_

This is the sibling problem to #2777: Build errors often occur when using a package on an unsupported python version. Instead of using one of the wheels that exist, uv tries to build and then spills a build error about this package, with no indication about why we tried to build in the first place (the difference to #2777 is that we have a source dist).

In the specific example, `msgpack` had wheels for up to python 3.12, but we're trying to build on python 3.13. The solution is to downgrade to 3.12 again. We should hint users towards selecting a different python version and/or implementation. We should disregard `requires-python` in the error message, since the default `requires-python` value may be a too high one.

We could also provide hints for platforms and/or architecture, though those are less actionable since the user is likely bound to a platform and architecture.

Example reproduction: While the hint is correct, it should hint the user at the solution (`uv sync -p 3.12`) instead.

```
$ uv init .
Initialized project `foo` at `[...]/foo`
$ uv add msgpack --exclude-newer 2023-11-01
warning: `VIRTUAL_ENV=/home/konsti/projects/uv/.venv` does not match the project environment path `.venv` and will be ignored
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 2 packages in 67ms
  × Failed to build `msgpack==1.0.7`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
[...]
      copying msgpack/unpack_define.h -> build/lib.linux-x86_64-cpython-313/msgpack
      copying msgpack/unpack_template.h -> build/lib.linux-x86_64-cpython-313/msgpack
      running build_ext
      building 'msgpack._cmsgpack' extension
      creating build/temp.linux-x86_64-cpython-313
      creating build/temp.linux-x86_64-cpython-313/msgpack
      cc -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -march=x86-64-v2 -fPIC -fPIC -I.
      -I/home/konsti/.cache/uv/builds-v0/.tmpBSSzb4/include -I/home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13 -c
      msgpack/_cmsgpack.cpp -o build/temp.linux-x86_64-cpython-313/msgpack/_cmsgpack.o

      [stderr]
      warning: no files found matching '*.c' under directory 'msgpack'
      msgpack/_cmsgpack.cpp:1326:72: warning: ‘Py_UNICODE’ is deprecated [-Wdeprecated-declarations]
       1326 | static CYTHON_INLINE size_t __Pyx_Py_UNICODE_strlen(const Py_UNICODE *u)
            |                                                                        ^
      In file included from /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13/unicodeobject.h:1014,
                       from /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13/Python.h:79,
                       from msgpack/_cmsgpack.cpp:16:
      /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13/cpython/unicodeobject.h:10:37: note: declared here
         10 | Py_DEPRECATED(3.13) typedef wchar_t Py_UNICODE;
            |                                     ^~~~~~~~~~
      msgpack/_cmsgpack.cpp: In function ‘size_t __Pyx_Py_UNICODE_strlen(const Py_UNICODE*)’:
[...]
      /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13/cpython/longobject.h:111:17: note: declared here
        111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
            |                 ^~~~~~~~~~~~~~~~~~~
      msgpack/_cmsgpack.cpp: In function ‘char __Pyx_PyInt_As_char(PyObject*)’:
      msgpack/_cmsgpack.cpp:24500:42: error: too few arguments to function ‘int _PyLong_AsByteArray(PyLongObject*, unsigned char*, size_t, int, int, int)’
      24500 |                 ret = _PyLong_AsByteArray((PyLongObject *)v,
            |                       ~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~
      24501 |                                            bytes, sizeof(val),
            |                                            ~~~~~~~~~~~~~~~~~~~
      24502 |                                            is_little, !is_unsigned);
            |                                            ~~~~~~~~~~~~~~~~~~~~~~~~
      /home/konsti/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13/cpython/longobject.h:111:17: note: declared here
        111 | PyAPI_FUNC(int) _PyLong_AsByteArray(PyLongObject* v,
            |                 ^~~~~~~~~~~~~~~~~~~
      error: command '/usr/bin/cc' failed with exit code 1

      hint: This usually indicates a problem with the package or the build environment.
  help: `msgpack` (v1.0.7) was included because `foo` (v0.1.0) depends on `msgpack`
```


---

_Label `error messages` added by @konstin on 2025-01-14 19:08_

---

_Comment by @edmorley on 2025-01-14 20:21_

This is a scenario I see come up in support tickets a fair amount with pip fwiw - so any improvements here would be very welcome 😄  

Thanks to wheels working pretty well most of the time it seems understandably many users aren't aware of what wheels are and how they differ from sdists - and definitely have no idea that their last successful build N months ago with a different Python version was using a wheel rather than an sdist for a particular package... so think the build failure is what needs to be fixed (by eg trying to install system packages).

I see the intro at https://docs.astral.sh/uv/reference/build_failures/ already has an explanation of what a wheel is, which helps towards the education part of this problem. However, I wonder if it would be worth adding an additional debugging step of "see if you can avoid building altogether" to the docs, to complement any uv error message improvement?

---
