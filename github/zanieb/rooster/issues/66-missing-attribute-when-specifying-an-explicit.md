---
number: 66
title: Missing attribute when specifying an explicit version
type: issue
state: closed
state_reason: completed
author: MichaReiser
labels: []
assignees: []
created_at: 2025-05-13T09:56:52Z
updated_at: 2025-05-13T12:44:24Z
url: https://github.com/zanieb/rooster/issues/66
synced_at: 2026-01-07T13:12:14-06:00
---

# Missing attribute when specifying an explicit version

---

_Issue opened by @MichaReiser on 2025-05-13 09:56_



```
./scripts/release.sh --version "0.0.1-alpha.1"
Traceback (most recent call last):
  File "/Users/micha/astral/ty/.venv/bin/rooster", line 10, in <module>
    sys.exit(app())
             ~~~^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/typer/main.py", line 340, in __call__
    raise e
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/typer/main.py", line 323, in __call__
    return get_command(self)(*args, **kwargs)
           ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/click/core.py", line 1161, in __call__
    return self.main(*args, **kwargs)
           ~~~~~~~~~^^^^^^^^^^^^^^^^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/typer/core.py", line 740, in main
    return _main(
        self,
    ...<6 lines>...
        **extra,
    )
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/typer/core.py", line 195, in _main
    rv = self.invoke(ctx)
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/click/core.py", line 1697, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
                           ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/click/core.py", line 1443, in invoke
    return ctx.invoke(self.callback, **ctx.params)
           ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/click/core.py", line 788, in invoke
    return __callback(*args, **kwargs)
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/typer/main.py", line 698, in wrapper
    return callback(**use_params)
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/rooster/_cli.py", line 290, in release
    update_version_file(
    ~~~~~~~~~~~~~~~~~~~^
        version_file, last_version or Version("0.0.0"), new_version
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    )
    ^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/rooster/_versions.py", line 155, in update_version_file
    to_cargo_version(new_version),
    ~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
  File "/Users/micha/astral/ty/.venv/lib/python3.13/site-packages/rooster/_versions.py", line 120, in to_cargo_version
    if not version.is_prerelease:
           ^^^^^^^^^^^^^^^^^^^^^
AttributeError: 'str' object has no attribute 'is_prerelease'
```

`new_version` is a `Version` when using bump (or no argument) but a string when specified as a CLI option.



---

_Referenced in [zanieb/rooster#67](../../zanieb/rooster/pulls/67.md) on 2025-05-13 10:09_

---

_Closed by @zanieb on 2025-05-13 12:44_

---
