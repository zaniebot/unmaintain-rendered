```yaml
number: 1098
title: "[panic] execute: too many cycle iterations"
type: issue
state: closed
author: obi-jerome
labels:
  - needs-mre
  - fatal
assignees: []
created_at: 2025-08-26T09:47:18Z
updated_at: 2025-09-05T01:27:23Z
url: https://github.com/astral-sh/ty/issues/1098
synced_at: 2026-01-10T02:06:24Z
```

# [panic] execute: too many cycle iterations

---

_Issue opened by @obi-jerome on 2025-08-26 09:47_

Hi, 
I'm testing ty and got this error:


```
error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/usr/src/app/xml_renderer.py`: `infer_definition_types(Id(70970)): execute: too many cycle iterations`
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.19
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: place_by_id(Id(6ec1d))
             at crates/ty_python_semantic/src/place.rs:688
   1: Type < 'db >::member_lookup_with_policy_(Id(6c490))
             at crates/ty_python_semantic/src/types.rs:635
   2: infer_definition_types(Id(70905))
             at crates/ty_python_semantic/src/types/infer.rs:167
   3: place_by_id(Id(6ec1c))
             at crates/ty_python_semantic/src/place.rs:688
   4: Type < 'db >::member_lookup_with_policy_(Id(6c48f))
             at crates/ty_python_semantic/src/types.rs:635
   5: infer_definition_types(Id(6f0d7))
             at crates/ty_python_semantic/src/types/infer.rs:167
   6: infer_definition_types(Id(6f36d))
             at crates/ty_python_semantic/src/types/infer.rs:167
   7: infer_definition_types(Id(6f371))
             at crates/ty_python_semantic/src/types/infer.rs:167
   8: infer_expression_types(Id(6bb6a))
             at crates/ty_python_semantic/src/types/infer.rs:248
   9: infer_definition_types(Id(6f377))
             at crates/ty_python_semantic/src/types/infer.rs:167
  10: place_by_id(Id(6ec1a))
             at crates/ty_python_semantic/src/place.rs:688
  11: Type < 'db >::member_lookup_with_policy_(Id(6c48d))
             at crates/ty_python_semantic/src/types.rs:635
  12: infer_definition_types(Id(6f096))
             at crates/ty_python_semantic/src/types/infer.rs:167
  13: place_by_id(Id(6ec19))
             at crates/ty_python_semantic/src/place.rs:688
  14: Type < 'db >::member_lookup_with_policy_(Id(6c48c))
             at crates/ty_python_semantic/src/types.rs:635
  15: infer_definition_types(Id(6f037))
             at crates/ty_python_semantic/src/types/infer.rs:167
  16: place_by_id(Id(6ec18))
             at crates/ty_python_semantic/src/place.rs:688
  17: Type < 'db >::member_lookup_with_policy_(Id(6c48b))
             at crates/ty_python_semantic/src/types.rs:635
  18: infer_expression_types(Id(51d43))
             at crates/ty_python_semantic/src/types/infer.rs:248
  19: infer_definition_types(Id(50e74))
             at crates/ty_python_semantic/src/types/infer.rs:167
  20: infer_scope_types(Id(4e29e))
             at crates/ty_python_semantic/src/types/infer.rs:138
  21: check_file_impl(Id(1835))
             at crates/ty_project/src/lib.rs:527
```
I guess it could be related to one of those two libraries:
xmlschema
django.utils.xmlutils.SimplerXMLGenerator

Tell me if you need more data.


---

_Label `needs-mre` added by @AlexWaygood on 2025-08-26 09:50_

---

_Label `fatal` added by @AlexWaygood on 2025-08-26 09:50_

---

_Comment by @AlexWaygood on 2025-08-26 09:52_

Thanks for the report!

> Tell me if you need more data.

The most common cause of this panic is #256, but yes, we would need a [minimal, reproducible example](https://stackoverflow.com/help/minimal-reproducible-example) to confirm this.

---

_Comment by @obi-jerome on 2025-08-26 16:15_

I think this is the simplest I can go. ^_^
Here's the library : https://pypi.org/project/xmlschema/

```
import xmlschema
schema = xmlschema.XMLSchema("schema.xsd")
```

---

_Comment by @AlexWaygood on 2025-08-29 09:32_

With some help from Claude, I narrowed it down further to a dependency of `xmlschema`:

```py
from elementpath import XPath2Parser
```

---

_Comment by @MeGaGiGaGon on 2025-09-05 01:20_

I was able to manually minimize this to
```py
from collections.abc import MutableSequence
from typing import TypeVar
TK = TypeVar('TK', bound='Token[Any]')
class Token(MutableSequence[TK]):...
```
Which is just #256

There may be more crashes, this is just the first one I found with my minimization method. What I did is:
1. Clone the repo and start from `XPath2Parser`'s file at `".\elementpath\xpath2\xpath2_parser.py"`
2. Using the command `uvx ty check <file> | rg "error\[panic\]" -`, repeatedly try deleting imports until the panic goes away
3. On finding the import that causes the panic, repeat step 2 on that file

This took me on the path of
1. `".\elementpath\xpath2\xpath2_parser.py"`
2. `".\elementpath\xpath1\xpath1_parser.py"`
3. `".\elementpath\xpath_tokens.py"`
4. `".\elementpath\exceptions.py"`
5. `".\elementpath\tdop.py"`

`".\elementpath\tdop.py"` contains no non-stdlib imports, so at that point I did a similar thing where I deleted code at random until the panic went away, and kept doing that until there were no lines left to delete, giving me the minimalization.

---

_Comment by @carljm on 2025-09-05 01:26_

Thanks for the minimization! Yes, we support recursive bounds on PEP 695 typevars, but not yet on legacy typevars.

---

_Closed by @carljm on 2025-09-05 01:27_

---
