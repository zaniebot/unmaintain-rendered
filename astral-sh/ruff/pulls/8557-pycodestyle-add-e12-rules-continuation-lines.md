```yaml
number: 8557
title: "[`pycodestyle`] Add E12 rules (continuation lines indentation rules)"
type: pull_request
state: closed
author: hoel-bagard
labels:
  - rule
  - preview
  - incompatibility
assignees: []
base: main
head: hoel/add_E12_rules
created_at: 2023-11-08T13:09:44Z
updated_at: 2024-04-03T13:04:04Z
url: https://github.com/astral-sh/ruff/pull/8557
synced_at: 2026-01-10T22:47:01Z
```

# [`pycodestyle`] Add E12 rules (continuation lines indentation rules)

---

_Pull request opened by @hoel-bagard on 2023-11-08 13:09_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/2402, it adds the `E12` rules (continuation lines indentation rules).

## Test Plan

The test fixture uses [the one from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testing/data/E12.py
), except for `E133` for which there is no fixture since it is an opt-in config option (same as pycodestyle) and I do not know how to handle it.

## Discussion

This PR replaces the [add E122](https://github.com/astral-sh/ruff/pull/7655) PR.

### Performance issue

The changes from the PR seem to slow down ruff quite a bit, I'm guessing that the `get_token_infos` function is the slow part, but it was needed to follow the pycodestyle implementation.  Is there a way to implement it that would be faster ?

### Extra rules
The PR is essentially a port of the [pycodestyle implementation](https://github.com/PyCQA/pycodestyle/blob/main/pycodestyle.py#L545) from python to rust. I implemented all the errors present in that function before realizing that [E121](https://www.flake8rules.com/rules/E121.html), [E123](https://www.flake8rules.com/rules/E123.html), [E126](https://www.flake8rules.com/rules/E126.html) and [E133](https://www.flake8rules.com/rules/E133.html) are not in the #2402 list. Should I remove them from the PR ?

### Test error
When running the tests, the following deserialization test fails:

```rust
        assert!(toml::from_str::<Pyproject>(
            r#"
[tool.black]
[tool.ruff.lint]
select = ["E123"]
"#,
        )
        .is_err());
```

I'm not sure what to do about it, although I suppose the issue would disappear if `E123` is removed from the PR. 

---

_Comment by @codspeed-hq[bot] on 2023-11-08 13:19_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:hoel/add_E12_rules)

### Merging #8557 will **degrade performances by 7.6%**

<sub>Comparing <code>hoel-bagard:hoel/add_E12_rules</code> (382429c) with <code>main</code> (7391f74)</sub>



### Summary

`❌ 5` regressions
`✅ 20` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/hoel-bagard:hoel/add_E12_rules)._

### Benchmarks breakdown

|     | Benchmark | `main` | `hoel-bagard:hoel/add_E12_rules` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[pydantic/types.py]` | 74.5 ms | 80.6 ms | -7.6% |
| ❌ | `linter/all-rules[large/dataset.py]` | 174 ms | 187.2 ms | -7.03% |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 16.3 ms | 17.2 ms | -5.15% |
| ❌ | `linter/all-rules[numpy/ctypeslib.py]` | 34.8 ms | 37.3 ms | -6.83% |
| ❌ | `linter/all-rules[numpy/globals.py]` | 3.9 ms | 4.2 ms | -5.59% |


---

_Comment by @Avasam on 2023-12-14 07:50_

As a note, fixes for these would bring Ruff closer to parity with autopep8 https://github.com/astral-sh/ruff/discussions/9057#discussioncomment-7850584

---

_Comment by @spaceone on 2024-02-18 19:21_

This is nearly the last missing pycodestyle check. Having it would allow use to throw away the slow `pycodestyle`, `flake8` and  `autopep8` pre-commit hooks.

---

_Comment by @hoel-bagard on 2024-02-25 11:00_

@MichaReiser sorry to ping you here, but I would appreciate some feedback/advice on this PR.

I had implemented the E12 rules for completeness (to get closer to having all the boxes checked in #2402), but I went at it lazily and used the same logic as pycodesyle, which resulted in the PR not even passing the CI.

Do you think it would be worth it to spend time trying to fix/rework it, or should this PR be closed and forgotten ?

---

_Comment by @MichaReiser on 2024-02-25 12:11_

@hoel-bagard I try to take a look on Monday. I've a lot of catch up to do on PRs and I'm, unfortunately, not as fast as @charliermarsh 

---

_Label `rule` added by @MichaReiser on 2024-02-25 12:11_

---

_Label `preview` added by @MichaReiser on 2024-02-25 12:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continuation_lines.rs`:202 on 2024-02-26 08:39_

