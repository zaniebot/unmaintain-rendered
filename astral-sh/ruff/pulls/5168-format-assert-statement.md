```yaml
number: 5168
title: "Format `assert` statement"
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: stmt-assert
created_at: 2023-06-17T22:09:41Z
updated_at: 2023-07-14T08:00:12Z
url: https://github.com/astral-sh/ruff/pull/5168
synced_at: 2026-01-12T03:30:21Z
```

# Format `assert` statement

---

_Pull request opened by @cnpryer on 2023-06-17 22:09_

## Summary

Format `assert` statement with `Parenthesize::IfBreaks` for test and message expressions.

## Test Plan

Add ruff fixture with comments.

There's some coverage already as well including:

- `assert left > right`
- `assert test, msg`

I'll try to leave the *good first issue* ones alone now :)

This looks really good btw.

-----

TODO (required):
- [x] Ruff fixture
- [x] Formatting [`test`'s value](https://github.com/astral-sh/ruff/pull/5168#issuecomment-1631148612)
- [ ] Formatting [`msg` long strings](https://github.com/astral-sh/ruff/pull/5168#issuecomment-1630767421) UPDATE: [comment](https://github.com/astral-sh/ruff/pull/5168#discussion_r1262709467)
- [x] Updated snapshots

TODO (extra):
- [ ] Handling [misplaced trailing own-line comments](https://github.com/astral-sh/ruff/pull/5168#issuecomment-1630767421) UPDATE: [comment](https://github.com/astral-sh/ruff/pull/5168#discussion_r1262740871)

---

_Comment by @github-actions[bot] on 2023-06-17 22:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.29ms     4.2 MB/sec    1.17     11.5±0.40ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.10ms     7.5 MB/sec    1.05      2.3±0.12ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   252.9±19.15µs    11.7 MB/sec    1.03   259.4±21.49µs    11.4 MB/sec
formatter/pydantic/types.py                1.00      4.9±0.22ms     5.2 MB/sec    1.02      5.0±0.24ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     17.2±0.47ms     2.4 MB/sec    1.00     17.1±0.50ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.17ms     3.9 MB/sec    1.00      4.3±0.17ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   566.5±38.74µs     5.2 MB/sec    1.03   582.7±38.43µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.23ms     3.3 MB/sec    1.01      7.7±0.26ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.04      8.6±0.25ms     4.7 MB/sec    1.00      8.3±0.25ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1826.7±67.86µs     9.1 MB/sec    1.00  1791.5±74.12µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   217.0±12.09µs    13.6 MB/sec    1.00    216.2±8.04µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.13ms     6.7 MB/sec    1.00      3.8±0.14ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.8±0.49ms     3.4 MB/sec    1.18     13.9±0.61ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.7±0.18ms     6.2 MB/sec    1.01      2.7±0.16ms     6.1 MB/sec
formatter/numpy/globals.py                 1.00   306.9±37.56µs     9.6 MB/sec    1.04   320.2±25.27µs     9.2 MB/sec
formatter/pydantic/types.py                1.00      5.9±0.33ms     4.3 MB/sec    1.00      5.9±0.26ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.00     20.2±0.71ms     2.0 MB/sec    1.01     20.3±0.74ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.4±0.33ms     3.1 MB/sec    1.00      5.2±0.23ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   628.9±42.79µs     4.7 MB/sec    1.00   624.2±27.82µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.2±0.41ms     2.8 MB/sec    1.00      9.0±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     10.2±0.36ms     4.0 MB/sec    1.00     10.1±0.43ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.7 MB/sec    1.02      2.2±0.11ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.02   255.9±15.95µs    11.5 MB/sec    1.00   250.2±15.34µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.5±0.25ms     5.6 MB/sec    1.00      4.5±0.17ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @cnpryer on 2023-06-18 02:05_

Hmm maybe there's more here. This isn't formatting the same as black.

```py
assert (
    # Some comment
    True  # Some comment
    # Some comment
), "Some string"  # Some comment
# Some comment
```

---

_Converted to draft by @cnpryer on 2023-06-18 02:05_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/assert.py`:1 on 2023-06-18 11:26_

these test cases are copied from black, please add new test cases to `crates/ruff_python_formatter/resources/test/fixtures/ruff` instead

---

_@konstin reviewed on 2023-06-18 11:27_

---

_Label `formatter` added by @konstin on 2023-06-18 13:35_

---

_@cnpryer reviewed on 2023-06-18 15:23_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/assert.py`:1 on 2023-06-18 15:23_

d780f2f8fa6dba9234e9b9301e1eae2fddddf5cd

---

_Comment by @cnpryer on 2023-06-21 12:15_

Also note if you format this with black `# First inner comment` is lost
```py
# Some assert
assert (
    # First inner comment
    (1 + 1)  # Test's trailing comment 
    # Below the expression
    # Last inner comment
), "Some string"  # Message's trailing comment
# Below the assert
```

I reported this [here](https://github.com/psf/black/issues/638#issuecomment-1597665572) for now. Probably should be a new issue if that one it's mentioned in is not updated. I'll get back to this.


---

_Comment by @cnpryer on 2023-07-09 19:56_

I find this snapshot result odd, but it could just be my inexperience with `insta`.
```
Reviewing [1/11] ruff_python_formatter@0.0.0:
Snapshot file: crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap
Snapshot: expression
Source: crates/ruff_python_formatter/tests/fixtures.rs:86
Input file: crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/expression.py
────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────
  391   391 │ -    str,
  392   392 │ -    int,
  393   393 │ -    float,
  394   394 │ -    dict[str, int],
        395 │+-]
        396 │+-very_long_variable_name_filters: t.List[
        397 │+-    t.Tuple[str, t.Union[str, t.List[t.Optional[str]]]],
  395   398 │ +    (
  396   399 │ +        str,
  397   400 │ +        int,
  398   401 │ +        float,
  399   402 │ +        dict[str, int],
  400   403 │ +    )
  401   404 │  ]
  402       │--very_long_variable_name_filters: t.List[
  403       │--    t.Tuple[str, t.Union[str, t.List[t.Optional[str]]]],
  404       │--]
  405   405 │ -xxxx_xxxxx_xxxx_xxx: Callable[..., List[SomeClass]] = classmethod(  # type: ignore
  406   406 │ -    sync(async_xxxx_xxx_xxxx_xxxxx_xxxx_xxx.__func__)
  407   407 │ -)
  408   408 │ -xxxx_xxx_xxxx_xxxxx_xxxx_xxx: Callable[..., List[SomeClass]] = classmethod(  # type: ignore
```

---

_Comment by @MichaReiser on 2023-07-10 08:04_

> I find this snapshot result odd, but it could just be my inexperience with `insta`.
> 
> ```
> Reviewing [1/11] ruff_python_formatter@0.0.0:
> Snapshot file: crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__expression.py.snap
> Snapshot: expression
> Source: crates/ruff_python_formatter/tests/fixtures.rs:86
> Input file: crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases/expression.py
> ────────────────────────────────────────────────────────────────────────────────────────
> -old snapshot
> +new results
> ────────────┬───────────────────────────────────────────────────────────────────────────
>   391   391 │ -    str,
>   392   392 │ -    int,
>   393   393 │ -    float,
>   394   394 │ -    dict[str, int],
>         395 │+-]
>         396 │+-very_long_variable_name_filters: t.List[
>         397 │+-    t.Tuple[str, t.Union[str, t.List[t.Optional[str]]]],
>   395   398 │ +    (
>   396   399 │ +        str,
>   397   400 │ +        int,
>   398   401 │ +        float,
>   399   402 │ +        dict[str, int],
>   400   403 │ +    )
>   401   404 │  ]
>   402       │--very_long_variable_name_filters: t.List[
>   403       │--    t.Tuple[str, t.Union[str, t.List[t.Optional[str]]]],
>   404       │--]
>   405   405 │ -xxxx_xxxxx_xxxx_xxx: Callable[..., List[SomeClass]] = classmethod(  # type: ignore
>   406   406 │ -    sync(async_xxxx_xxx_xxxx_xxxxx_xxxx_xxx.__func__)
>   407   407 │ -)
>   408   408 │ -xxxx_xxx_xxxx_xxxxx_xxxx_xxx: Callable[..., List[SomeClass]] = classmethod(  # type: ignore
> ```

The snapshots can be hard to read because it shows a diff of the new to old snapshot containing a diff with the black/ruff formatter differences. 

Overall: 
* Red lines / Lines gone -> Good (not very intuitive)
* More lines with a single `+` or `-` -> Good. It means the output of black and ruff are the same 
* New lines with `++` or `+-`: Potentially a regression, but may be fine. Check out the section below that show the black and ruff output on their own and compare it manually. 


---

_Comment by @cnpryer on 2023-07-10 23:12_

Thanks for the explanation. Yea I that's tough to understand. Manually reviewing it that result looks fine. Intuitively I would have expected no change in the diff here, but I'm sure I'm just missing something.

---

_Comment by @cnpryer on 2023-07-10 23:43_

This is maybe a better example of what's tripping me up. Manually reviewing the new snapshot ruff and black outputs, and comparing them with the original snapshot, I'd expect there to be no new results displayed during the `insta` review because everything seems to be fine. Instead I get:
 

```
black_compatibility@simple_cases__fmtonoff2.py.snap
   71    71 │      pass
   72    72 │  
   73    73 │ +
   74    74 │  def check_fader(test):
   75       │-+    pass
         75 │+-
         76 │+     pass
   76    77 │  
   77       │--    pass
   78       │- 
         78 │++
```

IIUC this result is displaying the same diff just in a different way.

Originally
```
+
 def check_fader(test):
+    pass
 
-    pass
```

And in this result
```
+
 def check_fader(test):
-
     pass
```

---

_Comment by @MichaReiser on 2023-07-11 06:30_

> This is maybe a better example of what's tripping me up. Manually reviewing the new snapshot ruff and black outputs, and comparing them with the original snapshot, I'd expect there to be no new results displayed during the `insta` review because everything seems to be fine. Instead I get:
> 
> ```
> black_compatibility@simple_cases__fmtonoff2.py.snap
>    71    71 │      pass
>    72    72 │  
>    73    73 │ +
>    74    74 │  def check_fader(test):
>    75       │-+    pass
>          75 │+-
>          76 │+     pass
>    76    77 │  
>    77       │--    pass
>    78       │- 
>          78 │++
> ```
> 
> IIUC this result is displaying the same diff just in a different way.
> 
> Originally
> 
> ```
> +
>  def check_fader(test):
> +    pass
>  
> -    pass
> ```
> 
> And in this result
> 
> ```
> +
>  def check_fader(test):
> -
>      pass
> ```

Yeah this is a bit annoying and something that I noticed too. The diff algorithm sometimes gets dripped up and produces changes even if there are non (to this particular section). We could explore if other diffing algorithm help but ultimately the best is to have higher compatibility, which will make the diff algorithm's job easier. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-11 06:32_

Nit: I prefer to keep the shared logic outside of the `if..else` branches. It avoids that the reader has to parse both branches of the if statement to identify what's difference between them
```suggestion
		write!(f, [
			text("assert"),
            space(),
            test.format().with_options(Parenthesize::IfBreaks),
        ])?;
                    
        if let Some(msg) = msg {
            write!(
                f,
                [       
                    text(","),
                    space(),
                    msg.format().with_options(Parenthesize::IfBreaks)
                ]
            )
        }
        
        Ok(())
```

---

_@MichaReiser approved on 2023-07-11 06:33_

This looks ready to merge for me. Is there something that you want to look into before merging?

---

_@cnpryer reviewed on 2023-07-11 12:36_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-11 12:36_

For sure!

---

_Comment by @cnpryer on 2023-07-11 12:48_

> This looks ready to merge for me. Is there something that you want to look into before merging?

Yea a couple of things, but one we can probably follow up on if you want to just get this merged.

First, the `msg` of the `assert` isn't there yet. Here's a related result:

```
   77       │--assert xxxxxxxxx.xxxxxxxxx.xxxxxxxxx(
   78       │--    xxxxxxxxx
         71 │+ assert xxxxxxxxx.xxxxxxxxx.xxxxxxxxx(
         72 │+     xxxxxxxxx
   79    73 │ -).xxxxxxxxxxxxxxxxxx(), (
   80    74 │ -    "xxx {xxxxxxxxx} xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   81    75 │ -)
   82       │-+NOT_YET_IMPLEMENTED_StmtAssert
         76 │++).xxxxxxxxxxxxxxxxxx(), "xxx {xxxxxxxxx} xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

Basically just need long strings causing line-width violation to wrap with parentheses.

The second one is one I personally was looking forward to tackling, but it's probably something I could follow up with after this pass is merged. A trailing own-line comment for the `assert`'s `test` value is assigned as a leading comment for the `msg`. I'd like to handle its placement. So
```py
# Comment
assert (
    # Leading
    True  # Trailing same-line
    # Trailing own-line
), "msg"  # Comment
# Comment
```
currently becomes
```py
# Comment
assert (
    # Leading
    True  # Trailing same-line
), (
    # Trailing own-line
    "msg"
)  # Comment
# Comment
```


---

_@cnpryer reviewed on 2023-07-11 13:36_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-11 13:36_

aaffdac1224d3217bc40371832f0a174e0d2a447

---

_@cnpryer reviewed on 2023-07-11 13:39_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-11 13:39_

Tbh I don't even see this as a nit lol.

---

_Comment by @cnpryer on 2023-07-11 16:40_

Hmm there's more to this than what I mentioned. I added `optional_parentheses` for the formatted `msg`, but what I really need to do is consider the entire statement, formatting `test`'s value first prior to having the `msg` break with parentheses.

```
black_compatibility@simple_cases__expression.py.snap
        488 │+-assert this is ComplexTest and not requirements.fit_in_a_single_line(
        489 │+-    force=False
        490 │+-), "Short message"
        491 │++assert this is ComplexTest and not requirements.fit_in_a_single_line(force=False), (
        492 │++    "Short message"
        493 │++)
        494 │+ assert parens is TooMany
        495 │+ for (x,) in (1,), (2,), (3,):
```

`black` would format `fit_in_a_single_line`, making it unnecessary to futher format the `msg`.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-12 01:59_

I ended up adding a `group`, so I've deviated from your suggestion. I'm still finding my feet with some of this stuff, so feel free to tell me to stop over-implementing this.

I'd want to format this next
```py
# Regression test for https://github.com/psf/black/issues/3414.
assert xxxxxxxxx.xxxxxxxxx.xxxxxxxxx(
    xxxxxxxxx
).xxxxxxxxxxxxxxxxxx(), (
    "xxx {xxxxxxxxx} xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
)
```
And then finally figure out comment placement, but I can rip out the fixture I've added for that and follow up after this is merged.

---

_@cnpryer reviewed on 2023-07-12 01:59_

---

_Comment by @MichaReiser on 2023-07-12 10:03_

I don't have a good answer for you yet but am looking into a similar problem with `with` statements (the optional parentheses there are tricky). 

My understanding from [Black's code](https://github.com/psf/black/blob/d1248ca9beaf0ba526d265f4108836d89cf551b7/src/black/linegen.py#L484-L499) is that `assert` should work the same as for `if` headers. Now, if our formatting of the if header is correct is another question... It may also be that e.g. this is caused by incorrect formatting of `string` (I'm also looking into that)

---

_@MichaReiser reviewed on 2023-07-12 10:06_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-12 10:06_

Is the formatting different from `msg.format().with_options(Parenthesize::IfBreaks)`?

---

_@cnpryer reviewed on 2023-07-12 13:26_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-12 13:26_

Yea I'm definitely not satisfied with it yet.

Without my "hack" here I would get 

```
black_compatibility@simple_cases__composition_no_trailing_comma.py.snap
  349       │-        } == expected, (
  350       │-            "Not what we expected and the message is too long to fit in one line"
  351       │-        )
        360 │+        } == expected, "Not what we expected and the message is too long to fit in one line"
```
`black` will wrap the `msg` (lines 349-351). So IIUC `Parenthesize::IfBreaks` wouldn't give me that alone since the `msg` doesn't break as a long string like that.

> It may also be that e.g. this is caused by incorrect formatting of string

I don't know enough yet but maybe this is possible.

---

_@cnpryer reviewed on 2023-07-12 13:26_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-12 13:26_

> My understanding from [Black's code](https://github.com/psf/black/blob/d1248ca9beaf0ba526d265f4108836d89cf551b7/src/black/linegen.py#L484-L499) is that assert should work the same as for if headers

This could be helpful. I'll check it out. Thank you! 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-12 13:56_

Let's see if https://github.com/astral-sh/ruff/pull/5708 helps. It changed the logic that decides when a string needs optional parentheses.

---

_@MichaReiser reviewed on 2023-07-12 13:56_

---

_@MichaReiser reviewed on 2023-07-13 15:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-13 15:12_

I spent some more time reading black's source code and what I think we miss is that strings have a higher split priority than any math operator. I'm not sure how we want to model this yet but I suggest to not block this PR because of it.

---

_@cnpryer reviewed on 2023-07-13 15:38_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:29 on 2023-07-13 15:38_

Noice. Perfect timing. I just sat down for lunch. Was just about to get back in here.

If you're okay with it, I'd also like to get this merged without the extra comment placement. I think I can follow up on that since I don't think it warrants blocking this either.

---

_Comment by @cnpryer on 2023-07-13 15:55_

Did a skim of the updated snapshots. I can check more thoroughly after work, but for now I'll mark this as ready for a review.

---

_Marked ready for review by @cnpryer on 2023-07-13 15:55_

---

_Comment by @cnpryer on 2023-07-13 15:56_

Hmm. Scratch that.

---

_Converted to draft by @cnpryer on 2023-07-13 15:56_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/tests/snapshots/format@statement__assert.py.snap`:73 on 2023-07-13 16:03_

I thought it'd be better to leave this here instead of ripping it out entirely.

---

_@cnpryer reviewed on 2023-07-13 16:03_

---

_Comment by @cnpryer on 2023-07-13 16:09_

Got it

---

_Marked ready for review by @cnpryer on 2023-07-13 16:09_

---

_@cnpryer reviewed on 2023-07-13 16:12_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-13 16:12_

🎉 709c4a70f6d4ba6ce91d12a71cde94dee34789a9

---

_@MichaReiser reviewed on 2023-07-14 06:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_assert.rs`:38 on 2023-07-14 06:57_

Nice. Yeah, it can sometimes be very subtle.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@miscellaneous__long_strings_flag_disabled.py.snap`:326 on 2023-07-14 07:00_

Ehh, this is interesting. It seems black happily wraps identifiers in parentheses in *some* positions but not all. This is fine for now.

---

_@MichaReiser approved on 2023-07-14 07:01_

Perfect! Thank you so much. This will increase our coverage significantly. 

---

_Merged by @MichaReiser on 2023-07-14 07:01_

---

_Closed by @MichaReiser on 2023-07-14 07:01_

---

_Branch deleted on 2023-07-14 07:09_

---

_@davidszotten reviewed on 2023-07-14 07:47_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@miscellaneous__long_strings_flag_disabled.py.snap`:326 on 2023-07-14 07:47_

i wonder if that's because assert is somewhat unusual in that one must not wrap the entire rhs in parens, i.e. 

`assert foo, "msg"` very much != `assert (foo, "msg")`  

---

_Comment by @konstin on 2023-07-14 08:00_

To add some numbers to "increases coverage", the similarity index (percentage of lines we format like black) went from 80.3% to 92.8% for [warehouse](https://github.com/pypi/warehouse), the software powering pypi

---
