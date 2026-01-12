```yaml
number: 19018
title: "Rule request: useless generator call"
type: issue
state: open
author: xavdid
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-28T19:05:15Z
updated_at: 2025-07-07T12:50:08Z
url: https://github.com/astral-sh/ruff/issues/19018
synced_at: 2026-01-12T15:54:56Z
```

# Rule request: useless generator call

---

_@xavdid_

### Summary

Because generators don't execute until they're consumed, I had a small bug today where I expected a function call to throw and it didn't.

```py
import pytest

def my_gen():
    yield 1
    yield 2
    
    raise ValueError()

def test_raises(): # test fails incorrectly
    with pytest.raises(ValueError):
        my_gen()
```

Though everything looks correct (and passes all the lints I could find), `test_raises` fails because the expression doesn't actually raise.

To produce working code, I'd need to consume the generator:

```diff
def test_raises(): # test fails incorrectly
    with pytest.raises(ValueError):
-       my_gen()
+       list(my_gen())
```

And then it passes as expected.

It would be great if it could warn me that the generator call won't do anything. The original code should fail the lint, but assigning it to a variable or using the result in another function (like `list`) should pass.

Thanks!

---

_Label `rule` added by @ntBre on 2025-06-28 22:58_

---

_Label `needs-decision` added by @ntBre on 2025-06-28 22:58_

---

_Comment by @ntBre on 2025-06-28 22:59_

This sounds reasonable to me, but I'm open to other thoughts.

Somewhat related: https://github.com/astral-sh/ruff/issues/9833. This rule could benefit from better type inference too, but we could potentially handle the single-file case for now.

---

_Comment by @MichaReiser on 2025-07-07 12:49_

The single file is tricky because it only works if the called functions annotate their return types. Which isn't the case for the example provided by @xavdid. I'm not sure how useful this rule is without return type inference 

---
