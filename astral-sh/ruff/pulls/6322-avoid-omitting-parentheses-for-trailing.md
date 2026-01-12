```yaml
number: 6322
title: Avoid omitting parentheses for trailing attributes on call expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/compare-call
created_at: 2023-08-03T23:48:26Z
updated_at: 2023-08-10T15:20:51Z
url: https://github.com/astral-sh/ruff/pull/6322
synced_at: 2026-01-12T02:52:04Z
```

# Avoid omitting parentheses for trailing attributes on call expressions

---

_Pull request opened by @charliermarsh on 2023-08-03 23:48_

## Summary

This PR modifies our `can_omit_optional_parentheses` rules to ensure that if we see a call followed by an attribute, we treat that as an attribute access rather than a splittable call expression.

This in turn ensures that we wrap like:

```python
ct_match = aaaaaaaaaaact_id == self.get_content_type(
    obj=rel_obj, using=instance._state.db
)
```

For calls, but:

```python
ct_match = (
    aaaaaaaaaaact_id == self.get_content_type(obj=rel_obj, using=instance._state.db).id
)
```

For calls with trailing attribute accesses.

Closes https://github.com/astral-sh/ruff/issues/6065.

## Test Plan

Similarity index before:

- `zulip`: 0.99436
- `django`: 0.99779
- `warehouse`: 0.99504
- `transformers`: 0.99403
- `cpython`: 0.75912
- `typeshed`: 0.72293

And after:

- `zulip`: 0.99436
- `django`: 0.99780
- `warehouse`: 0.99504
- `transformers`: 0.99404
- `cpython`: 0.75913
- `typeshed`: 0.72293


---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:443 on 2023-08-03 23:49_

I'm guessing this is not the right solution, but it passes the new fixtures. Should this instead be in `expr_compare.rs`?

---

_@charliermarsh reviewed on 2023-08-03 23:49_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-03 23:49_

---

_Comment by @github-actions[bot] on 2023-08-04 00:17_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.4±0.42ms     3.9 MB/sec    1.00     10.3±0.49ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.08ms     8.2 MB/sec    1.01      2.1±0.11ms     8.1 MB/sec
formatter/numpy/globals.py                 1.01   235.8±13.71µs    12.5 MB/sec    1.00   234.5±11.59µs    12.6 MB/sec
formatter/pydantic/types.py                1.03      4.4±0.22ms     5.8 MB/sec    1.00      4.3±0.20ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.33ms     2.9 MB/sec    1.05     14.9±0.43ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.13ms     4.6 MB/sec    1.02      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   506.6±21.17µs     5.8 MB/sec    1.04   526.4±25.39µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.66ms     4.0 MB/sec    1.05      6.7±0.16ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.17ms     5.8 MB/sec    1.03      7.2±0.22ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1500.9±40.38µs    11.1 MB/sec    1.00  1488.2±50.69µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   183.3±10.29µs    16.1 MB/sec    1.00    182.6±8.51µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.10ms     8.1 MB/sec    1.02      3.2±0.20ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.2±0.17ms     4.0 MB/sec    1.00     10.2±0.83ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1928.0±33.78µs     8.6 MB/sec    1.00  1930.2±39.16µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    218.2±4.82µs    13.5 MB/sec    1.01    221.4±9.21µs    13.3 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.11ms     6.0 MB/sec    1.00      4.2±0.07ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.21ms     3.2 MB/sec    1.05     13.6±0.26ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.08ms     4.7 MB/sec    1.00      3.4±0.05ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.05    437.0±7.94µs     6.8 MB/sec    1.00    417.7±7.62µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.15ms     4.3 MB/sec    1.02      6.1±0.11ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.11ms     5.9 MB/sec    1.00      6.8±0.09ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1411.1±28.46µs    11.8 MB/sec    1.00  1378.0±19.18µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    159.8±2.34µs    18.5 MB/sec    1.00    154.9±3.11µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.05ms     8.4 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:443 on 2023-08-04 06:18_

The method here mirrors https://github.com/psf/black/blob/2fd9d8b339e1e2e1b93956c6d68b2b358b3fc29d/src/black/lines.py#L884-L966 I had a quick glance and don't seem to find this logic. But your change seems correct because the method returns `false` for the provided example. I recommend adding print statements to black's code base and then running the formatting on the code snipped to better understand what the underlying logic is.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:443 on 2023-08-04 06:19_

@konstin any chance this already gets fixed by your Call chain PR?

---

_@MichaReiser requested changes on 2023-08-04 06:31_

This logic doesn't seem to be specific to compare operators. It also applies to boolean operators and, I assume for binary expressions too. 

