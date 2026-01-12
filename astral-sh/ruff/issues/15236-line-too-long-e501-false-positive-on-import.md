```yaml
number: 15236
title: "`line-too-long` (`E501`) - false positive on import statements from long module names where it's impossible to fix"
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2025-01-03T06:08:05Z
updated_at: 2025-01-03T18:36:47Z
url: https://github.com/astral-sh/ruff/issues/15236
synced_at: 2026-01-12T15:54:54Z
```

# `line-too-long` (`E501`) - false positive on import statements from long module names where it's impossible to fix

---

_@DetachHead_

if a line contains an import statement from a very long module path, it triggers `E501`, even though there's no way to fix it as far as i know:

```py
from asdfasasd.asdfasdf.asdfasdfsa.dfasdfasdfasdf.asdfasdfasdf.asdfasdfasdf.asdfasdfasdf.asdfsasdfasdf import ( # error: E501
    foo,
)
```

https://play.ruff.rs/93d32aef-cefb-41f2-8205-e05bbea8ba28?secondary=Format

ruff should be able to determine that this situation is unavoidable and therefore should not report the error. it already does this in some cases for example comments/docstrings with long URLs

---

_Comment by @MichaReiser on 2025-01-03 08:17_

You can use line continuation if you really wanted to avoid the E501 violation:

```python
from asdfasasd.asdfasdf.asdfasdfsa.dfasdfasdfasdf\
    .asdfasdfasdf.asdfasdfasdf.asdfasdfasdf\
    .asdfsasdfasdf import foo
```

But that's not something we want to encourage (especially because the formatter removes the line continuations again).

That's why I think it's correct to say that you can't change anything on the import side. However, you can change the module structure to reduce the nesting, which will make it easier to import the module. 

I did some quick search and long parenthesized imports don't seem that common ([source](https://sourcegraph.com/search?q=context%3Aglobal+lang%3APython+from%5Cs%2B%5Ba-zA-Z%5C.%5D%2B%5Cs%2Bimport%5Cs%2B%5C%28%5Cs%2B%23%5Cs%2Bnoqa%3A%5Cs%2BE501%5Cn%5Ba-zA-Z%2C%5Cs%5D%2B%5C%29&patternType=regexp&sm=0)) or my search is too strict. Suppressions for single-line imports is more common ([source](https://sourcegraph.com/search?q=context%3Aglobal+lang%3APython+from%5Cs%2B%5Ba-zA-Z%5C.%5D%2B%5Cs%2Bimport%5Cs%2B%5Ba-zA-Z%2C%5D%2B%5Cs%2B%23%5Cs%2Bnoqa%3A%5Cs%2BE501&patternType=regexp&sm=0)).

There are many more cases where Python doesn't provide a way for splitting an expression unlike other languages. 

```
class AVeryLongClassNameAndTheresNothingYouCanDoAboutIt: # E501
   pass

a = (1000000000_00) # noqa: E501
```

but the import case is probably the most common.

Ultimately this is somewhat related to https://github.com/astral-sh/ruff/issues/8383 and raises the question if we should make E501 token aware. I think we could do so now, because we have cheap access to the token stream. 


---

_Label `rule` added by @MichaReiser on 2025-01-03 08:17_

---

_Comment by @InSyncWithFoo on 2025-01-03 18:36_

@MichaReiser Your query is indeed too strict. [A slightly looser one](https://sourcegraph.com/search?q=context:global+lang:Python+from%5Cs%2B%5Ba-zA-Z0-9_%5C.%5D%2B%5Cs%2Bimport%5Cs*%5C%28%5Cs*%23%5Cs*noqa:%5Cs*E501%5Cn%5Ba-zA-Z0-9_%2C%5Cs%5D%2B%5C%29&patternType=regexp&sm=0) returns ~800 results.

```text
from\s+[a-zA-Z    \.]+\s+import\s+\(\s+#\s+noqa:\s+E501\n[a-zA-Z    ,\s]+\)

from\s+[a-zA-Z0-9_\.]+\s+import\s*\(\s*#\s*noqa:\s*E501\n[a-zA-Z0-9_,\s]+\)
#             ^^^^               ^    ^   ^       ^             ^^^^
```

[Same for the other query](https://sourcegraph.com/search?q=context:global+lang:Python+from%5Cs%2B%5Ba-zA-Z0-9_%5C.%5D%2B%5Cs%2Bimport%5Cs%2B%5Ba-zA-Z0-9_%2C%5D%2B%5Cs*%23%5Cs*noqa:%5Cs*E501&patternType=regexp&sm=0).

---
