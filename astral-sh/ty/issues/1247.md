```yaml
number: 1247
title: "[panic] Biopython Atom causes panic behaviour."
type: issue
state: closed
author: jday1
labels:
  - fatal
assignees: []
created_at: 2025-09-24T15:10:46Z
updated_at: 2025-09-24T15:48:39Z
url: https://github.com/astral-sh/ty/issues/1247
synced_at: 2026-01-10T02:06:25Z
```

# [panic] Biopython Atom causes panic behaviour.

---

_Issue opened by @jday1 on 2025-09-24 15:10_

### Summary

To reproduce:

install `biopython==1.85`

File: `example.py`

```
from Bio.PDB.Atom import Atom

def parse_atom(atom: Atom) -> None:    
    atom.parent
```

Full stacktrace:

```
➜  MonoCAT git:(main) ✗ uvx ty check example.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                       error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/jamesday/Documents/repositories/MonoCAT/example.py`: `infer_definition_types(Id(b01c)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.21
info: Args: ["ty", "check", "example.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(341a))
             at crates/ty_python_semantic/src/place.rs:706
   1: Type < 'db >::member_lookup_with_policy_(Id(2810))
             at crates/ty_python_semantic/src/types.rs:768
   2: infer_definition_types(Id(ab30))
             at crates/ty_python_semantic/src/types/infer.rs:97
   3: infer_deferred_types(Id(ab64))
             at crates/ty_python_semantic/src/types/infer.rs:143
   4: ClassLiteral < 'db >::explicit_bases_(Id(3818))
             at crates/ty_python_semantic/src/types/class.rs:1386
   5: ClassLiteral < 'db >::is_typed_dict_(Id(3818))
             at crates/ty_python_semantic/src/types/class.rs:1386
   6: infer_definition_types(Id(141b))
             at crates/ty_python_semantic/src/types/infer.rs:97
   7: ClassLiteral < 'db >::implicit_attribute_inner_(Id(7c00))
             at crates/ty_python_semantic/src/types/class.rs:1386
   8: Type < 'db >::member_lookup_with_policy_(Id(2807))
             at crates/ty_python_semantic/src/types.rs:768
   9: infer_scope_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:68
  10: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:522


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

### Version

0.0.1-alpha.21

---

_Label `fatal` added by @AlexWaygood on 2025-09-24 15:18_

---

_Closed by @carljm on 2025-09-24 15:48_

---