The difference between this and `ContinuationLineOverIndentedForHangingIndent` and `ContinuationLineUnderIndentedForVisualIndent` isn't clear to me from reading the documentation. Are these different styles and users should only enable one of those rules?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continuation_lines.rs`:459 on 2024-02-26 08:41_

This would need to support the new `indent_width` setting

---

_@MichaReiser reviewed on 2024-02-26 08:46_

Hey @hoel-bagard 

I haven't reviewed the code yet but I read through the rules.

My main concern is that not all rules are formatter-compatible. For example, `ClosingBracketNotMatchingOpeningBracketVisualIndentation` is guaranteed to conflict with all formatted code. This is problematic because many users use `--select ALL`, which enables these new rules, and they then get a lot of errors if they use our formatter (and see an incompatibility warning when running format). That's why we want to hold off from adding new formatting-related lint rules that are formatter incompatible until we're done with the rules re-categorization where we are likely to have a `format` (or similar) related category that we only recommend for users that **do not** use our formatter.

I'm having a hard time judging if other rules conflict with the formatter, too, from just reading their documentation. E.g. I'm unsure about `ContinuationLineOverIndentedForHangingIndent`, `ContinuationLineOverIndentedForVisualIndent`, 

I believe `VisuallyIndentedLineWithSameIndentAsNextLogicalLine` and `ContinuationLineUnalignedForHangingIndent` are incompatible, too (at least today, we are considering changing that).

I'm sorry that this means we can't move forward with the rules as they are now. We could explore limiting the rules to the formatter-compatible ones or finding a way to make them formatter-compatible. 

---

_Comment by @Corentin-pro on 2024-02-26 15:41_

Hello, I want to emphasize that this addition would be very welcome. I had many instances where I would have liked ruff to tell me about over-indentation (E127) when I refactored my code (removing arguments from a function that changed the indentation).

I am interested in this PR. If I have the time and the code is not yet OK to be merge I'll look into this in the next month.

---

