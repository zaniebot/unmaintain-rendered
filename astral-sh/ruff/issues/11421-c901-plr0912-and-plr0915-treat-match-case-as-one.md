---
number: 11421
title: C901, PLR0912 and PLR0915 treat match/case as one statement
type: issue
state: closed
author: jaap3
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-05-14T09:21:47Z
updated_at: 2024-10-17T07:14:35Z
url: https://github.com/astral-sh/ruff/issues/11421
synced_at: 2026-01-10T01:22:51Z
---

# C901, PLR0912 and PLR0915 treat match/case as one statement

---

_Issue opened by @jaap3 on 2024-05-14 09:21_

Using ruff 0.4.4 the following code triggers `C901`, `PLR0912` and `PLR0915`:

```python
def grades_to_average(grades):
    numbers = []
    for grade in grades:
        if grade in {"F-", "F", "F+", "E-", "E"}:
            numbers.append(0)
        elif grade == "E+":
            numbers.append(.3)
        elif grade == "D-":
            numbers.append(.7)
        elif grade == "D":
            numbers.append(1.0)
        elif grade == "D+":
            numbers.append(1.3)
        elif grade == "C-":
            numbers.append(1.7)
        elif grade == "C":
            numbers.append(2.0)
        elif grade == "C+":
            numbers.append(2.3)
        elif grade == "B-":
            numbers.append(2.7)
        elif grade == "B":
            numbers.append(3.0)
        elif grade == "B+":
            numbers.append(3.3)
        elif grade == "A-":
            numbers.append(3.7)
        elif grade in {"A", "A+"}:
            numbers.append(4.0)
        else:
            raise ValueError(f"Unknown grade: {grade}")

    try:
        avg = sum(numbers) / len(numbers)
    except ZeroDivisionError:
        avg = 0

    if avg < 0.3:
        avg_grade = "F"
    elif avg < .7:
        avg_grade = "E+"
    elif avg < 1.0:
        avg_grade = "D-"
    elif avg < 1.3:
        avg_grade = "D"
    elif avg < 1.7:
        avg_grade = "D+"
    elif avg < 2.0:
        avg_grade = "C-"
    elif avg < 2.3:
        avg_grade = "C"
    elif avg < 2.7:
        avg_grade = "C+"
    elif avg < 3.0:
        avg_grade = "B-"
    elif avg < 3.3:
        avg_grade = "B"
    elif avg < 3.7:
        avg_grade = "B+"
    elif avg < 4.0:
        avg_grade = "A-"
    elif avg >= 4.0:
        avg_grade = "A"
    else:
        raise ValueError(f"Unexpected average: {avg}")
    return avg_grade
```

The equivalent code that uses `match`/`case` however does not:

```python
def grades_to_average(grades):
    numbers = []
    for grade in grades:
        match grade:
            case "F-" | "F" | "F+" | "E-" | "E":
                numbers.append(0)
            case "E+":
                numbers.append(.3)
            case "D-":
                numbers.append(.7)
            case "D":
                numbers.append(1.0)
            case "D+":
                numbers.append(1.3)
            case "C-":
                numbers.append(1.7)
            case "C":
                numbers.append(2.0)
            case "C+":
                numbers.append(2.3)
            case "B-":
                numbers.append(2.7)
            case "B":
                numbers.append(3.0)
            case "B+":
                numbers.append(3.3)
            case "A-":
                numbers.append(3.7)
            case "A" | "A+":
                numbers.append(4.0)
            case _:
                raise ValueError(f"Unknown grade: {grade}")

    try:
        avg = sum(numbers) / len(numbers)
    except ZeroDivisionError:
        avg = 0

    match avg:
        case avg if avg < .3:
            avg_grade = "F"
        case avg if avg < .7:
            avg_grade = "E+"
        case avg if avg < 1.0:
            avg_grade = "D-"
        case avg if avg < 1.3:
            avg_grade = "D"
        case avg if avg < 1.7:
            avg_grade = "D+"
        case avg if avg < 2.0:
            avg_grade = "C-"
        case avg if avg < 2.3:
            avg_grade = "C"
        case avg if avg < 2.7:
            avg_grade = "C+"
        case avg if avg < 3.0:
            avg_grade = "B-"
        case avg if avg < 3.3:
            avg_grade = "B"
        case avg if avg < 3.7:
            avg_grade = "B+"
        case avg if avg < 4.0:
            avg_grade = "A-"
        case avg if avg >= 4.0:
            avg_grade = "A"
        case _:
            raise ValueError(f"Unexpected average: {avg}")
    return avg_grade
```