```python
def f():
    return (
        unicodedata.normalize("NFKC", s1).casefold()
        == unicodedata.normalize("NFKC", s2).casefold()
    )
    return (
        unicodedata.normalize("NFKC", s1).casefold()
        and unicodedata.normalize("NFKC", s2).casefold()
    )


ct_match = (
    aaaaaaaaaaact_id == self.get_content_type(obj=rel_obj, using=instance._state.db).id
)

ct_match = (
    aaaaaaaaaaact_id and self.get_content_type(obj=rel_obj, using=instance._state.db).id
)

ct_match = aaaaaaaaaaact_id == xyzaaaaaa

ct_match = aaaaaaaaaaact_id and xyzaaaaaa


ct_match = {aaaaaaaaaaaaaaaa} == self.get_content_type(
    obj=rel_obj, using=instance._state.db
).id

ct_match = {aaaaaaaaaaaaaaaa} and self.get_content_type(
    obj=rel_obj, using=instance._state.db
).id

ct_match = (aaaaaaaaaaaaaaaa) == self.get_content_type(
    obj=rel_obj, using=instance._state.db
).id

ct_match = (aaaaaaaaaaaaaaaa) and self.get_content_type(
    obj=rel_obj, using=instance._state.db
).id
```

We need to spend more time understanding what black's doing to ensure we move closer. One way to assert this is to run the ecosystem checks on our selected projects and verify that either the similarity index increases or that diffing the Black/Ruff differences between the old and new Ruff versions introduces no regressions.

I recommend you to debug Black's [`can_omit_invisible_parens`](https://github.com/psf/black/blob/2fd9d8b339e1e2e1b93956c6d68b2b358b3fc29d/src/black/lines.py#L881-L963), [`delimiter_split`](https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L999-L1079), and [`rhs`](https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L551-L574) to better understand what happening. It can also be useful to debug the selected [selected transformers](https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L610-L621). 

* `rhs` -> `in_parentheses_only_*` helpers 
* delimiter` -> Normal groups

We can also hop on a call and do this together. It ca

---

_@konstin reviewed on 2023-08-04 07:51_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:443 on 2023-08-04 07:51_

i don't think so, they should be independent

---

_Comment by @charliermarsh on 2023-08-04 13:46_

Thank you! Will take a look at this again today and review the Black source.

---

_Converted to draft by @charliermarsh on 2023-08-04 13:51_

---

_Label `formatter` added by @konstin on 2023-08-04 14:16_

---

_Comment by @charliermarsh on 2023-08-05 00:38_

I have deduced at least that our `max_priority_count` and `max_priority` are correct and consistent with Black. The issue seems to be this branch, which is returning `true` but doesn't do so in Black:

```rust
// Only use the layout if the first or last expression has parentheses of some sort.
let first_parenthesized = visitor
    .first
    .is_some_and(|first| has_parentheses(first, visitor.source));
let last_parenthesized = visitor
    .last
    .is_some_and(|last| has_parentheses(last, visitor.source));
first_parenthesized || last_parenthesized
```

(Although in Black, this logic is split between a few branches, I think.)

Continuing to dig in...

---

_Comment by @MichaReiser on 2023-08-05 07:51_

> I have deduced at least that our `max_priority_count` and `max_priority` are correct and consistent with Black. The issue seems to be this branch, which is returning `true` but doesn't do so in Black:
> 
> ```rust
> // Only use the layout if the first or last expression has parentheses of some sort.
> let first_parenthesized = visitor
>     .first
>     .is_some_and(|first| has_parentheses(first, visitor.source));
> let last_parenthesized = visitor
>     .last
>     .is_some_and(|last| has_parentheses(last, visitor.source));
> first_parenthesized || last_parenthesized
> ```
> 
> (Although in Black, this logic is split between a few branches, I think.)
> 
> Continuing to dig in...

I think `last` incorrectly points to the `call` expression for 

```python
ct_match = (
    aaaaaaaaaaact_id == self.get_content_type(obj=rel_obj, using=instance._state.db).id
)
```

rather than the attribute. This makes the above-mentioned path to return `true` because a call is parenthesized.

Doing something like this seems to help (do we need to do the same for `Subscript`, similar to call?

```
Expr::Attribute(ast::ExprAttribute {
                range: _,
                value,
                attr: _,
                ctx: _,
            }) => {
                self.visit_expr(&value);
                if has_parentheses(value, self.source) {
                    self.update_max_priority(OperatorPriority::Attribute);
                }
                self.last = Some(expr);
                return;
            }