_@hoel-bagard reviewed on 2024-02-27 00:23_

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continuation_lines.rs`:202 on 2024-02-27 00:23_

It seems to me like they could both apply in one codebase, although that would not be a very consistent codebase.

Looking at the [fixtures from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testing/data/E12.py),  `E126` (`ContinuationLineOverIndentedForHangingIndent`) seems to apply when there is a newline after the symbol causing the continuation line (`(`, `\`, etc...) whereas `E128` (`ContinuationLineUnderIndentedForVisualIndent`) applies when there is at least one value after the start of the continuation line.

For example this is a `E126` error
```python
print("E126", (
                "1",
    "2",
))
```

Whereas this is a `E128` error:
```python
print("E128", ("1",
    "2",
))
```

See also the unofficial rules explanation for [E126](https://www.flake8rules.com/rules/E126.html) and [E128](https://www.flake8rules.com/rules/E128.html)

---

_Comment by @hoel-bagard on 2024-02-27 00:40_

Having rules that conflict with the formatter would be quite annoying indeed (I already noticed the formatter disagreeing with flake8, and that meant having to add an exception to the flake8 config).

I tried running ruff with the `E12` rules on one (small) codebase that has been formatted with the ruff formatter, and got no error.

I tried to make an example that would cause the formatter and the `E129` (`VisuallyIndentedLineWithSameIndentAsNextLogicalLine`) to disagree, but without success.

The following triggers `E129`:
```python
if (var in "one_looooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooong_string"
    and var not in "another_looooooooooooooooooooooooooooooooooooooooooooooooooooong_string"):
    ...
```

But it get formatted into the `E12` compliant code:
```python
if (
    var
    in "one_looooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooong_string"
    and var
    not in "another_looooooooooooooooooooooooooooooooooooooooooooooooooooong_string"
):
    ...
```

The docstring examples and fixtures would definitely need to be simplified, I'll try to do that today/tomorrow. I wanted to run the formatter on the `E12` fixtures, but it uses python2 style code, which prevents the formatter from formatting the code.

---

_Comment by @hoel-bagard on 2024-02-27 02:53_

@MichaReiser I ran ruff format on the `E12.py` fixtures file, and it fixed all but two of the errors.

The errors that did not get fixed automatically are both `E125` errors:
```python
if """
    """:
    pass

for foo in """
    abc
    def
    """.strip().split():
    print(foo)
```

They can be fixed by manually formatting them to:
```python
if """""":
    pass

for foo in """
    abc
    def
""".strip().split():
    print(foo)
```
(this changes the content of the string, but that's not really the point here)

At which point the formatter does not attempt to change the file anymore, and both pycodestyle and the rules added in this PR do not detect any `E12` violation.

So it seems like the `E12` rules are not incompatible with the formatter.

---

_Comment by @MichaReiser on 2024-03-04 13:01_

I haven't forgotton about this PR but we first need to decide internally if we want to add more formatting lint rules and my priority for now is to fix incompatibilities with existing rules.

---

_Label `incompatibility` added by @zanieb on 2024-03-12 18:34_

---

_Comment by @MichaReiser on 2024-03-28 17:10_

Thank you @hoel-bagard for your patience and sorry that it took me so long. 

I started reviewing the rules but stopped after E125 because discovering all possible formatter incompatibilities took a very long time, and I generally struggled to understand the rules or how they differ (this isn't a critique, it's just me struggling and not being familiar with the rules).

For `E121`: I see how the rule is useful, but the rule is in conflict with some possible expression formatting that I want to try out in the formatter.  That's why I want to hold off adding this specific rule, at least until #1774 is complete. I heard @AlexWaygood has ideas on how to support formatter incompatible rules ;)

```python
a = (
    a 
    + b
    + len([
        1, 
        2, 
      ]) # NOK
    + more([
        a, 
      ]) # NOK
)
```

I'm generally open to accepting the rules `E122` and `E125` (I still have to review the other rules), but I would prefer if we could tackle one rule at a time in separate PRs. I'm aware that this asks for extra work on your side and I'm sorry for this, but I don't think I can prioritize reviewing all the additions in a single PR right now. Adding the rules one by one also has the benefit that it reducing the risk of regressions because we can ship them independently (we would still have the regression, but we would receive the feedback across multiple releases)

The other thing we need to consider is that the rule seems to regress performance somewhat significantly. I think we need to figure out a design that reduces the performance less. This could involve refactoring existing pycodestye rules to see if performance can be improved. 

What do you think? Do you want to pursue the rules in individual PRs (please don't open all PRs at once, let's do one at a time)?


---

_Comment by @hoel-bagard on 2024-03-31 02:01_

@MichaReiser 

Thanks for taking time to look into it.
While I use the formatter, I never looked too much into its rules, so I don't know about the incompatibilities with the `E12` rules. I'll try to check for incompatibilities while working on it. 

Implementing the rules one by one sounds good to me, that would also likely make understanding what each rule does easier.

> The other thing we need to consider is that the rule seems to regress performance somewhat significantly. I think we need to figure out a design that reduces the performance less. This could involve refactoring existing pycodestye rules to see if performance can be improved.

I suspect that working directly on tokens would help with the performance issue. I used logical lines because that's what pycodestyle does, but I don't think that was a good idea.

> What do you think? Do you want to pursue the rules in individual PRs (please don't open all PRs at once, let's do one at a time)?

I can try to start working on `E122` and see how it goes. I won't spend time on the other rules before that one gets merged (there's no point working on the other rules before solving the formatter incompatibilities / performance issues).

---

_Comment by @MichaReiser on 2024-04-02 14:03_

> I suspect that working directly on tokens would help with the performance issue. I used logical lines because that's what pycodestyle does, but I don't think that was a good idea.

Logical lines might be fine, I'm not sure. One problem I noticed while skimming over the code is that we allocate multiple vectors and hash sets for each line. I think we should look into how we can avoid that. It can mean that we should use something other than logical line, reuse the allocations across check calls etc. 

> I can try to start working on E122 and see how it goes. I won't spend time on the other rules before that one gets merged (there's no point working on the other rules before solving the formatter incompatibilities / performance issues).

Thanks a lot! I'll close this PR. We can still use it as a reference. I hope that's okay with you.

---

_Closed by @MichaReiser on 2024-04-02 14:03_

---

_Comment by @hoel-bagard on 2024-04-03 13:04_

I think the main issue with logical lines is that they have to be sliced to look for new lines (like [here](https://github.com/astral-sh/ruff/pull/8557/files#diff-035fb3ef6174f555db00ce0f906fece3167eb58572c9389ffe98d1608d49b491R397) for example). This currently happens for every token, which I assume is the slow part.

> One problem I noticed while skimming over the code is that we allocate multiple vectors and hash sets for each line. I think we should look into how we can avoid that. 

If you're referring to [this](https://github.com/astral-sh/ruff/pull/8557/files#diff-035fb3ef6174f555db00ce0f906fece3167eb58572c9389ffe98d1608d49b491R418), then it's being done for every token, which is most likely more than needed indeed.

> I'll close this PR. We can still use it as a reference. I hope that's okay with you.

No issue at all. I'd like to keep using this PR as a reference / discussion point to avoid going into a wrong direction.



---
