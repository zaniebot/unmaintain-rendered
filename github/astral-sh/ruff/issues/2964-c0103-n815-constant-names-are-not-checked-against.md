---
number: 2964
title: "C0103/N815: Constant names are not checked against UPPER_CASE naming convention"
type: issue
state: open
author: Fall1ngStar
labels:
  - rule
assignees: []
created_at: 2023-02-16T18:19:12Z
updated_at: 2025-04-25T15:48:23Z
url: https://github.com/astral-sh/ruff/issues/2964
synced_at: 2026-01-07T13:12:14-06:00
---

# C0103/N815: Constant names are not checked against UPPER_CASE naming convention

---

_Issue opened by @Fall1ngStar on 2023-02-16 18:19_

Hi ruff team,

It looks like ruff doesn't check the name of constants like pylint does, despite `invalid-name` / `C0103` (`N815`) being marked as done in the [pylint tracking issue](https://github.com/charliermarsh/ruff/issues/970).

PEP 8 recommends UPPER_CASE for constant names ([ref](https://peps.python.org/pep-0008/#constants)).

### Example

```py
# file.py
from typing import NamedTuple

Other = NamedTuple("Other", "a,b,c")

not_correct = "tata"
CORRECT = ""
ALSO_CORRECT = ""

def foo():
    print(not_correct)
```
```sh
$ pylint file.py --disable=all --enable=C0103
************* Module file
file.py:6:0: C0103: Constant name "not_correct" doesn't conform to UPPER_CASE naming style (invalid-name)

------------------------------------------------------------------
Your code has been rated at 8.57/10 (previous run: 8.57/10, +0.00)

$ ruff file.py --verbose --no-cache
[2023-02-16][12:57:12][ruff::commands::run][DEBUG] Identified files to lint in: 226.791µs
[2023-02-16][12:57:12][ruff::commands::run][DEBUG] Checked 1 files in: 7.339083ms

$ ruff --version
ruff 0.0.247
```

### Solutions

pep8-naming N816 (mixedCase variable in global scope) already implements a weaker version of this, so I was able to adapt the code to only allow UPPER_CASE variable names in global scope, but the two rules feel a bit redundant together as some constant named `thisIs_incorrect` would trigger both N816 (because it is a mixed case naming) and C0103 (it does not follow UPPER_CASE convention for constants).

I guess there are 2 paths forward to solve this issue:
- Implement C0103 as a separate rule, but accept that some variable naming may trigger 2 separate diagnostics
- Modify N816 to only allow UPPER_CASE names, but this would differ from the original `pep8-naming` package

In any case, I would be glad to submit a PR to help get this fixed !

_PS: Thanks for the awesome work you're all doing !_






---

_Comment by @charliermarsh on 2023-07-04 22:41_

I'm not sure how I want to proceed, but I found this old issue on `pep8-naming`: https://github.com/PyCQA/pep8-naming/issues/49.

I suppose I'd be okay with implementing C0103 as a separate rule.

---

_Comment by @Fall1ngStar on 2023-07-05 00:33_

I will work on that if that's ok !

---

_Comment by @Fall1ngStar on 2023-07-08 15:12_

👋 Looking for a bit of guidance here, should I implement the full [`invalid-name`](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/invalid-name.html) from pylint ? With the checks for every type of names ? There seems to be a lot of overlap with the rules from pep8-naming so I just want to make sure that the scope for this issue is correct before I work on it 😄 

---

_Comment by @woodconn on 2023-08-01 21:36_

While there are already some N801, N802, etc. rules, it would indeed be ideal to implement the full functionality of this into it's own pylint category of rules.  I'd prefer you just start with rules for the CONSTANT variable.

Ex. for "SolidNumber"
enforce-const-naming-style-UPPER (SOLIDNUMBER)
enforce-const-naming-style-snake-case  (solid_number)
enforce-const-naming-CapWords (SolidNumber)
enforce-const-naming-lowercase (solidnumber)

And ideally we could use both enforce-const-naming-style-UPPER and enforce-const-naming-style-snake-case:
Meaning the constant variable would be named SOLID_NUMBER

---

_Comment by @RmStorm on 2025-03-14 14:15_

What's the current status on this? I would also really like a rule to enforce upper case constants in the global scope! I think the problem @Fall1ngStar ran into is that [C0103](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/invalid-name.html) has a lot of overlap with the [pep8-naming (N)](https://docs.astral.sh/ruff/rules/#pep8-naming-n) rules..

Actually implementing C0103 seems to come down to implementing this [list of rules from pylint](https://github.com/pylint-dev/pylint/blob/512c8bee6cd237a8e56ab5527ee2876d8070c0d6/pylint/checkers/base/name_checker/naming_style.py#L121C1-L133C2), where every single name in python has a precisely acceptable format. Seems close to incompatible with the (N) rules but maybe they actually are compatible?

Would a PR implementing C0103 in full be welcome even though it's a rule with a rather large scope?
Or is a smaller rule only implementing upper case checking on global constant be more desirable?

---

_Label `rule` added by @MichaReiser on 2025-03-14 14:17_

---

_Comment by @ansiblejunky on 2025-03-26 15:07_

+1 I'd also like to see this rule implemented in order to enforce the pylint C0103 rule. 

---

_Comment by @jack-mcivor on 2025-04-25 15:48_

> Or is a smaller rule only implementing upper case checking on global constant be more desirable?

I think a partial implementation is still really helpful

From the [C0103 docs](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/invalid-name.html) and [styles](https://github.com/pylint-dev/pylint/blob/512c8bee6cd237a8e56ab5527ee2876d8070c0d6/pylint/checkers/base/name_checker/naming_style.py#L121C1-L133C2), there are two cases that require UPPER_CASE:
1. const: Module-level constants: any name defined at module level that is not bound to a class object nor reassigned.
2. class-const: Enum constants and class variables annotated with Final

Is it easier and less contentious to implement the check just on class-const names? Or even just on enum constants?


---

_Referenced in [astral-sh/ruff#19893](../../astral-sh/ruff/issues/19893.md) on 2025-08-13 12:59_

---

_Referenced in [containers/podman-py#567](../../containers/podman-py/pulls/567.md) on 2025-08-13 14:43_

---
