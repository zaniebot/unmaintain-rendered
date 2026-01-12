```yaml
number: 5810
title: "Update `TupleParentheses` usage"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: chore-tuple-paren
created_at: 2023-07-16T17:07:04Z
updated_at: 2023-07-24T14:46:00Z
url: https://github.com/astral-sh/ruff/pull/5810
synced_at: 2026-01-12T03:30:21Z
```

# Update `TupleParentheses` usage

---

_Pull request opened by @cnpryer on 2023-07-16 17:07_

## Summary

Implements a potentially more re-usable naming convention for `TupleParentheses`.

`Subscript` case  -> `Preserve` - "Preserve the tuple's parentheses if they are there"
`SkipInsideLambda` case  -> `Never` - "Parentheses are never needed with this parent"  (#5806; [comment](https://github.com/astral-sh/ruff/pull/5790#discussion_r1264593822))
`Comprehension` case -> `Never` - "Parentheses are never needed with this parent" (#5790; [comment](https://github.com/astral-sh/ruff/pull/5790#discussion_r1264593822))
`StripInsideForLoop` case -> `NeverPreserve` - "Parentheses are never preserved in special cases" (I interpret "Preserve" as "not always necessary but keep them anyway")

The counter to this I'd think is that there may be benefit to having node-specific differences.

## Test Plan

Existing snapshots.

_____

**Questions**

- ~~Where would `TupleParentheses::Expr(...)` be helpful in some of these cases (if at all)?~~ (answered)

Related:
- [Handle parenthesized `Argument`](https://github.com/astral-sh/ruff/issues/5779#issuecomment-1637614763)


---

_Comment by @github-actions[bot] on 2023-07-16 17:21_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.03ms     4.3 MB/sec    1.00      9.4±0.05ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1879.5±2.54µs     8.9 MB/sec    1.00   1877.0±3.25µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    205.2±0.56µs    14.4 MB/sec    1.02    209.9±0.28µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.01ms     6.3 MB/sec    1.00      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.1±0.05ms     3.1 MB/sec    1.00     13.0±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.3±4.34µs     6.8 MB/sec    1.00    433.0±0.85µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.04ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1406.8±2.11µs    11.8 MB/sec    1.00   1400.6±2.28µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.0±1.16µs    18.9 MB/sec    1.00    156.2±1.00µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.4±0.50ms     3.3 MB/sec    1.15     14.2±0.40ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.10ms     6.8 MB/sec    1.13      2.8±0.08ms     6.0 MB/sec
formatter/numpy/globals.py                 1.00   284.9±14.34µs    10.4 MB/sec    1.10   313.2±12.10µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.20ms     4.8 MB/sec    1.14      6.0±0.17ms     4.2 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.61ms     2.3 MB/sec    1.05     18.9±0.95ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.9±0.28ms     3.4 MB/sec    1.00      4.8±0.19ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.06   610.8±29.39µs     4.8 MB/sec    1.00   575.3±30.83µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.2±0.29ms     3.1 MB/sec    1.00      8.0±0.34ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.42ms     4.3 MB/sec    1.08     10.2±0.58ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1958.9±86.55µs     8.5 MB/sec    1.04      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.5±19.08µs    12.0 MB/sec    1.01   249.5±11.91µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.24ms     5.8 MB/sec    1.00      4.4±0.15ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @cnpryer on 2023-07-16 17:24_

---

_Marked ready for review by @cnpryer on 2023-07-16 17:44_

---

_Label `formatter` added by @konstin on 2023-07-16 18:18_

---

_@cnpryer reviewed on 2023-07-16 18:35_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:117 on 2023-07-16 18:35_

I'm not in love with this.

---

_@cnpryer reviewed on 2023-07-16 18:48_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:117 on 2023-07-16 18:48_

Maybe ec9b23d4e0a041c0c150d789e4e32be9784ce96f

---

_@cnpryer reviewed on 2023-07-16 18:51_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:117 on 2023-07-16 18:51_

But I guess same question as https://github.com/astral-sh/ruff/pull/5790#discussion_r1264593822 but for `TupleParentheses::Expr(Parentheses::Preserve)`.

---

_Review requested from @konstin by @MichaReiser on 2023-07-17 07:45_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:29 on 2023-07-18 08:27_

I'd rename this to Preserve to match `Parentheses::Preserve`

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:21 on 2023-07-18 08:27_

I don't think this variant is used anymore, it would make sense to remove it and replace the one usage with `Default` or not passing the option (which also sets `Default`)

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:29 on 2023-07-18 08:48_

Could you add these as black formatting examples to the source code? I think it's easier for someone later to follow what is actually happening if you can see the cases.
```python
x[a, :]
x[a, b:]
x[(a, b):]
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:82 on 2023-07-18 08:51_

Similarly, could you add these examples?

```python
for (a,) in []:
    pass
for a, b in []:
    pass
for a, b in []:  # Strips parentheses
    pass
for (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
    b,
) in []:
    pass
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-18 08:52_

Could you describe the use case where this is required, ideally with a python code snippet and how black formats this differently from the other variants?

---

_@konstin reviewed on 2023-07-18 08:57_

Thanks for tackling something as finicky as the tuple parentheses! We discussed this in team and we realized naming these is hard with all the black edge cases. It would be great if we could reduce this enum to as few variants as possible to reduce complexity

---

_Comment by @cnpryer on 2023-07-19 00:51_

Really appreciate all of this direction. I'm going to wrap up #5806 and then #5790. Then I can come back here with everything you need to see if this is even something that's worth merging.

---

_@cnpryer reviewed on 2023-07-19 01:24_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:29 on 2023-07-19 01:24_

Would you prefer I add these fixtures in a separate PR? I might do that to get started and if you want it here we can just close that, but my thought process is that if we decide to ditch any `TupleParentheses` changes we'd still want those fixtures.

---

_@cnpryer reviewed on 2023-07-19 01:30_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:21 on 2023-07-19 01:30_

d7f0c26a3ac870caba44b80a0622adbb217ecbc2

I chose 
> not passing the option

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:117 on 2023-07-19 01:44_

d7f0c26a3ac870caba44b80a0622adbb217ecbc2

---

_@cnpryer reviewed on 2023-07-19 01:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:29 on 2023-07-19 01:45_

I think I misunderstood. You wanted these in the comments.

---

_@cnpryer reviewed on 2023-07-19 01:45_

---

_@cnpryer reviewed on 2023-07-19 01:54_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:82 on 2023-07-19 01:54_

This one makes me think you want fixtures, not comment examples. I'll add the comment examples for the last suggestion. Do you mean to add these as fixtures? This is what's there currently:

```rs
    /// Handle the special cases where we don't include parentheses if they are not required.
    ///
    /// Normally, black keeps parentheses, but in the case of for loops it formats
    /// ```python
    /// for (a, b) in x:
    ///     pass
    /// ```
    /// to
    /// ```python
    /// for a, b in x:
    ///     pass
    /// ```
    /// Black still does use parentheses in these positions if the group breaks or magic trailing
    /// comma is used.
    NeverPreserve,
```

---

_@cnpryer reviewed on 2023-07-19 01:55_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-19 01:55_

Sure. I'll compile some examples tomorrow.

---

_@cnpryer reviewed on 2023-07-19 01:59_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:29 on 2023-07-19 01:59_

89268f0d6ed17c877f6d73ea2926f44fab153b20 (potentially resolved)

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-19 23:17_

So the first example that motivated this was #5790. 

```py
# Input
{k: v for k, v in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension}

# black
{
    k: v
    for k, v in this_is_a_very_long_variable_which_will_cause_a_trailing_comma_which_breaks_the_comprehension
}
```
`black` will *never* format tuple `for` targets with parentheses if the target tuple is found in a comprehension. 

> how black formats this differently from the other variants

If I understand this, in other variations of statements or expressions tuples often don't format this way. For example

```py
# Input
for a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a, a in []:
    ...

# Black
for (
    a,
    a,
    a,
    # ...
    a,
    a,
) in []:
    ...
```
This for loop is not found in a comprehension and formats with expended multi-line wrapping parentheses.

For the comprehension implementation I understood its `TupleParentheses` options to be "here we *never* want parentheses.

> It would be great if we could reduce this enum to as few variants as possible to reduce complexity

I have new thoughts here. 
1. I feel like *complexity* can be defined as "many variants" but it could also be more *complex* to try to build special case formatting with *less* variants. I'm leaning towards less variants anyway, but I thought I'd share that. 
1. If we want less variant surface area for `TupleParentheses`, why would we stop there? I haven't encountered this yet contributing here, but would it be better to have a broader enum like `SpecialParentheses` for more than just special tuple cases? (terrible name but hopefully you get the point)

---

_@cnpryer reviewed on 2023-07-19 23:17_

---

_@cnpryer reviewed on 2023-07-20 00:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-20 00:44_

I re-read [this](https://github.com/astral-sh/ruff/pull/5790#discussion_r1267598124). My second thought above (2) doesn't seem like a good path at this time.

---

_@cnpryer reviewed on 2023-07-20 01:11_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:82 on 2023-07-20 01:11_

395404b5e9ef2769bc9f0c32473a3f083abd5933 (potentially resolved)

---

_@cnpryer reviewed on 2023-07-20 01:12_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-20 01:12_

I added this example to 2e1ab30efd436370c2e6292743a65941662c106a

---

_Comment by @cnpryer on 2023-07-20 01:13_

I'm not in love with `TupleParentheses::NeverPreserve`. I'm going to think a bit more about some good variant names.

---

_@cnpryer reviewed on 2023-07-20 01:14_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:19 on 2023-07-20 01:14_

This needs to be updated

---

_@cnpryer reviewed on 2023-07-20 01:19_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:19 on 2023-07-20 01:19_

6e6bb89688d7e2be8452c7df7404d0e3539a0d79

---

_Comment by @MichaReiser on 2023-07-20 07:09_

> I'm not in love with `TupleParentheses::NeverPreserve`. I'm going to think a bit more about some good variant names.

@konstin likes the name a lot. I believe it has the same semantics as `Parenthesize::IfBreaks` (which naming I find okay, but not good)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:32 on 2023-07-20 07:12_

I think it's worth pointing out that this implies that the tuple elements will all be formatted on "the same line" (except if the element itself supports breaking in an un-parenthesized context). 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-20 07:17_

We currently use `f.context().node_level().is_parenthesized()` to know if an expression is in a parenthesized context. The comprehension `Never` formatting *pretends* that it is outside of parentheses. That makes me wonder if we need to set the `node_level` to `NodeLevel::Expression(None)`. 

The way to figure out the desired behavior is to use a tuple in a parenthesized expression and add an element that only expands in parenthesized contexts (binary expression). Good that comprehensions are always parenthesized, so this is a given

```python
[ a for (aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb, ccccccccccccccccccccc) in b
]
```

[Playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZX6BgGgIVjIYADDQHqw1xrTXmtVZMGAMgHXOtde6z13rfX+sDcG0NvrzAwCVvGxNybU2sBhDAPwdrIBGBAA)

Does black expand the binary expression before the `+`? 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:15 on 2023-07-20 07:18_

This comment is outdated now. Can we document how this "layout" looks like (e.g. how is it different from `Preserve`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-20 07:20_

Thanks for example. It seems like you've identified the *minimal* set, because each variant differs in at least one property from the other variants. 

---

_@MichaReiser approved on 2023-07-20 07:21_

This looks good to me. I've one outstanding question about what the expected behavior for lower priority (`in_parentheses_only_group`) split points is inside of comprehensions. 

---

_@cnpryer reviewed on 2023-07-20 13:08_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:15 on 2023-07-20 13:08_

Am I understanding this one right? `TupleParentheses::Default` would *default* the options to whatever the "general parenthesize" strategy is. So it would be different if the general strategy isn't `Preserve`?

---

_@cnpryer reviewed on 2023-07-20 13:11_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:32 on 2023-07-20 13:11_

I added some more to the comments and can push in a bit.

---

_@cnpryer reviewed on 2023-07-20 13:12_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-20 13:12_

I'll check this out today.

---

_@cnpryer reviewed on 2023-07-20 16:21_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:32 on 2023-07-20 16:21_

97710d2836c98bcf9a3c87637416f72c61cfea21

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:15 on 2023-07-21 07:03_

Yes. `Default` is the strategy that is used in the majority of tuple usages. We should either document how `Default` and `Preserve` differ *or* merge the two. 

---

_@MichaReiser reviewed on 2023-07-21 07:03_

---

_@cnpryer reviewed on 2023-07-21 16:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-21 16:44_

A little caught up with work. Today or tomorrow I can loop back here.

---

_@MichaReiser reviewed on 2023-07-21 19:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-21 19:17_

No worries. Have a great weekend!

---

_@cnpryer reviewed on 2023-07-21 22:33_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-21 22:33_

You too! :)

---

_@cnpryer reviewed on 2023-07-22 17:29_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-22 17:29_

So the example is invalid syntax

```
File "/Users/chrispryer/github/ruff/scratch.py", line 1
    [ a for (aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb, ccccccccccccccccccccc) in b
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: cannot assign to expression
```

Which has me reading more docs on comprehensions. I'm surprised I never ran into this before because I use comprehensions a lot. I guess it's just a super arbitrary example (which IMO isn't a bad thing for this PR). IIUC it's expecting a *name-like* expression, or a *valid assignment target*. So I need to find an example of a valid assignment target that will always expand in a parenthesized context, but 

> That makes me wonder if we need to set the node_level to NodeLevel::Expression(None).

Logically this makes sense, so I do want to better understand edge cases here.

---

_@cnpryer reviewed on 2023-07-22 17:31_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-22 17:31_

Backing out of comprehension contexts, you can get this expansion with other contexts like lists for example -- but that's fairly obvious. So I can check out more examples before I stamp this as fully understood.

---

_@cnpryer reviewed on 2023-07-22 17:44_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:15 on 2023-07-22 17:44_

I'm struggling to communicate how `Default` and `Preserve` could be different for tuples specifically, because semantically they represent the same thing to me. "Tuples by default will *preserve* their parentheses".

So I'm leaning towards merging. 

EDIT: Tuples as part of assignment statements seems to be an area I should explore more here. I'm checking this out today.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:23 on 2023-07-22 18:21_

Marking this as resolved

---

_@cnpryer reviewed on 2023-07-22 18:21_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:15 on 2023-07-22 19:39_

So I decided that my interpretation of default tuple parentheses strategy was incomplete. A complete definition is probably closer to "Tuples by default will preserve their parentheses, but in some cases parentheses may also be introduced".

Now if that's a *good default* is a different question (I think it's fine, just more complicated), but I added some of this verbiage in 9e65b9acba75e75644007e0001fc76b31206437a 

---

_@cnpryer reviewed on 2023-07-22 19:39_

---

_@cnpryer reviewed on 2023-07-23 01:06_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-23 01:06_

Okay so returning to this I wanted to at least check out a bin op within a comprehension's context. Inside the generator you don't get the expansion before the `+`.

```python
# Input
[i for i in (aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb, ccccccccccccccccccccc)]

# Black
[
    i
    for i in (
        aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb,
        ccccccccccccccccccccc,
    )
]
```

And as the `elt` you don't either.

```python
# Input
[(aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb, ccccccccccccccccccccc) for i in b]

#Black
[
    (
        aaaaaaaaaaaaaaaaaaaaaa + bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb,
        ccccccccccccccccccccc,
    )
    for i in b
]
```

---

_@MichaReiser reviewed on 2023-07-24 07:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-24 07:19_

Thanks for the research and sorry for providing an invalid example. 

Does our formatting match black's behavior? If not, should we merge the PR regardless (my preference), because fixing the formatting is probably unrelated to the `TupleParentheses` usage

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-24 12:36_

> Does our formatting match black's behavior?

Looks like it c31643a5964f0f40c5f2081648213ea4fd0ac28e

---

_@cnpryer reviewed on 2023-07-24 12:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-24 14:28_

Perfect. So this PR is ready to merge after rebase. Or is there something left to be done?

---

_@MichaReiser reviewed on 2023-07-24 14:28_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:155 on 2023-07-24 14:33_

Nah you're right. I've just been dealing with something. Sorry for the delay on resolving the conflicts.

---

_@cnpryer reviewed on 2023-07-24 14:33_

---

_Comment by @MichaReiser on 2023-07-24 14:41_

Thank you. Enabling auto merge. Hopefully, this flushes through. 

---

_Merged by @MichaReiser on 2023-07-24 14:44_

---

_Closed by @MichaReiser on 2023-07-24 14:44_

---

_Branch deleted on 2023-07-24 14:46_

---
