```yaml
number: 1539
title: Add flake8-simplify SIM300 check for Yoda Conditions
type: pull_request
state: merged
author: PedramNavid
labels: []
assignees: []
merged: true
base: main
head: SIM300
created_at: 2023-01-01T22:15:17Z
updated_at: 2023-01-05T07:40:03Z
url: https://github.com/astral-sh/ruff/pull/1539
synced_at: 2026-01-12T05:36:31Z
```

# Add flake8-simplify SIM300 check for Yoda Conditions

---

_Pull request opened by @PedramNavid on 2023-01-01 22:15_

Adds a SIM300 Check for Yoda Conditions

```
resources/test/fixtures/flake8_simplify/SIM300.py:3:1: SIM300 Use `'yoda' == compare` instead of `compare == 'yoda' (Yoda-conditions)`
resources/test/fixtures/flake8_simplify/SIM300.py:4:1: SIM300 Use `42 == age` instead of `age == 42 (Yoda-conditions)`
Found 2 error(s).
```

Refs: https://github.com/charliermarsh/ruff/issues/998

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM300.py`:4 on 2023-01-01 22:22_

One condition that `flake8-simplify` seems to get wrong:

```py
"yoda" == "yoda"
```

`flake8-simplify` gives me:

```
resources/test/fixtures/flake8_simplify/SIM300.py:10:1: SIM300 Use '"yoda" == "yoda"' instead of '"yoda" == "yoda"' (Yoda-conditions)
```

But we should probably avoid raising this in the event that both sides are constant?

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM300.py`:3 on 2023-01-01 22:23_

Couple other good test cases:

- `x == y and 'yoda' == compare` (should raise)
- `'yoda' == compare == 1` (shouldn't raise)

---

_Review comment by @charliermarsh on `src/flake8_simplify/plugins/yoda_conditions.rs`:8 on 2023-01-01 22:23_

Mind just adding `/// SIM300` above this function? Helps a lot with grepping for rule implementations, since otherwise there's nothing tying this file to the check code.

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:73 on 2023-01-01 22:24_

I think this line might be repeated between here and the previous paragraph.

---

_@charliermarsh reviewed on 2023-01-01 22:24_

This looks great! Thank you for putting it together!

Couple small things -- only behavior change would be catching the constant-constant case, if you agree.

---

_@PedramNavid reviewed on 2023-01-01 22:50_

---

_Review comment by @PedramNavid on `resources/test/fixtures/flake8_simplify/SIM300.py`:4 on 2023-01-01 22:50_

Oh good catch, I'll add another check. Might be worth creating an alert at some point when comparing constants with each other too. Can't think of any good reason to do it on purpose. 

---

_@charliermarsh reviewed on 2023-01-01 23:00_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_simplify/SIM300.py`:4 on 2023-01-01 23:00_

Haha yeah the absurdity of that didn't even occur to me... I have my correctness hat on.

---

_Merged by @charliermarsh on 2023-01-01 23:37_

---

_Closed by @charliermarsh on 2023-01-01 23:37_

---

_Comment by @charliermarsh on 2023-01-01 23:37_

Awesome, thank you!

---

_Comment by @charliermarsh on 2023-01-01 23:38_

If you were interested in enabling autofix for this rule, you could take a look at the `compare` function in `src/pycodestyle/plugins.rs`, which demonstrates the use of `SourceCodeGenerator` to take a modified `AST` and generate Python source code from it.

---

_Comment by @PedramNavid on 2023-01-02 00:19_

> If you were interested in enabling autofix for this rule, you could take a look at the `compare` function in `src/pycodestyle/plugins.rs`, which demonstrates the use of `SourceCodeGenerator` to take a modified `AST` and generate Python source code from it.

great next step. thanks for the hint i'll have a look!

---

_Branch deleted on 2023-01-02 00:19_

---

_Comment by @BenediktAllendorf on 2023-01-04 16:49_

I was just writing an issue because I thought this check wanted me to use Yoda conditions, and thus it all didn't make sense to me. But now I think this is more of a communication thing, and I hope it's okay to address that here.

First, I'm not sure if the description `Use left == right instead of right == left (Yoda-conditions)` is the best way to clarify what your code is generally supposed to look like according to this check.

Second, ``Use `42 == age` instead of `age == 42 (Yoda-conditions)` `` (besides having the last tick at the wrong location) sounds like I should use that format (i.e., I should use Yoda-conditions), but I think the message is supposed to mean "you are wrongly doing X instead of correctly doing Y"?

Maybe the message could be more along the line of:
``Usage of Yoda-conditions is discouraged; use `age == 42` instead``?

---

_Comment by @charliermarsh on 2023-01-04 17:08_

Yeah this message was taken directly from `flake8-simplify` for simplicity (sorry for the pun). I'll tweak it a bit, I agree that it's suboptimal.

---

_Comment by @charliermarsh on 2023-01-04 21:05_

(Fixed.)

---

_Comment by @BenediktAllendorf on 2023-01-05 07:40_

Thank you for your quick response! I'm very impressed by this tool and am looking forward to integrating it into our project(s) more and more!

---