Is this intentional? Should each `case` count as a conditional?

---

_Comment by @dhruvmanila on 2024-05-15 09:18_

I think it is intentional as Pylint doesn't detect it either.

> Should each `case` count as a conditional?

Personally, I wouldn't count it as I find the `match` statement to be more readable than an `if` statement. Additionally, the semantics of pattern matching is very different then the test expression of an `if` statement. I'd love to hear others opinion, cc @AlexWaygood @zanieb 

---

_Comment by @jaap3 on 2024-05-16 08:41_

I feel like the same complexity and maintainability arguments apply to `match` and `case`. They are definitely a way to achieve branching code and each case (especially the conditional ones containing `if`) could be considered to be a distinct statement right?

This issue should probably be converted to a discussion.

---

_Label `question` added by @dhruvmanila on 2024-05-20 05:35_

---

_Comment by @dhruvmanila on 2024-05-20 05:52_

> This issue should probably be converted to a discussion.

It's fine, we can discuss here.

I don't have any arguments against this, and it does make sense (I think?) to include the `match` statement similarly to an `if` statement. I would wait for others to share their opinions before we decide. It would also be useful to hear the thoughts of the Pylint maintainers. Do you want to open an issue on the Pylint repository similar to this? I can also do that :)

---

_Referenced in [astral-sh/ruff#11205](../../astral-sh/ruff/issues/11205.md) on 2024-05-22 03:13_

---

_Comment by @charliermarsh on 2024-05-22 03:17_

\cc @Pierre-Sassoulas 

---

_Comment by @Pierre-Sassoulas on 2024-05-22 05:07_

Thank you for the ping @charliermarsh :) I see no reason for match  to behave differently than if. I think it's an oversight when we added the match statement for python 3.10 we did not think to test the consequences on the already implemented "too-complex" (assumed the "too-complex" code was generic enough). 

---

_Comment by @Pierre-Sassoulas on 2024-05-22 06:47_

So, I did some light research and there might be a point for 'too-complex" behaving the way it does right now. It is based on Mc Cabe and:

> McCabe originally recommended exempting modules consisting of single mul-
tiway decision (“switch” or “case”) statements from the complexity limit. The multiway deci-
sion issue has been interpreted in many ways over the years, sometimes with disastrous
results. 

https://web.archive.org/web/20210908120324/https://www.mccabe.com/pdf/mccabe-nist235r.pdf (page 15, notice also that McCabe himself is one of the authors)

Also on page 26 we can see:

> A less frequently occurring issue that has greater impact on complexity is the distinction between
“case-labeled statements” and “case labels.” When several case labels apply to the same pro-
gram statement, this is modeled as a single decision outcome edge in the control flow graph,
adding one to complexity. It is certainly possible to make a consistent flow graph model in
which each individual case label contributes a different decision outcome edge and hence also
adds one to complexity, but that is not the typical usage

Imo, the match case in python adds complexity because it's not a simple "case labels", it's behaving more like an if, or even a regex.

---

