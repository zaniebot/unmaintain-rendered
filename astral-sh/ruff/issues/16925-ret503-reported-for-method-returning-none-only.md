```yaml
number: 16925
title: RET503 reported for method returning None only
type: issue
state: closed
author: stefan6419846
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-03-23T12:26:24Z
updated_at: 2025-06-13T06:57:02Z
url: https://github.com/astral-sh/ruff/issues/16925
synced_at: 2026-01-12T15:54:55Z
```

# RET503 reported for method returning None only

---

_@stefan6419846_

### Summary

Running *ruff* on the whole file reports the following method as an issue: https://github.com/py-pdf/pypdf/blob/f805e6d7c29fc31c6c335652b45c2f8478eae23c/pypdf/_page.py#L1134-L1244

```
pypdf/_page.py:1244:9: RET503 Missing explicit `return` at the end of function able to return non-`None` value
     |
1242 |         self.replace_contents(ContentStream(new_content_array, self.pdf))
1243 |         self[NameObject(PG.RESOURCES)] = new_resources
1244 |         self[NameObject(PG.ANNOTS)] = new_annots
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ RET503
1245 |
1246 |     def _merge_page_writer(
     |
     = help: Add explicit `return` statement

```

This does not really make sense, as the corresponding method is type annotated and states it only returns `None`. 

### Version

ruff 0.11.2

---

_Comment by @FabiPi3 on 2025-03-23 20:28_

On line 1150 you are returning something which could be not-none. Split this line into the function-call and afterward leave a blank `return`

---

_Comment by @stefan6419846 on 2025-03-24 07:31_

This is still confusing, as the type hint for the return value on line 1141 explicitly states `None`, thus typing does not allow me to return anything different from `None` in line 1150 as well (which is ensured using *mypy*).

---

_Label `question` added by @ntBre on 2025-03-24 14:21_

---

_Label `type-inference` added by @MichaReiser on 2025-03-24 15:51_

---

_Comment by @MichaReiser on 2025-03-24 15:52_

I can see how this is confusing. The problem is that Ruff doesn't understand that the called function returns `None`. The easiest solution for now (which some may also find more readable) is to change your code to call the function, followed by an explicit empty return

```py
            self._merge_page_writer(
                page2, page2transformation, ctm, over, expand
            )
            return
```

---

_Comment by @stefan6419846 on 2025-03-24 16:22_

Thanks for the explanation. I am going to leave to it up to you whether this can be closed.

---

_Label `question` removed by @MichaReiser on 2025-03-27 16:16_

---

_Label `rule` added by @MichaReiser on 2025-03-27 16:16_

---

_Closed by @MichaReiser on 2025-06-13 06:57_

---