```

---

_Comment by @charliermarsh on 2023-08-05 15:52_

Ohhh smart. I didn't realize that Black formatted these differently:

```python
ct_match = (
    aaaaaaaaaaact_id == self.get_content_type(obj=rel_obj, using=instance._state.db).id
)

ct_match = aaaaaaaaaaact_id == self.get_content_type(
    obj=rel_obj, using=instance._state.db
)
```

---

_Comment by @charliermarsh on 2023-08-05 16:01_

It looks like on `main`, we already break this differently than Black ([Ruff](https://play.ruff.rs/?secondary=Format#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsovYAMleQbC1QW5ooPPIDxJx4upAADtIAzABGFwxXL4WDQADWKFWGGguHSWz4eEEYFAEFEMj+GH4nFemR4hEGunwcGKsVExEIPx+mWIxAw4iksiUGlRpNg5IuUCpNLpDPSuFmBKJuH4GhJZJQFKgcEQSCKTzhmjhuGkks50u5kDGGVmRHgsg6PGkP3QGuKbAAvr1wVCYRgyPC0HjdOiMZAlko0Dg0OgMDwJD9GZxMmUMurzhiPQB6QM-UHR0QxhSKONBxNJyAxv1yGPEJYSO7Rh6iLBLFBYSEB03++JYFA-ZG4C0oa22iHQ2EcDRkFBKN3atopKSELYUWZYOWopjtsGdh0cbh8ASD6OQJe8fgM3WZabEM4MRhz0R2rvYBBBzIVgSfVd0d2iOXIENlbDTjBIPFLDDQ6RIDJMFZSQkh4dkpRPKAzwdHA-mUDQlkPR8oERTgG3UbRvgMEJODCABhBA4NSRCAB0SOIEwDDIgw8LIsIyIosIQjItBgAOK0DAwFi2KtMJvEzTF+AwOo1A+PZdBcSDIGg2EBkUCRiAlB9tQkaAtEVZQFjmJJFhWOhJIgG153tWEAEdCAQRJUWQnVcGmIUMHMyyqG+NAEEICcBKgCRCC3ezZicqzXPczyZTEBAsAPTSNEciygrocKPMoLyYDkBBOEwKgpzNXQeEUQg20MjsTKKEp8DXD1d1mIYJBcqMsyMVToS80QjGgYgXTKFqoCMTIfghBtusgIw6hKNBqBlVrRtgNAjD9UlfLAoajBqlAjGWhRYAK9buWPIrjPPPg0FkTggwyJblPXJ1cHUEpoD4ORZlOn5ztRX4lFAibtWuoUgJ+Tg3StKSZJrM1sArKtxQqyk8rQw8OS5dd4hQINXgkBBxEoGduVEE1-oWYsMVLKB8twPhaowFBNsIe7-SdYpsAhek6qPMLSfJ2YqbgGnEkwP0cEUe6MhnPawCM08F1hDQXkSBJoagjoyjvDBVuxpMeqG6WeFl8pJpQrWdcJiBib6ApakV5sVegWqReByWcSDaYsDxDSL1wKckO1Z8FR8vyZly-LCrF4rz2e863cesm72sr3pnalnkNEH5nXa74fjQIa-iURRkDT6BM8QHhYGkAA6eJaSoN6fh0PXIGKcgMhbBLiFXWuCmIDJwVzhKeHK2vcEkP5vlwBNa9Unh+ss6YyG+R2hvHyei84MgS7+Re094IaeEhfhEkUb5t8zhAIUUKvYEz+ESjTlta7gLZQ2+O+hqWY+EDkTgUCQN6ljkI2xbtkqfwrKvAPNIZKl0PR1ASESWYydMhkwrMUVEeUCphTgdbF4yg9hWxtqDfYkBCT9SyGgpQGDYbYOplQPB3x9y621Og2q5DZiUIZDnBUeN8GEOSmzaAnAkG3XMqGWY49yzCQyNkHGUAABC8cACiWgGxNjvENeRijmxDQAGrcxQLIxQOd961wAPIAGVdH6KGgASUMWY8RtdZG4HfjnXAtUyY2IMZIuuEVoQ8DLno8Ru0eF8KocOARXBMgYBET+fA4jmhhVUooascDQKIJZigoO4soL2w6CQf0wEXEXTRNqKq8glAqE6jglmCN0m9D4edeW0kMjoSQILYMcAVBINbGFaJih0IfAWGofynTtR9I0AMmYlMFFbQ+CiSYHocASDIOM9qDsXqKAKVUsKxB+ouzwAGQW3BxQXikPnOgaSwqAQGI6WQHCA6oO1N03p4cvzLCEsUJWMy6AbO1FOBmxRbwPX2F89cDzZg8AQAGQiszRCQnSEgG6dRT6vDgSaKFUAYXIBui8xQmBkWRlZtqdFcK5gRTgGImaAxUWm0FoqFO2K5jL0FooPFOR1x7nupwR6KzXoYAufvBKRA1mINeGCxmCAkF0NZSgQRmQRyrIKSyuZTMOmxO+SiLYZMRZhVTCoCcmrtTpHpUKBkfZun7AVVMGYDJoD4D3lytZqIMCAjCv5Y1LxUBJDwaiFwXTGn+mKOgxIlK3mW15RI9W0kxwwKGgeWlmBp6MvVG3JYoYcVKBRbXBFB4MC4qGogKcsAyV+n3gE4ZlZmzWSBvtTEWApx9nqapdS8yiFaDxHi4EUkzQ-FhDVKGEDRDFNVmGrMxQeAAFVR4eMSEoAAIhi6NLxx14SVVvfsihZ1wqXe1L64aR3joALIY18sQydq7124APZjY94b2rSHdsYhdE7r3EFvVgAAKqeudtdd0-HfQead9184Zt4VtTIqjGzqNrogBYe7K7QGlvPNS07OD4HKiW9cWAlWMJfnzSsGQhanyDUiNCWGMaMlw4LMFBHmgdukL6KgJpwGFPXMUt+AwoMxS2MQas8z8ngU1DRtykUGP7BssU-meHKN6vXLSQiAwTRkYFvh22VaoB-GgjHJG-RCjSyFMoSKFQaP2U9uub2ES4NoRKVtUFtY1bDrykNMg0ggpoY9A2x0gt3aIN0ICAATHEtSt1CSDFRAANn8+pVkqIACs4WaWJF47oKLBlg65D+NSDQgs-T1OhI2dQxpTrWfxkpMAVTrQgCtJESrZRGr3XLGAAAvGAAw3JgDQDa+1jrnXoBWga41k4+AS5ayMDgMmoEjAcPoAgMgAArZgYA9xGCm7NsAJBxRze3OqhsJcjAxsSCXNAZBGAlwyiAMIIAgA), [Black](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ADRAI5dAD2IimZxl1N_WlXr_Won_jHR0459liMGgqSxtGjU7K9xnyRQP2P9EtkHSrDKKSqfo6CEAF9uPoda_yay0XPHticc7Lg3m4fl2ImyPM8zHsHqjgAsMVdmiRr2obXjn01ZpBqulTX9cgcwicxkUVTF-GWp0sfjX53-CLNE-JrMbcxAy0uZe8nlNLUt83CecQAAAACnGWVgY12pGwABqgHSAQAAwNQty7HEZ_sCAAAAAARZWg==)):

```python
ct_match = (
    {aaaaaaaaaaaaaaaa} == self.get_content_type[obj, rel_obj, using, instance._state.db].id
)
```

Is that covered in the same place, or elsewhere? I feel like it's elsewhere, since it's not about removing the optional parentheses, but about which expression we break (left-vs-right).

---

_Marked ready for review by @charliermarsh on 2023-08-05 16:06_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-05 16:07_

---

_Comment by @charliermarsh on 2023-08-05 16:15_

It seems like Black prefers splitting the expression on the RHS of a binary operator, but will split the left if it can't ([playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AIbALRdAD2IimZxl1N_WlXr_Won_jHR0459liMGgqSxtGjU7K9xnyRQP2P9EtkHSrDKKSqfo6CEAF9uPoda_yay0XPHticc7Lg3m4fl2ImyPM8zHsHqjgAsMVdmiRr2obXjn36TRB6v_QvPcvW2NAfeNQQ7F_gemNTO6sToyemxASSojoPt2ydKu3L8igyKcmRYTPfUBlE51MPp6MyjErkIbEt5SbRiXvHN_pbaTZJTlzZFClvkk6RSAAAF7HZ3o3O1ZgAB0AGcBAAAgheUHLHEZ_sCAAAAAARZWg==)), but hesitant to keep investigating this here because it seems separate and I know you've spent more time on binary operators.

---

_Comment by @MichaReiser on 2023-08-05 16:42_

Is it specific to subscript operations? It would be good to understand in depth which expressions we break differently.

We implement a similar "Prefer splitting parenthesized expressions before binary expression" logic [internal discord](https://discord.com/channels/1039017663004942429/1136756271228399727/1136927427248001024). Or it is because our groups have the wrong hierarchy:

* group sequence: `group1, group2`: The right group expands before the left
* Group hierarchy: `group1(group2)`: The outer group expands before the inner.

---

_Comment by @charliermarsh on 2023-08-05 16:44_

Not specific to subscripts, I'll look into the group hierarchy, thanks!

---

_Comment by @charliermarsh on 2023-08-05 16:46_

(We do seem to have a "group sequence" as described above. Gonna keep looking into it, I need to understand this better -- but I do think it can be done separately from this PR.)

---

_Comment by @MichaReiser on 2023-08-05 17:54_

Separate PR makes sense. Can you include the similarity index changes in your test plan 

---

_Comment by @charliermarsh on 2023-08-05 19:13_

Yes sir!

---

_Comment by @charliermarsh on 2023-08-05 19:23_

Done, very small improvements in three projects.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 09:13_

Can you test if we need the same for `Subscript`? Because it has the same pattern or is it not required because we never want to break subscripts?

---

_@MichaReiser approved on 2023-08-07 09:13_

---

_@charliermarsh reviewed on 2023-08-07 15:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 15:55_

I did look into this but perhaps I didn't understand what you're referencing. Are you referring to a subscript with a trailing attribute, like `foo[bar].baz`? Or a subscript following a function call, like `foo(bar)[baz]`? I believe I added test coverage for both, and both are consistent with Black.

---

_@MichaReiser reviewed on 2023-08-07 16:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 16:00_

This PR fixes 

```python
ct_match = aaaaaaaaaaact_id == self.get_content_type(
    obj=rel_obj, using=instance._state.db
)
```

Where the last item is a call chain. The fix was to correctly set `last` to the call because the callee is to the left of the call. This fixed  `has_parentheses` and `has_own_parentheses` to return `true`. 

Now, we have a similar situation with subscript where we visit the value to the left of the subscript. Meaning `a[b]` ends with the subscript and not the node `a` (the subscript should be last). 

Now, whether it should apply that layout is a different question, but it currently seems inconsistent because `has_parentheses` returns `true`, but we don't set the subscript to the last node. 

---

_@MichaReiser reviewed on 2023-08-07 16:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 16:02_

Black output:

```python
ct_match = (
    self.get_content_type["obj=rel_obj, using=instance._state.db"] == aaaaaaaaaaact_id
)

