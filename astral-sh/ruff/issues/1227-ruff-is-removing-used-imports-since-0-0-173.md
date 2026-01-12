```yaml
number: 1227
title: Ruff is removing used imports since 0.0.173 version 
type: issue
state: closed
author: Czaki
labels:
  - bug
assignees: []
created_at: 2022-12-13T09:01:15Z
updated_at: 2022-12-27T16:16:35Z
url: https://github.com/astral-sh/ruff/issues/1227
synced_at: 2026-01-12T15:54:41Z
```

# Ruff is removing used imports since 0.0.173 version 

---

_@Czaki_

Since version 0.0.173 (up to 0.0.178 - newest when create this issue) the ruff is removing used imports

I have two examples:

https://github.com/4DNucleome/PartSeg/blob/fadc4f8ad9b552386de21bbc2a1ab3e0159f52e7/package/PartSegCore/analysis/io_utils.py#L10-L24 

is converted to:

![obraz](https://user-images.githubusercontent.com/3826210/207272087-d57797bc-d27f-4de2-9029-13b4187ec7ea.png)

and remove the type used in type annotation:
https://github.com/4DNucleome/PartSeg/blob/fadc4f8ad9b552386de21bbc2a1ab3e0159f52e7/package/PartSegCore/mask/io_functions.py#L725

The `ProjectInfoBase` is a Protocol if this information is important. In the second case, such import could be hidden under `if typing.TYPE_CHECKING:` but I use in code other things from this module. 

And even if it should be moved under the type checking clause, the autofix should not just remove it. 


---

_Label `bug` added by @charliermarsh on 2022-12-13 14:26_

---

_Comment by @charliermarsh on 2022-12-13 14:26_

Thanks! Will fix. Sorry about that.

---

_Comment by @charliermarsh on 2022-12-13 14:38_

In both cases the cause is the same, which is that the import appears to be redefined-while-unused (hence the NoQA), so we then mark the import as unused.

I could suggest a variety of workarounds… e.g., you could rebind it to a variable of the same name (“Foo = Foo” before the if-statement), or add a NoQA for that case. But, of course, we shouldn’t be removing those.

I may just have to disable this behavior (of marking redefined-while-unused imports as unused) until we have more extensive branch analysis.

---

_Comment by @charliermarsh on 2022-12-13 14:40_

(I probably won’t be able to resolve this today just given my schedule but hopefully a NoQA will be ok for you for now?)

---

_Comment by @Czaki on 2022-12-14 21:09_

Do you mean add noqua to the import line? It may be ok. 

name conditionally redefined should not be removed. 



---

_Comment by @charliermarsh on 2022-12-14 21:10_

Yeah, I mean adding a `noqa` to the import line, to avoid marking it as unused.

---

_Comment by @Czaki on 2022-12-14 23:47_

I'm ok with this solution 

---

_Comment by @Czaki on 2022-12-15 13:18_

I try to move problematic imports under if clause and i could create commit that pass `pre-commit` but when run pre-commit on all files then ruff creates multiple warnings:

```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`
warning: `U` has been remapped to `UP`

```

https://results.pre-commit.ci/run/github/166421141/1671109943.V-T5M9gkRhOblSlqIfxcUA

That hides real error (in this case E501) because the output is truncated.  

---

_Comment by @charliermarsh on 2022-12-19 21:41_

@Czaki - Can you try upgrading? I made some improvements to how `noqa` are handled in those cases _and_ modified the warnings to only omit once per code (you can just change `U` to `UP` in your `pyproject.toml` though hopefully to suppress them entirely).

---

_Comment by @Czaki on 2022-12-19 22:21_

I see that pre-commit hook is in version 186 and this repository is in 188. Did I correctly understand that I need to use at least 188 to test? 

---

_Comment by @charliermarsh on 2022-12-19 22:48_

188 is now up. (There’s a slight delay between the two repos.)

---

_Comment by @Czaki on 2022-12-19 22:54_

What is the expected change in behavior? `noqa` in both lines preventing removal? 

---

_Comment by @charliermarsh on 2022-12-20 00:53_

Yeah, if you apply this diff, Ruff won't touch that member:

```diff
diff --git a/foo.py b/foo.py
index d72d8d6e..d5d7a367 100644
--- a/foo.py
+++ b/foo.py
@@ -7,7 +7,11 @@ import numpy as np
 import packaging.version
 
 from PartSegCore.mask_create import MaskProperty
-from PartSegCore.project_info import AdditionalLayerDescription, HistoryElement, ProjectInfoBase
+from PartSegCore.project_info import (
+    AdditionalLayerDescription,
+    HistoryElement,
+    ProjectInfoBase,  # noqa: F401
+)
 from PartSegCore.roi_info import ROIInfo
 from PartSegCore.utils import numpy_repr
 from PartSegImage import Image
```

---

_Comment by @charliermarsh on 2022-12-26 01:15_

Closing for now, but let me know if you continue to run into this issue.

---

_Closed by @charliermarsh on 2022-12-26 01:15_

---

_Comment by @Czaki on 2022-12-27 16:11_

Thanks :heart: 

---

_Comment by @charliermarsh on 2022-12-27 16:16_

You're welcome :)

---
