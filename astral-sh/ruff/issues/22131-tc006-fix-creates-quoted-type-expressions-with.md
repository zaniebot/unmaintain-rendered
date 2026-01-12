```yaml
number: 22131
title: TC006 fix creates quoted type expressions with escape sequences
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-12-21T20:44:55Z
updated_at: 2025-12-22T18:46:22Z
url: https://github.com/astral-sh/ruff/issues/22131
synced_at: 2026-01-12T15:54:58Z
```

# TC006 fix creates quoted type expressions with escape sequences

---

_@dscorbett_

### Summary

The fix for [`runtime-cast-value` (TC006)](https://docs.astral.sh/ruff/rules/runtime-cast-value/) can introduce an escape sequence into a quoted type expression, which ty rejects. The typing spec is not clear whether this is allowed but I’d expect Ruff to agree with ty. Example ([Ruff](https://play.ruff.rs/4107d241-3517-46ad-b20d-1b22b7614c89), [ty](https://play.ty.dev/c953c3d8-2132-4ac3-82e9-1a21a6aa3f0c)):
```console
$ cat >tc006_fp.py <<'# EOF'
from typing import Literal, cast
cast(Literal["'"], "'")
# EOF

$ ruff --isolated check tc006_fp.py --select TC006 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat tc006_fp.py
from typing import Literal, cast
cast("Literal[\"'\"]", "'")

$ ty check --output-format concise tc006_fp.py
tc006_fp.py:2:6: error[escape-character-in-forward-annotation] Type expressions cannot contain escape characters
Found 1 diagnostic
```

Note that the problem is only with escape sequences in the fix’s _output_. Escape sequences are fine in the fix’s _input_, as long as the fix resolves them. Example ([Ruff](https://play.ruff.rs/ec95404a-3d18-4e51-a8ea-29ec4da4153c), [ty](https://play.ty.dev/a63403b5-58d1-4cb8-833d-eba22e712500)):
```console
$ cat >tc006_tp.py <<'# EOF'
from typing import Literal, cast
cast(Literal["\x3F"] | None, "?")
# EOF

$ ruff --isolated check tc006_tp.py --select TC006 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat tc006_tp.py
from typing import Literal, cast
cast("Literal['?'] | None", "?")

$ ty check --output-format concise tc006_tp.py
All checks passed!
```

### Version

ruff 0.14.10 (45bbb4cbf 2025-12-18)

---

_Comment by @AlexWaygood on 2025-12-21 21:01_

https://discuss.python.org/t/exotic-kinds-of-string-annotations/50527 is some relevant context here, but I can't remember exactly what the resolution of that was or how much of the original proposal ever made it into the spec in the end (@JelleZijlstra might remember more?)

---

_Label `rule` added by @ntBre on 2025-12-22 14:18_

---

_Label `needs-decision` added by @ntBre on 2025-12-22 14:18_

---

_Comment by @JelleZijlstra on 2025-12-22 18:38_

Yes, I think we never resolved the issue there. For Ruff, I'd recommend writing out the string in such a way to avoid escape sequences, like `cast("""Literal["'"]""", ...)`. Another option could be to make Ruff's fix unsafe if there are escape sequences involved. The linked thread discusses some other edge cases that might cause trouble.

---

_Comment by @AlexWaygood on 2025-12-22 18:41_

Yeah, I think whatever the case with the spec, it's definitely unfortunate for one of Astral's tools to autofix something in a way that causes another of Astral's tools to start complaining

---

_Comment by @ntBre on 2025-12-22 18:46_

Glancing quickly at the rule's implementation, this might be tricky to fix, but it definitely sounds like this is a bug for the fix output to disagree with ty. I think we should try harder not to escape nested quotes and then avoid a fix or at least make it unsafe if we can't avoid it.

Thank you all!

---

_Label `rule` removed by @ntBre on 2025-12-22 18:46_

---

_Label `needs-decision` removed by @ntBre on 2025-12-22 18:46_

---

_Label `bug` added by @ntBre on 2025-12-22 18:46_

---