ct_match = (
    aaaaaaaaaaact_id == self.get_content_type["obj=rel_obj, using=instance._state.db"]
)

```

So black prefers not to break subscripts. Can we remove the `Subscript` case from `has_own_parentheses`?

---

_@charliermarsh reviewed on 2023-08-07 16:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 16:25_

Will investigate, thank you for clarifying.

---

_@MichaReiser reviewed on 2023-08-07 16:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:409 on 2023-08-07 16:46_

We can merge this in the meantime. Maybe update the title?

---

_Renamed from "Avoid omitting parentheses for compare expressions with calls" to "Avoid omitting parentheses for trailing attributes on call expressions" by @charliermarsh on 2023-08-07 17:18_

---

_Merged by @charliermarsh on 2023-08-07 17:18_

---

_Closed by @charliermarsh on 2023-08-07 17:18_

---

_Branch deleted on 2023-08-07 17:18_

---

_Comment by @charliermarsh on 2023-08-10 15:13_

I'm realizing that this didn't actually fix the whole issue. Given

```python
def f():
    return (
        unicodedata.normalize("NFKC", s1).casefold()
        == unicodedata.normalize("NFKC", s2).casefold()
    )
```

That's stable formatting for Black, but we do:

```python
def f():
    return unicodedata.normalize("NFKC", s1).casefold() == unicodedata.normalize(
        "NFKC", s2
    ).casefold()
```

---

_Comment by @charliermarsh on 2023-08-10 15:20_

It looks like Black treats these differently depending on whether the function call contains any arguments.

https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AItAJ1dAD2IimZxl1N_WmXWfJWiTYLz4-Xr1ocavDZNy3yj_TrA725dHDb4iaUJISuadfhoT0sKHZLLk1QxHmGNY5xrrWDt1EKt5lq1gq5h7gVq9AKcRwEZ1BmvCk8Yz_Mr7D5lfjC0kkK4Ijl0mefy8pKhR554SYnrYdRZHw5TzgSkSgI8eJOYrTthc6A1exOcCkfWBuyxxInO8Zc12LdP7gAAAAAAIqEgVX6QfxMAAbkBrgQAADp5TWmxxGf7AgAAAAAEWVo=

---
