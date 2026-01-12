```yaml
number: 17760
title: "[`ruff`] add fix safety section (`RUF033`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-post-init-defaults
created_at: 2025-05-01T07:24:58Z
updated_at: 2025-05-11T16:15:16Z
url: https://github.com/astral-sh/ruff/pull/17760
synced_at: 2026-01-12T15:56:05Z
```

# [`ruff`] add fix safety section (`RUF033`)

---

_@VascoSch92_

The PR add the fix safety section for rule `RUF033` (https://github.com/astral-sh/ruff/issues/15584 ).

It seems that the fix was always unsafe, see #13192. An example of unsafely can be found in the PR description of #13192 or in the next section.

## Unsafety example

I took as an example the case 

```python
from dataclasses import dataclass, InitVar

@dataclass
class User:
    name: str

    def __post_init__(self, greeting: str = "Hello"):
        print(f"{greeting}, {self.name}!")

@dataclass
class UserAfterFix:
    name: str
    greeting: InitVar[str] = "Hello"

    def __post_init__(self, greeting: str):
        print(f"{greeting}, {self.name}!")

User('Alice')  # print `Hello, Alice`
UserAfterFix('Alice') # raise an error
```

---

_Comment by @github-actions[bot] on 2025-05-01 07:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2025-05-01 17:14_

---

_Comment by @VascoSch92 on 2025-05-04 09:25_

Hey @dylwil3 

I believe the PR is ready for review.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:67 on 2025-05-04 22:09_

I don't really follow this as a justification - if the user didn't want this, they wouldn't select this lint rule to begin with. Does the fix ever cause a change in runtime behavior? Is it too likely that there are false positives, or are there known false positives? That's the sort of thing we're looking for here.

---

_@dylwil3 requested changes on 2025-05-04 22:10_

I could not reproduce the exception being raised in the example in your PR summary (but maybe I've done something wrong?), nor could I see why the example fix in the original PR was unsafe. The example in the docs for the rule of a suggested change _would_ be an unsafe fix since it changes runtime behavior, but we don't actually offer an autofix in the situation where we would be overwriting a default like that. So I don't really see yet why this fix is unsafe (is it?).

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:67 on 2025-05-05 12:22_

The fix is unsafe because, although switching to `InitVar` is usually correct, it is incorrect when the parameter is not meant to be a field exposed in the public API, or when the value should be shared between all instances. The examples below use a defaulted parameter as a class-level cache; the correct fix would be to switch to `ClassVar`.

Here is an example where the fix changes runtime behavior:
```console
$ cat >ruf033_1.py <<'# EOF'
from dataclasses import dataclass, field
@dataclass
class Foo:
    name: str
    serial_number: int = field(init=False)
    def __post_init__(self, cache: dict[str, int] = {}) -> None:
        self.serial_number = cache.setdefault(self.name, len(cache))
print(Foo("A").serial_number)
print(Foo("B").serial_number)
print(Foo("A").serial_number)
try:
    print(Foo("A", cache={"A": 10}).serial_number)
except TypeError:
    pass
else:
    print("Error: `cache` should not be accessible in the API")
# EOF

$ python ruf033_1.py
0
1
0

$ ruff --isolated check ruf033_1.py --select RUF033 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf033_1.py                                                    
0
1
0
10
Error: `cache` should not be accessible in the API
```

Here is another:
```console
$ cat >ruf033_2.py <<'# EOF'
from dataclasses import dataclass, field
@dataclass
class Foo:
    name: str
    serial_number: int = field(init=False)
    def __post_init__(self, *, cache: dict[str, int] = {}) -> None:
        self.serial_number = cache.setdefault(self.name, len(cache))
print(Foo("A").serial_number)
print(Foo("B").serial_number)
print(Foo("A").serial_number)
# EOF

$ python ruf033_2.py
0
1
0

$ ruff --isolated check ruf033_2.py --select RUF033 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf033_2.py
Traceback (most recent call last):
  File "ruf033.py", line 9, in <module>
    print(Foo("A").serial_number)
          ^^^^^^^^
  File "<string>", line 4, in __init__
TypeError: Foo.__post_init__() takes 1 positional argument but 2 were given
```

---

_@dscorbett reviewed on 2025-05-05 12:22_

---

_Comment by @VascoSch92 on 2025-05-05 18:41_

> I could not reproduce the exception being raised in the example in your PR summary (but maybe I've done something wrong?), nor could I see why the example fix in the original PR was unsafe. The example in the docs for the rule of a suggested change _would_ be an unsafe fix since it changes runtime behavior, but we don't actually offer an autofix in the situation where we would be overwriting a default like that. So I don't really see yet why this fix is unsafe (is it?).

Ok, strange... It works on ruff playground, i.e., the rule is triggered.

<img width="547" alt="Screenshot 2025-05-05 at 20 38 53" src="https://github.com/user-attachments/assets/be4c9447-ab0d-4ee0-a72a-e71be000ecb8" />


I had in mind the same reason of @dscorbett but I could not explain it so clear ;-) 

I will change the section accordingly. 

---

_Review requested from @dylwil3 by @VascoSch92 on 2025-05-10 16:53_

---

_Comment by @dylwil3 on 2025-05-11 16:14_

> Ok, strange... It works on ruff playground, i.e., the rule is triggered.

I agree that the rule is triggered, but when you actually run the code it does not raise an exception (both before and after the fix).

---

_@dylwil3 approved on 2025-05-11 16:14_

Thank you! (and thanks @dscorbett !)

---

_Merged by @dylwil3 on 2025-05-11 16:15_

---

_Closed by @dylwil3 on 2025-05-11 16:15_

---
