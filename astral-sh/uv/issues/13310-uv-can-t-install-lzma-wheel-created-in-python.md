---
number: 13310
title: "uv can't install lzma wheel created in Python"
type: issue
state: open
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-05-06T08:33:34Z
updated_at: 2025-05-06T08:35:01Z
url: https://github.com/astral-sh/uv/issues/13310
synced_at: 2026-01-10T01:25:31Z
---

# uv can't install lzma wheel created in Python

---

_Issue opened by @konstin on 2025-05-06 08:33_

> I made a wheel using LZMA compression format with this script. pip can install it, but unfortunately, I’m getting an error from uv saying “stream/file format not recognized.”
> 
> `make-lzma-wheel.py`
> 
> ```python
> # /// script
> # requires-python = ">=3.13"
> # dependencies = [
> #     "wheel",
> # ]
> # ///
> from zipfile import ZIP_LZMA
> 
> from wheel.wheelfile import WheelFile
> 
> with WheelFile("fakepkg-0.0.1-py3-none-any.whl", "w", compression=ZIP_LZMA) as zf:
>     zf.writestr("fakepkg/__init__.py", b'print("hello world")')
>     zf.writestr(
>         "fakepkg-0.0.1.dist-info/METADATA",
>         """\
> Metadata-Version: 2.4
> Name: fakepkg
> Version: 0.0.1
> Summary: fakepkg
> Requires-Python: >=3.8
> 
> fakepkg is a fake package for testing purposes.
> """,
>     )
>     zf.writestr(
>         "fakepkg-0.0.1.dist-info/WHEEL",
>         """\
> Wheel-Version: 1.0
> Generator: fakepkg
> Root-Is-Purelib: false
> """.encode(),
>     )
> 
> print("Created fakepkg-0.0.1-py3-none-any.whl")
> ```
> 
> pip can install this wheel:
> 
> ```console
> $ uv venv --seed
> $ .venv/bin/pip install fakepkg-0.0.1-py3-none-any.whl
> Processing ./fakepkg-0.0.1-py3-none-any.whl
> Installing collected packages: fakepkg
> Successfully installed fakepkg-0.0.1
> $ .venv/bin/python -c 'import fakepkg'
> hello world
> ```
> 
> `uv add` or `uv pip install` fails:
> 
> ```console
> $ cargo run -- pip install fakepkg-0.0.1-py3-none-any.whl
>   × Failed to read `fakepkg @ file:///private/tmp/hello/fakepkg-0.0.1-py3-none-any.whl`
>   ├─▶ Failed to read metadata: `/private/tmp/hello/fakepkg-0.0.1-py3-none-any.whl`
>   ├─▶ Failed to read from zip file
>   ├─▶ an upstream reader returned an error: stream/file format not recognized
>   ╰─▶ stream/file format not recognized
> 
> $ cargo run -- --version
> uv 0.7.2+24 (3218e364a 2025-05-05)
> ``` 

 _Originally posted by @j178 in [#13192](https://github.com/astral-sh/uv/issues/13192#issuecomment-2853682298)_

---

_Label `bug` added by @konstin on 2025-05-06 08:33_

---

_Comment by @konstin on 2025-05-06 08:34_

The last error here, "stream/file format not recognized", comes from lzma-sys. The zip crate recognizes the lzma encoding and tries to unpack it, but it seems there's some format incompatibility.

---

_Referenced in [astral-sh/uv#13192](../../astral-sh/uv/issues/13192.md) on 2025-05-06 08:38_

---
