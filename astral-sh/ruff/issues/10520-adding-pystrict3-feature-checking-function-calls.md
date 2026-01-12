```yaml
number: 10520
title: "adding pystrict3 feature: checking function calls"
type: issue
state: closed
author: JohannesBuchner
labels:
  - type-inference
assignees: []
created_at: 2024-03-22T08:52:01Z
updated_at: 2024-10-24T14:34:13Z
url: https://github.com/astral-sh/ruff/issues/10520
synced_at: 2026-01-12T15:54:50Z
```

# adding pystrict3 feature: checking function calls

---

_@JohannesBuchner_

Hi,

I was wondering whether you may be interested in copying into your project some features from the static code analyzer [pystrict3](https://github.com/JohannesBuchner/pystrict3).

In particular, I want to highlight here the feature for **checking function calls**:

A simple example of what it can do:
```
def foo(a, b):
    return a*b
foo(1, 2)        ## OK
foo(123)         ## error: wrong number of arguments

def bar(a, b=1):
    return a*b
bar(1)           ## OK
bar(1, 2)        ## OK
bar(1, 2, 3)     ## error: wrong number of arguments
```

That is perhaps not too advanced, but then it can also check built-ins and imported function calls:

```
# builtin module signatures are verified too:
import os, numpy
os.mkdir("foo", "bar") ## error: wrong number of arguments (if run with --load-builtin-modules)
numpy.exp()            ## error: wrong number of arguments (if run with --load-any-modules)
```

This is achieved with syntax tree analysis for the function calls, combined with importing the builtin libraries and inspecting their call signatures.

It works very well.

pystrict3 is designed to not give false positives.

---

_Label `multifile-analysis` added by @MichaReiser on 2024-03-22 16:37_

---

_Comment by @MichaReiser on 2024-03-25 14:25_

Thanks for reporting this. We're considering this kind of functionality around our work to add support for multifile analysis and potentially type checking to Ruff. Until then, implementing such a rule would be very limited in its functionality because it would only work for functions defined in the same file. I'll close this and track it as part of our mutifile analysis work. 

Thanks again for reporting this feature request.

This is similar to https://github.com/astral-sh/ruff/issues/10234 



---

_Closed by @MichaReiser on 2024-03-25 14:25_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
