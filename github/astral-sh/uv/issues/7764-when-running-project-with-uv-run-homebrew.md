---
number: 7764
title: When running project with uv run, homebrew-installed dynamic libraries on MacOS are not found.
type: issue
state: open
author: tcwalther
labels: []
assignees: []
created_at: 2024-09-28T18:42:41Z
updated_at: 2025-08-24T07:13:28Z
url: https://github.com/astral-sh/uv/issues/7764
synced_at: 2026-01-07T13:12:17-06:00
---

# When running project with uv run, homebrew-installed dynamic libraries on MacOS are not found.

---

_Issue opened by @tcwalther on 2024-09-28 18:42_

I'm using torchaudio to load audio files, which under the hood uses ffmpeg. On MacOS with Apple Silicon, ffmpeg's libraries will be installed to /opt/homebrew/lib.

When I run my script regularly with Python's good old venv and `python scripts/worker.py`, torchaudio successfully finds ffmpeg in /opt/homebrew/lib. When I run my script with `uv run worker`, whereas `worker` is a defined script in `pyproject.toml`, torchaudio can't find ffmpeg and says that the search paths are:

- `$PROJECT_DIR/.venv/lib/python3.10/site-packages/torch/lib/libavutil.56.dylib` 
- `/usr/lib/libavutil.56.dylib`
- `libavutil.56.dylib`
- `/usr/local/lib/libavutil.56.dylib`
- `/usr/lib/libavutil.56.dylib`

(not sure why /usr/lib is duplicated; I'm just copying the output)

The file, however, is located at `/opt/homebrew/lib/libavutil.56.dylib`. Why is the dyld search path different when running my script via `uv run`?

---

_Comment by @zanieb on 2024-09-29 15:26_

Is the Python interpreter different? i.e. are you using a Brew interpreter in one case and a uv-managed interpreter in the other?

---

_Referenced in [astral-sh/uv#7870](../../astral-sh/uv/issues/7870.md) on 2024-10-02 15:56_

---

_Comment by @tcwalther on 2024-10-11 13:41_

@zanieb yes, that is probably the case. Thanks for pointing that out.

I solved it by symlinking ffmpeg into /usr/local/lib. Not perfect but works. I also looked at pixi, which I know uses uv under the hood, but found it a bit confusing tbh.

Would you consider adding /opt/homebrew/lib to the search path? Given homebrew's widespread adoption I believe it would be very helpful, and at least in my eyes it would be expected behaviour.

---

_Comment by @zanieb on 2024-10-25 20:24_

I'm pretty hesitant to add a special-case like that for all macOS users. We can definitely consider it though.

---

_Referenced in [astral-sh/uv#8576](../../astral-sh/uv/issues/8576.md) on 2024-10-25 20:25_

---

_Comment by @sonnen-coviance on 2024-10-25 21:00_

Also experiencing this issue. I think having a simple workaround documented may be helpful. It would be nice if `uv` could somehow account for this, maybe with a flag during `uv python install`

A minimal example to cause this error, make a project with `uv` that uses the `filemagic` python library, which relies on the C lib `libmagic`.

Install libmagic with brew
```Shell
brew install libmagic
```
Bootstrap a UV project
```Shell
uv init
uv add filemagic
uv python install
```

in your `hello.py` add the following
```Python
import magic


with magic.Magic() as m:
    print("This will cause an error")

```

Now run the file.
```Shell
uv run hello.py
```

And see the error caused at runtime:
```Shell
Traceback (most recent call last):
  File "<dir>/example/hello.py", line 1, in <module>
    import magic
  File "<dir>/example/.venv/lib/python3.11/site-packages/magic/__init__.py", line 18, in <module>
    from magic.identify import Magic, MagicError
  File "<dir>/example/.venv/lib/python3.11/site-packages/magic/identify.py", line 16, in <module>
    from magic import api
  File "<dir>/example/.venv/lib/python3.11/site-packages/magic/api.py", line 22, in <module>
    raise ImportError('Unable to find magic library')
ImportError: Unable to find magic library
```

If you now uninstall the `uv` instance of python and install it via homebrew the error no longer occurs.

```Shell
uv python uninstall 3.11
rm -rf .venv
brew install python@3.11
uv sync
uv run hello.py
```

The reason this works for homebrew installed versions of python is because the setup script mutates `Lib/ctypes/macholib/dyld.py` see here: https://github.com/Homebrew/homebrew-core/blob/7bf4109b2502839ff5facd1982522c51709749e1/Formula/p/python%403.11.rb#L192-L199

The default values can be found in `cpython` source code here:
https://github.com/python/cpython/blob/c5b99f5c2c5347d66b9da362773969c531fb6c85/Lib/ctypes/macholib/dyld.py#L29-L34

---

_Comment by @dryan on 2024-11-12 16:51_

Via #6971, `export DYLD_FALLBACK_LIBRARY_PATH="$HOMEBREW_PREFIX/lib"` in my `.zshrc` has solved this issue for me (with libcairo).

---

_Comment by @helmut-hoffer-von-ankershoffen on 2024-12-23 07:39_

You can add the fallback library path within python. Make sure you call before the `import` loading a module that depends on a homebrew installed library.
```
def patch_for_homebrew_libs():
    os.environ["DYLD_FALLBACK_LIBRARY_PATH"] = (
        f"{os.getenv('HOMEBREW_PREFIX', '/opt/homebrew')}/lib/"
    )
```


---

_Comment by @tibbe on 2025-01-29 13:25_

> Via [#6971](https://github.com/astral-sh/uv/issues/6971), `export DYLD_FALLBACK_LIBRARY_PATH="$HOMEBREW_PREFIX/lib"` in my `.zshrc` has solved this issue for me (with libcairo).

This doesn't work for me. It seems that macOS protections filter out `DYLD_FALLBACK_LIBRARY_PATH` (https://developer.apple.com/library/archive/documentation/Security/Conceptual/System_Integrity_Protection_Guide/RuntimeProtections/RuntimeProtections.html). For example, if I set `DYLD_FALLBACK_LIBRARY_PATH` in `~/.zshrc` and start a new shell it's not in `env`. Same in a terminal launched in VSCode.

---

_Referenced in [kkovaletp/photoview#560](../../kkovaletp/photoview/pulls/560.md) on 2025-08-08 09:37_

---

_Comment by @Iamrodos on 2025-08-24 07:13_

> You can add the fallback library path within python. Make sure you call before the `import` loading a module that depends on a homebrew installed library.
> 
> ```
> def patch_for_homebrew_libs():
>     os.environ["DYLD_FALLBACK_LIBRARY_PATH"] = (
>         f"{os.getenv('HOMEBREW_PREFIX', '/opt/homebrew')}/lib/"
>     )
> ```

Migrated to uv and hit the same issue as everyone else here. This is the only thing that would solve it for me. Thanks

```py
 if sys.platform == "darwin" and os.getenv("HOMEBREW_PREFIX"):
        os.environ["DYLD_FALLBACK_LIBRARY_PATH"] = (
            f"{os.getenv('HOMEBREW_PREFIX', '/opt/homebrew')}/lib/"
        )
```

---