_Referenced in [pylint-dev/pylint#9667](../../pylint-dev/pylint/pulls/9667.md) on 2024-05-22 07:17_

---

_Comment by @AlexWaygood on 2024-05-23 13:38_

I agree with @Pierre-Sassoulas. For the Mccabe plugin, I believe one of the primary motivations for the rule is to ensure that each function is testable in isolation. If a function has too many branches, it becomes hard to write a unit test for it. I think this is just as much a concern for `match`/`case` as it is with `if`/`elif`, as each new `case` statement in the `match`/`case` treee can be seen as a wholly distinct branch that you would need to account for when writing tests.

---

_Label `question` removed by @dhruvmanila on 2024-05-23 13:56_

---

_Label `rule` added by @dhruvmanila on 2024-05-23 13:56_

---

_Label `help wanted` added by @dhruvmanila on 2024-05-23 13:56_

---

_Comment by @dhruvmanila on 2024-05-23 13:57_

Thank you @Pierre-Sassoulas for chiming in! I agree with the conclusion. I think we should update all three rules to consider `match` statement and we can see how many changes we see in the ecosystem checks.

---

_Comment by @blueraft on 2024-05-23 16:09_

I can take a stab at this if that's ok. 

---

_Comment by @charliermarsh on 2024-05-23 16:37_

Go for it -- thanks!

---

_Referenced in [astral-sh/ruff#11521](../../astral-sh/ruff/pulls/11521.md) on 2024-05-23 19:34_

---

_Closed by @dhruvmanila on 2024-05-24 09:14_

---

_Comment by @jaap3 on 2024-05-24 14:33_

Thanks everyone!

---

_Comment by @qartik on 2024-05-29 12:44_

I wish the consideration for match-case statement for PLR rules was configurable to be turned off with lint.pylint setting. Long match-case statements are very common while walking abstract syntax trees and are arguably way less complex than any other solution. See usage, e.g. at https://github.com/CQCL/pytket-phir/blob/main/pytket/phir/phirgen.py#L102-L148 which now raises  both PLR 912 and 915.

@jaap3, @blueraft, @dhruvmanila what do you think? If there is agreement, I can file a separate issue.

---

_Comment by @AlexWaygood on 2024-05-29 12:52_

> Long match-case statements are very common while walking abstract syntax trees and are arguably way less complex than any other solution.

I'd agree that match/case statements are more readable for this purpose, and arguably more idiomatic, but I don't think there's any fewer branches than if you walked the tree using if/elif/else, and the number of branches is what these rules are concerned with. I don't see a strong reason to make match/case statements configurable here specifically. I think I'd find it just as difficult to write comprehensive unit tests for a function with many `case` branches as I would for a function with many `elif` branches

---

_Comment by @qartik on 2024-05-29 15:31_

The only thing to add is there is no simpler way to process an enumeration than pattern matching (such as those that show up in AST walks as mentioned) and for those cases, I'd argue complexity considerations covered by these rules do not apply.

With current change, any parsers/compilers processing a grammar could be full of ruff ignores for each such match construct (or in each file, but then missing several other cases) or we can let users choose whether they want these code complexity heuristics to apply to match-case.

---

_Comment by @AlexWaygood on 2024-05-29 15:38_

To me that just seems to be arbitrarily favouring some language constructs at the expense of others in the name of readability and style, which isn't what this rule is meant to be ultimately concerned about. If the argument is that there are many cases where writing a more complex function is in fact more maintainable and redable than splitting the function into several smaller functions, then I'd agree that that's a valid critique. But to me, that seems like a flaw of the rule in general rather than a flaw of the rule as specifically applied to `match`/`case` statements.

---

_Comment by @albertomurillo on 2024-10-17 07:14_

The exhaustive match/case pattern for all possible values in an enumeration is actually promoted by the rust compiler, it is a good practice to replicate in other languages such as python but that triggers the complexity warning.

It does make a difference to have if/elif/... statements vs match/case/... because in an if/elif/... scenario you can have any kind of conditional while on the match/case scenario is only values for the same variable.

Anyway, a config setting to revert the rule to the previous behavior would be the best of both worlds.

---
