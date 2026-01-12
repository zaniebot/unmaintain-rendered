```yaml
number: 6740
title: "UP007: Flags code even when python version is less than 3.10"
type: issue
state: closed
author: bgianfo
labels:
  - documentation
assignees: []
created_at: 2023-08-21T20:56:25Z
updated_at: 2024-09-18T14:27:20Z
url: https://github.com/astral-sh/ruff/issues/6740
synced_at: 2026-01-12T15:54:46Z
```

# UP007: Flags code even when python version is less than 3.10

---

_@bgianfo_

The documentation for `UP007` points to [PEP 604](https://peps.python.org/pep-0604/), which claims it targets python 3.10.

The ruff rule seems to currently run even if you are targeting python 3.9, is this a bug? I can't seem to determine if the PEP 604 syntax was backported to py39 or not? 

Current Ruff Version: 0.0.285 

```toml
[tool.ruff]
# Support Python 3.9+.
target-version = "py39"
````

---

_Comment by @bgianfo on 2023-08-21 21:32_

It seems like `https://beta.ruff.rs/docs/settings/#pyupgrade-keep-runtime-typing` is the thing that controls the behavior here? Maybe that should be listed in the `UP007` rule docs? 

---

_Comment by @charliermarsh on 2023-08-21 21:55_

Yeah, if you have `from __future__ import annotations` in your file, then we will rewrite those annotations on pre-Python 3.10 versions, unless you have `keep-runtime-typing` set. I'll make sure that setting is referenced in those docs.

---

_Label `documentation` added by @charliermarsh on 2023-08-21 21:55_

---

_Closed by @charliermarsh on 2023-08-22 21:04_

---

_Comment by @vfazio on 2024-09-18 11:59_

I'm still seeing this behavior for type stub files.

```
(venv) vfazio@vfazio4 /mnt/development/libgpiod/bindings/python $ ruff -V
ruff 0.6.4

(venv) vfazio@vfazio4 /mnt/development/libgpiod/bindings/python $ tail pyproject.toml 

[build-system]
requires = ["setuptools", "wheel", "packaging"]

[project.optional-dependencies]
dev = ["mypy", "ruff"]

[tool.ruff.lint.pyupgrade]
# Preserve types, even if a file imports `from __future__ import annotations`.
keep-runtime-typing = true
(venv) vfazio@vfazio4 /mnt/development/libgpiod/bindings/python $ ruff check --target-version=py39 --select "UP007" gpiod
gpiod/_ext.pyi:35:44: UP007 Use `X | Y` for type annotations
   |
33 |     def set_values(self, values: dict[int, Value]) -> None: ...
34 |     def reconfigure_lines(self, line_cfg: LineConfig) -> None: ...
35 |     def read_edge_events(self, max_events: Optional[int]) -> list[EdgeEvent]: ...
   |                                            ^^^^^^^^^^^^^ UP007
36 |     @property
37 |     def chip_name(self) -> str: ...
   |
   = help: Convert to `X | Y`

gpiod/_ext.pyi:53:19: UP007 Use `X | Y` for type annotations
   |
51 |         self,
52 |         line_cfg: LineConfig,
53 |         consumer: Optional[str],
   |                   ^^^^^^^^^^^^^ UP007
54 |         event_buffer_size: Optional[int],
55 |     ) -> Request: ...
   |
   = help: Convert to `X | Y`

gpiod/_ext.pyi:54:28: UP007 Use `X | Y` for type annotations
   |
52 |         line_cfg: LineConfig,
53 |         consumer: Optional[str],
54 |         event_buffer_size: Optional[int],
   |                            ^^^^^^^^^^^^^ UP007
55 |     ) -> Request: ...
56 |     def read_info_event(self) -> InfoEvent: ...
   |
   = help: Convert to `X | Y`

Found 3 errors.
No fixes available (3 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Comment by @calumy on 2024-09-18 12:14_

> I'm still seeing this behavior for type stub files.

I believe this is intended behaviour as stub files are not used at run time, so can make use of new typing features: https://docs.astral.sh/ruff/settings/#target-version

---

_Comment by @vfazio on 2024-09-18 14:27_

> > I'm still seeing this behavior for type stub files.
> 
> I believe this is intended behaviour as stub files are not used at run time, so can make use of new typing features: https://docs.astral.sh/ruff/settings/#target-version

@calumy Thanks for the reply

It's a little strange because the ruff behavior deviates from pyupgrade. In this instance, pyupgrade does _not_ morph the pyi stubs when run with `--py39-plus`.

Despite the caveat that's called out in that link, when typing a library, it seems ideal to maintain consistency across the entire codebase and not mix syntax. In this case, we have public python interfaces that self annotate and use Union/Optional syntax since our minimal version is 3.9, but we have a C extension which requires the stub to type properly and is thus being told to use 3.10+ style syntax.

We can't change the Python interface syntax without the 3.9 interpreter blowing up.

I can exclude the stub file from UP007 checks, but I think there should be a preference for consistency within a codebase.

I realize i necroposted on this issue, so my apologies. This may warrant a new issue


---
