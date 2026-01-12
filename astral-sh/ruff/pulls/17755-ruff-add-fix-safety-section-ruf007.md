```yaml
number: 17755
title: "[`ruff`] add fix safety section (`RUF007`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-zip-instead-of-pairwise
created_at: 2025-05-01T06:02:43Z
updated_at: 2025-05-14T15:07:12Z
url: https://github.com/astral-sh/ruff/pull/17755
synced_at: 2026-01-12T15:56:05Z
```

# [`ruff`] add fix safety section (`RUF007`)

---

_@VascoSch92_

The PR add the `fix safety` section for rule `RUF007` (#15584 )

It seems that the fix was always marked as unsafe #14401

## Unsafety example

This first example is a little extreme. In fact, the class `Foo` overrides the  `__getitem__` method but in a very special, way. The difference lies in the fact that `zip(letters, letters[1:])` call the slice `letters[1:]` which is behaving weird in this case, while `itertools.pairwise(letters)` call just `__getitem__(0), __getitem__(1), ...` and so on.

Note that the diagnostic is emitted: [playground](https://play.ruff.rs)

I don't know if we want to mention this problem, as there is a subtile bug in the python implementation of `Foo` which make the rule unsafe.

```python
from dataclasses import dataclass
import itertools

@dataclass
class Foo:
    letters: str
    
    def __getitem__(self, index):
        return self.letters[index] + "_foo"


letters = Foo("ABCD")
zip_ = zip(letters, letters[1:])
for a, b in zip_:
    print(a, b) # A_foo B, B_foo C, C_foo D, D_foo _
    
pair = itertools.pairwise(letters)
for a, b in pair:
    print(a, b) # A_foo B_foo, B_foo C_foo, C_foo D_foo
```

This other example is much probable.
here, `itertools.pairwise` was shadowed by a costume function [(playground)](https://play.ruff.rs)

```python
from dataclasses import dataclass
from itertools import pairwise

def pairwise(a):
    return []
    
letters = "ABCD"
zip_ = zip(letters, letters[1:])
print([(a, b) for a, b in zip_]) # [('A', 'B'), ('B', 'C'), ('C', 'D')]

pair = pairwise(letters)
print(pair) # []
```

---

_Comment by @github-actions[bot] on 2025-05-01 06:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2025-05-01 17:15_

---

_Comment by @VascoSch92 on 2025-05-04 09:19_

@dylwil3 Hey ;-) I think it is ready to review

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs`:35 on 2025-05-09 20:32_

If this were the case, it would be a bug, not a reason for unsafety. We usually check if a symbol has the binding we expect, especially for symbols from the standard library.

https://github.com/astral-sh/ruff/pull/12663 was the PR actually adding the unsafe fix, the one in the description just stabilized it (moved it to stable from preview). There's no discussion of making it unsafe, so your first example from the PR summary may be the best thing to include here. I think the fix will probably remove comments too, but I haven't tested that.

(btw your playground links don't include your code, you have to click the `Share` button to get a link that preserves your input :slightly_smiling_face:)

---

_@ntBre reviewed on 2025-05-09 20:32_

---

_@VascoSch92 reviewed on 2025-05-10 16:49_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs`:35 on 2025-05-10 16:49_

Hey  @ntBre 

I re-pasted the example in the playground and it seems that the rule is triggered, but it seems that if I push the share button, the link is not copied :-(

I'm adding a picture to prove my point :-) 

<img width="736" alt="Screenshot 2025-05-10 at 18 48 08" src="https://github.com/user-attachments/assets/3c1d51e9-39a9-4732-aaf0-cf20120e0fa3" />



---

_@dylwil3 reviewed on 2025-05-11 16:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs`:35 on 2025-05-11 16:08_

@VascoSch92 if you click "quick fix" in this example https://play.ruff.rs/231094cf-8f73-4510-b3f0-f9b3d640d0b0 then you will see that Ruff imports `itertools` and rewrites using `itertools.pairwise` so there is no conflict.

---

_@dylwil3 reviewed on 2025-05-11 16:09_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs`:35 on 2025-05-11 16:09_

Moreover, if you _do_ have a conflict - like if you already did `from itertools import pairwise` and then also shadowed it, then no fix is offered, even though the lint is emitted: https://play.ruff.rs/322a73ac-d1f9-4f47-a3a0-e3b7d541465d

---

_@VascoSch92 reviewed on 2025-05-11 20:33_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/ruff/rules/zip_instead_of_pairwise.rs`:35 on 2025-05-11 20:33_

Ah ok thanks for the explanation. I could reproduce what you are saying in my playground. 

So yes... the comments are deleted (I will add in the fix safety section) and I will try to summarise the first example.

---

_Review requested from @ntBre by @VascoSch92 on 2025-05-13 20:31_

---

_Review requested from @dylwil3 by @VascoSch92 on 2025-05-13 20:31_

---

_Comment by @VascoSch92 on 2025-05-13 20:32_

@ntBre , @dylwil3 let me know what do you think :-)

---

_@ntBre approved on 2025-05-14 15:06_

Thanks! And nice catch on the slicing behavior. That is very tricky.

---

_Merged by @ntBre on 2025-05-14 15:07_

---

_Closed by @ntBre on 2025-05-14 15:07_

---
