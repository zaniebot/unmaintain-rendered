```yaml
number: 13545
title: collapsible-else-if (PLR5501) --fix removes comments
type: issue
state: closed
author: sylvestre
labels:
  - fixes
assignees: []
created_at: 2024-09-28T09:59:23Z
updated_at: 2024-10-03T15:30:58Z
url: https://github.com/astral-sh/ruff/issues/13545
synced_at: 2026-01-12T15:54:53Z
```

# collapsible-else-if (PLR5501) --fix removes comments

---

_@sylvestre_

Trying to reformat firefox code with ruff https://phabricator.services.mozilla.com/D223851#inline-1243679
we noticed a bug with collapsible-else-if - https://docs.astral.sh/ruff/rules/collapsible-else-if/#collapsible-else-if-plr5501

take this case:
```
def check_sign(value: int) -> None:
    if value > 0:
        print("Number is positive.")
    else:
	# my comment to explain this
	# but will be removed
        if value < 0:
            print("Number is negative.")
        else:
            print("Number is zero.")
```

Then, with ruff main (ec72e675d9325d404864c0848e969f59a89090a7)
```
cargo run -p ruff -- check --select PLR5501 foo.py --no-cache  --fix  
Found 1 error (1 fixed, 0 remaining).
```
will update the code to:
```
def check_sign(value: int) -> None:
    if value > 0:
        print("Number is positive.")
    elif value < 0:
        print("Number is negative.")
    else:
        print("Number is zero.")
```

which removed my comment

---

_Comment by @sylvestre on 2024-09-28 10:01_

cc @diceroll123 as it seems that you are the original author 


---

_Comment by @MichaReiser on 2024-09-28 12:01_

Related to https://github.com/astral-sh/ruff/issues/9790

---

_Comment by @MichaReiser on 2024-09-28 12:02_

@sylvestre where would you expect the comment to be placed after the fix?

---

_Label `fixes` added by @MichaReiser on 2024-09-28 12:02_

---

_Comment by @sylvestre on 2024-09-28 12:29_

Good point.
To keep it relevant, I would continue on the line #elif and move on the next line 

---

_Comment by @zanieb on 2024-09-30 15:50_

We have test coverage for this behavior already at https://github.com/astral-sh/ruff/blob/0f674d1d903d0fa0da08eed7c2ed7250c362b805/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR5501_collapsible_else_if.py.snap#L73-L84

---

_Closed by @zanieb on 2024-10-03 15:22_

---

_Comment by @sylvestre on 2024-10-03 15:30_

Thanks!


---
