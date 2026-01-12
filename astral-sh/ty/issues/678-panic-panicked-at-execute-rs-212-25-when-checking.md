```yaml
number: 678
title: "[panic] Panicked at .../execute.rs:212:25 when checking `.../gen_credits.py`: `infer_definition_types(Id(c406)): execute: too many cycle iterations`"
type: issue
state: closed
author: pawamoy
labels:
  - fatal
assignees: []
created_at: 2025-06-18T14:06:34Z
updated_at: 2025-06-19T19:26:32Z
url: https://github.com/astral-sh/ty/issues/678
synced_at: 2026-01-12T15:54:23Z
```

# [panic] Panicked at .../execute.rs:212:25 when checking `.../gen_credits.py`: `infer_definition_types(Id(c406)): execute: too many cycle iterations`

---

_@pawamoy_

EDIT: Maybe a duplicate of https://github.com/astral-sh/ty/issues/525? I don't think I have any self-referential or recursive type in the snippet below though.

```python
from collections.abc import Iterable
from typing import Union

from packaging.requirements import Requirement

PackageMetadata = dict[str, Union[str, Iterable[str]]]
Metadata = dict[str, PackageMetadata]


def _extra_marker(req: Requirement) -> str | None:
    try:
        return next(marker[2].value for marker in req.marker._markers if getattr(marker[0], "value", None) == "extra")
    except StopIteration:
        return None


def _get_deps(metadata: Metadata) -> None:
    for pkg_name in metadata:
        for pkg_dependency in metadata[pkg_name].get("requires-dist", []):
            requirement = Requirement(pkg_dependency)
            extra_marker = _extra_marker(requirement)
```

```console
% uvx ty check  scripts/gen_credits.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/09627e4/src/function/execute.rs:212:25 when checking `/run/media/pawamoy/Data/dev/copier-uv/tests/tmp/scripts/gen_credits.py`: `infer_definition_types(Id(722e)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["ty", "check", "scripts/gen_credits.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(3022))
             at crates/ty_python_semantic/src/place.rs:597
   1: infer_definition_types(Id(7233))
             at crates/ty_python_semantic/src/types/infer.rs:167
   2: member_lookup_with_policy_(Id(281a))
             at crates/ty_python_semantic/src/types.rs:561
   3: infer_expression_types(Id(19bf))
             at crates/ty_python_semantic/src/types/infer.rs:243
   4: infer_expression_type(Id(19bf))
             at crates/ty_python_semantic/src/types/infer.rs:305
   5: member_lookup_with_policy_(Id(2815))
             at crates/ty_python_semantic/src/types.rs:561
   6: infer_expression_types(Id(1802))
             at crates/ty_python_semantic/src/types/infer.rs:243
   7: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:138
   8: check_types(Id(c00))
             at crates/ty_python_semantic/src/types.rs:88


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

Enabling the backtrace with `RUST_BACKTRACE=1` just shows `<unknown>` for every frame.

ty 0.0.1-alpha.11

---

_Renamed from "[panic]" to "[panic] Panicked at .../execute.rs:212:25 when checking `.../gen_credits.py`: `infer_definition_types(Id(c406)): execute: too many cycle iterations`" by @pawamoy on 2025-06-18 14:07_

---

_Label `fatal` added by @carljm on 2025-06-18 19:06_

---

_Comment by @AlexWaygood on 2025-06-18 19:22_

> I don't think I have any self-referential or recursive type in the snippet below though.

You're importing and using the `packaging` library, though, which definitely _does_ use recursive type aliases, so it's definitely plausible that this would be #256 🙃

---

_Comment by @pawamoy on 2025-06-18 20:25_

Right, didn't think of that!

---

_Comment by @AlexWaygood on 2025-06-19 19:26_

A more minimal repro of this is:

```py
from packaging.requirements import Requirement

def _get_deps(pkg_dependency: str) :
    extra_marker = Requirement(pkg_dependency).marker
```

So I think this is just #256 (we already track the panic on `packaging` in [this file](https://github.com/astral-sh/ruff/blob/10a1d9f01e899201c8d37d29071fe68752d09544/crates/ty_python_semantic/resources/primer/bad.txt#L10) -- it's due to [these recursive type aliases](https://github.com/pypa/packaging/blob/d0d5ad8687f666bea942d1ab4ee2feb5fa019d04/src/packaging/_parser.py#L44-L47)

But thanks for the report!

---

_Closed by @AlexWaygood on 2025-06-19 19:26_

---
