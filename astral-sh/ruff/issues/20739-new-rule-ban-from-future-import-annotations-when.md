```yaml
number: 20739
title: "new rule - ban `from __future__ import annotations` when targeting python >=3.14"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-07T08:04:13Z
updated_at: 2026-01-21T01:29:58Z
url: https://github.com/astral-sh/ruff/issues/20739
synced_at: 2026-01-21T02:01:06Z
```

# new rule - ban `from __future__ import annotations` when targeting python >=3.14

---

_@DetachHead_

### Summary

`__future__.annotations` is deprecated as of python 3.14, see https://peps.python.org/pep-0749/#abstract

would be nice if ruff had a rule (similar to the existing pyupgrade rules) to suggest removing it.

---

_Comment by @dylwil3 on 2025-10-07 14:48_

I agree with the suggestion but not the reasoning: for most use-cases the default behavior of Python 3.14 will look the same with or without `from __future__ import annotations`, but it is not the case that `from __future__ import annotations` does nothing in Python 3.14 or is deprecated.

If you follow that link it specifies that `__future__.annotations` will not be deprecated until after Python 3.13 is EOL, which is quite a ways off. Until then `from __future__ import annotations` has the same effect in Python 3.14 as it did before - to turn annotations into strings. See the discussion around https://github.com/astral-sh/ruff/issues/18502#issuecomment-3355668188 for more.

---

_Label `rule` added by @ntBre on 2025-10-07 15:14_

---

_Label `needs-decision` added by @ntBre on 2025-10-07 15:14_

---

_Comment by @DetachHead on 2025-10-08 10:00_

hmm i guess i misread it then. although i the way i see it, "is deprecated" and "will subsequently be deprecated" mean the same thing to me as long as there's a suitable replacement. if the new behavior in 3.14 is intended to replace `from __future__ import annotations` then that means it doesn't make sense to use it at all in new code (unless targeting python <=3.13), regardless of whether it's formally deprecated.

i guess to avoid confusion the error message could say something like "`from __future__ import annotations` is *unneccessary* as of python 3.14" instead of "`from __future__ import annotations` is *deprecated* as of python 3.14"

---

_Comment by @ericbn on 2026-01-21 01:29_

As @dylwil3 correctly stated:

> it is not the case that `from __future__ import annotations` does nothing in Python 3.14 or is deprecated.

I actually find it confusing that the Python 3.14 documentation gives a different idea. Here's one example of code that fails to run in Python 3.14 if you remove the `from __future__ import annotations` from it:

```python
from __future__ import annotations

from typing import TYPE_CHECKING, Any

from attr import fields, has
from cattrs.gen import make_dict_unstructure_fn, override
from cattrs.preconf.json import make_converter

if TYPE_CHECKING:
    from collections.abc import Callable

    from cattrs import Converter

json_converter = make_converter()


@json_converter.register_unstructure_hook_factory(has)
def _json_converter_unstructure_override_class(
    cl: Any, converter: Converter
) -> Callable:
    return make_dict_unstructure_fn(
        cl,
        converter,
        **{
            f.name: override(omit_if_default=True)
            for f in fields(cl)
            if f.default is None
        },
    )
```

Code using [inspect.signature](https://docs.python.org/3/library/inspect.html#inspect.signature), as is the case behind the scenes with cattrs above, might fail if the type definition is enclosed in `if TYPE_CHECKING` (and the annotation is not a string). And ruff will suggest that `if TYPE_CHECKING` should be used exactly as above.

---
