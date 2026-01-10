```yaml
number: 17759
title: "[`ruff`] add fix safety section (`RUF013`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-check-implicit-optionals
created_at: 2025-05-01T07:14:09Z
updated_at: 2025-05-05T18:47:56Z
url: https://github.com/astral-sh/ruff/pull/17759
synced_at: 2026-01-10T18:57:03Z
```

# [`ruff`] add fix safety section (`RUF013`)

---

_Pull request opened by @VascoSch92 on 2025-05-01 07:14_

The PR add the fix safety section for rule `RUF013` (https://github.com/astral-sh/ruff/issues/15584 )
The fix was introduced here #4831

The rule as a lot of False Negative (as it is explained in the docs of the rule).

The main reason because the fix is unsafe is that it could change code generation tools behaviour, as in the example here:

```python
def generate_api_docs(func):
    hints = get_type_hints(func)
    for param, hint in hints.items():
        if is_optional_type(hint):
            print(f"Parameter '{param}' is optional")
        else:
            print(f"Parameter '{param}' is required")

# Before fix
def create_user(name: str, roles: list[str] = None):
    pass

# After fix
def create_user(name: str, roles: Optional[list[str]] = None):
    pass

# Generated docs would change from "roles is required" to "roles is optional"
```



---

_Comment by @github-actions[bot] on 2025-05-01 07:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2025-05-01 17:14_

---

_Comment by @VascoSch92 on 2025-05-04 19:11_

Hey @dylwil3 

it was hard to find an example were the fix was unsafe, or to understand the reasons why is unsafe. But I think the main reasons is the one explained above.

---

_Comment by @dylwil3 on 2025-05-04 22:37_

I think this fix might just be safe. This is a creative reason it might not be safe:

> The main reason because the fix is unsafe is that it could change code generation tools behaviour, as in the example here

but I think that example looks like a bug fix or desired change to me - the argument really _is_ optional.

My guess is that - as we've seen in other instances for this documentation issue - the fix was _originally_ marked as unsafe because there were some false positives, but then during code review the implementation was changed to either skip or correctly handle those cases.

I've made a separate PR to make the fix safe in preview https://github.com/astral-sh/ruff/pull/17836 . Assuming I'm not missing something and that gets merged I'll close this PR.

Thank you for the investigative work! Really it's helpful either way if we end up documenting the fix or learning that it has a safe fix.

---

_Comment by @dscorbett on 2025-05-05 00:19_

RUF013 can change runtime behavior:
```console
$ cat >ruf013.py <<'# EOF'
def foo(arg: int = None):
    pass
print(foo.__annotations__["arg"])
# EOF

$ python ruf013.py
<class 'int'>

$ ruff --isolated check ruf013.py --select RUF013 --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ python ruf013.py
typing.Optional[int]
```

It is conceivable that some code might inspect annotations at runtime, expecting normal `type`s, and break on typing-specific things. In such a case, it’s still a true positive for RUF013, but the proper fix would be more involved than this autofix.

---

_Comment by @dylwil3 on 2025-05-05 10:28_

@VascoSch92 - could you revise the fix safety section to reflect the comments given by Micha here https://github.com/astral-sh/ruff/pull/17836#issuecomment-2849979215 and @dscorbett above?

---

_@dylwil3 approved on 2025-05-05 18:47_

Thank you, great way to summarize the issues brought up!

---

_Merged by @dylwil3 on 2025-05-05 18:47_

---

_Closed by @dylwil3 on 2025-05-05 18:47_

---
