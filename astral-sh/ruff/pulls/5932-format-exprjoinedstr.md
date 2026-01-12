```yaml
number: 5932
title: format ExprJoinedStr
type: pull_request
state: merged
author: davidszotten
labels: []
assignees: []
merged: true
base: main
head: format-expr-joined-str
created_at: 2023-07-20T22:24:19Z
updated_at: 2023-08-03T09:12:56Z
url: https://github.com/astral-sh/ruff/pull/5932
synced_at: 2026-01-12T15:55:20Z
```

# format ExprJoinedStr

---

_@davidszotten_

reuses most of the (constant) string formatting, adding ability to skip changing quote style, if that would require escaping quotes inside formatted values which isn't allowed (in py < 3.12)

fixes #5913

---

_Comment by @github-actions[bot] on 2023-07-20 22:56_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.26ms     4.0 MB/sec    1.01     10.2±0.35ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1986.8±66.24µs     8.4 MB/sec    1.02      2.0±0.12ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00   225.8±19.42µs    13.1 MB/sec    1.00    225.3±7.48µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.11ms     6.0 MB/sec    1.01      4.3±0.17ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.22ms     2.9 MB/sec    1.02     14.1±0.29ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.07ms     4.8 MB/sec    1.02      3.5±0.08ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   487.3±17.97µs     6.1 MB/sec    1.02   499.2±26.02µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.18ms     4.0 MB/sec    1.02      6.4±0.27ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.17ms     5.5 MB/sec    1.03      7.6±0.15ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1545.6±56.34µs    10.8 MB/sec    1.02  1581.0±62.14µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±6.65µs    16.4 MB/sec    1.04   186.7±11.27µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.10ms     7.8 MB/sec    1.03      3.4±0.10ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.09ms     4.0 MB/sec    1.11     11.2±0.11ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1939.5±18.75µs     8.6 MB/sec    1.10      2.1±0.04ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    211.9±4.02µs    13.9 MB/sec    1.07    227.3±5.91µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     6.0 MB/sec    1.11      4.7±0.07ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.11ms     3.0 MB/sec    1.00     13.6±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.05ms     4.6 MB/sec    1.00      3.6±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.6±5.10µs     6.8 MB/sec    1.00    438.5±7.65µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.09ms     4.2 MB/sec    1.00      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.05ms     5.4 MB/sec    1.00      7.5±0.10ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1524.8±14.55µs    10.9 MB/sec    1.00  1531.8±26.14µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.2±2.34µs    17.2 MB/sec    1.00    171.1±2.54µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.00      3.3±0.03ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:29 on 2023-07-21 07:08_

You can use `f.join()` to join elements without a separator (assuming that's what we need). We should look into how black splits long fstrings (best to include examples where the f-string is used directly in an ExpressionStatement and once where it is inside of parentheses).

Does python allow comments inside of string expressions or is this only a JavaScript madness?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 07:08_

Would it be possible to re-use `FormatStringPart` and add a new field of type `StringKind` that has two variants `Default` and `FString` where the `FString` variant stores the quotes used by the `FString`? 

Re-using `FormatStringPart` has the benefit that it normalizes the string content. 

---

_@MichaReiser reviewed on 2023-07-21 07:09_

Wow. Thanks for being so bold in tackling an as complicated formatting as f-strings. 

---

_@davidszotten reviewed on 2023-07-21 07:33_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:29 on 2023-07-21 07:33_

```
>>> f'asd{#foo}'
  File "<stdin>", line 1
    f'asd{#foo}'
                ^
SyntaxError: f-string expression part cannot include '#'
```

at least not i py3.11

---

_@davidszotten reviewed on 2023-07-21 08:01_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 08:01_

maybe i'm misunderstanding your suggestion, but `FormatStringPart` formats "entire" strings. here we only need the string "decorations", (prefixes and quotes)

---

_@davidszotten reviewed on 2023-07-21 08:03_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:29 on 2023-07-21 08:03_

atm i'm (or maybe this isn't the best approach?) focused on getting the test suite to run, ie on producing output that's 1) valid python and 2) stable

(thanks for `f.join()`)

---

_@MichaReiser reviewed on 2023-07-21 08:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 08:16_

Oh I see. I think copy-pasting is fine. The *hard* part is hidden behind `StringPrefix` and `StringQuotes`. Will it be necessary to determine the preferred quotes as well or do f-string always use the configured quote style?

---

_@davidszotten reviewed on 2023-07-21 08:26_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 08:26_

> The hard part is hidden

👍 

> Will it be necessary to determine the preferred quotes as well or do f-string always use the configured quote style?

sorry i don't understand your question

possibly related: where i left this pr last night was with nested quotes outstanding. i haven't yet looked at how this is is handled in regular strings, but i suspect we need something similar

---

_@davidszotten reviewed on 2023-07-21 08:41_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 08:41_

oh, interesting. this is about to change with python 3.12 (scheduled for this October) https://peps.python.org/pep-0701/ after which _any valid python expression can be used, so in particular, using the same quotes becomes ok

---

_@MichaReiser reviewed on 2023-07-21 09:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 09:09_

> sorry i don't understand your question

Black prefers using `"` over `'` but not if changing the quotes would require more escape sequences:

```
'abcd' -> "abcd"
'Hy "Micha"' -> remains unchanged, because "Hy \"Micha\""` is worse 
```


The question is. Does this also apply to strings and if so, does it apply to the whole string or only to individual parts?

---

_@konstin reviewed on 2023-07-21 09:22_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 09:22_

> oh, interesting. this is about to change with python 3.12 (scheduled for this October) https://peps.python.org/pep-0701/ after which _any valid python expression can be used, so in particular, using the same quotes becomes ok

I'd start with the pre 3.12 syntax first (and an assertion that quotes match the older expectations). We can add support for 3.12 in a second step, where we also need to check the python version the user has set. 

---

_@davidszotten reviewed on 2023-07-21 10:19_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 10:19_

>  start with the pre 3.12 syntax first 

yep makes sense and already the plan

> Does this also apply to strings and if so, does it apply to the whole string or only to individual parts?

do you mean _f -strings?

my understanding so far is that pre 3.12, it works exactly the same because from a quoting perspective the contained FormattedValues don't change things (i.e. the quote rules can be treated as if there are no curly brackets in the f-string. what i'm trying to figure out atm is how to implement this since, unlike regular strings, the inner strings (inside curlies) don't "know" that they are contained in outer strings. i've only just started looking at it, but ideas are welcome

---

_@MichaReiser reviewed on 2023-07-21 12:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 12:01_

> do you mean _f -strings?

yes, I meant f-strings. Grammarly enjoys removing the `f-` from f-string. 

> my understanding so far is that pre 3.12, it works exactly the same because from a quoting perspective the contained FormattedValues don't change things

Does that mean that Black computes the most common quote across all string parts and then normalizes each part?

>  what i'm trying to figure out atm is how to implement this since, unlike regular strings, the inner strings (inside curlies) don't "know" that they are contained in outer strings. i've only just started looking at it, but ideas are welcome

You can implement `FormatRuleWithOptions` on `FormatExprConstant`  and pass down a custom option (layout) telling it it is inside an  f-string. 

---

_Converted to draft by @MichaReiser on 2023-07-21 12:01_

---

_@davidszotten reviewed on 2023-07-21 12:31_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 12:31_

> Does that mean that Black computes the most common quote across all string parts and then normalizes each part

from some more testing:

```
>>>  f"{'\"'}"
SyntaxError: f-string expression part cannot include a backslash
```

maybe this means we can't normalize at all (and don't need to)


> You can implement FormatRuleWithOptions on FormatExprConstant 

ah yes. thanks

---

_@davidszotten reviewed on 2023-07-21 13:59_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 13:59_

actually maybe this isn't enough (unless i'm missing something). because the nested string might not be the _immediate child, e.g. in `f"{foo('')}"` .  could we set something in the `PyFormatContext` while inside f-strings?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 15:02_

We could, as a last resort, I try to avoid the context when I can because:

* You need to be extremely careful to correctly restore to the previous value (even if an error occurred)
* It complicates error recovery (needs to be snapshotted). This isn't something we do today, but may want to do in the future (requires an error-resilient parser)
* I find it much harder to reason about: Who sets the value? Who reads the value? Is it still used? 

---

_@MichaReiser reviewed on 2023-07-21 15:02_

---

_@davidszotten reviewed on 2023-07-21 15:52_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-21 15:52_

what's the alternative? add an attribute to all expressions that contain other expressions and make sure it's propagated during formatting? will try that out too if you think that's got legs? also open to other ideas? 

---

_@MichaReiser reviewed on 2023-07-24 07:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-24 07:45_

Yeah, we could pass something like an `ExpressionContext`, that also stores the group id from `NodeLevel::Expression` or whether it is parenthesized. I considered exploring this at some point to see how cumbersome it is/if it improves readability. It wouldn't be necessary to route the context through to all expressions. It is only necessary to pass it through to expressions that contain child expressions or nodes that are e.g. strings that explicitly need the information

I would recommend exploring this in a separate PR

---

_@davidszotten reviewed on 2023-07-24 08:36_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-24 08:36_

sure, will take a look (might come back here and ask for more pointers)

also (in case it ends up related), the "no backslashes inside f-strings" is slightly more subtle than i originally noticed. it's only `ExprFormattedValue`s that can't have backslashes. (i.e. `f" \' {foo}"` is ok) so i think this means the string normalisation now needs to know if there are quotes inside any contained `FormattedValue`s since we can't change the outer quotes if that would require escaping inside a formatted value

---

_@davidszotten reviewed on 2023-07-27 10:28_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-27 10:28_

ok, back to looking at this, could you elaborate on the suggestions of passing an `ExpressionContext` please? not sure i quite understand what you had in mind. 

---

_@MichaReiser reviewed on 2023-07-29 08:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-29 08:46_

Let's go with your initial proposed context-based implementation and see how it turns out. We still have the option to refactor later.

---

_@davidszotten reviewed on 2023-07-29 13:38_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-29 13:38_

oki

related to this, i think i also need to walk all children of some expressions to see if any of them are `FormattedValue`s with quotes inside of them (which we can't escape, so we can't change outside quotes). do we have any helpers to walk expressions and all their children?

---

_Comment by @davidszotten on 2023-07-30 07:20_

oh. not sure how i managed to miss this, but black [doesn't (yet) format expressions inside f-strings](https://github.com/psf/black/issues/567)



---

_Comment by @davidszotten on 2023-07-30 14:28_

so this approach works.. not sure what we think of it though

note that `FormattedValue` isn't used at all with this impl and none of my related refactorings are needed (though i think we still want them). That will all be needed when we decide to start formatting things inside formatted values

---

_Marked ready for review by @davidszotten on 2023-07-30 14:29_

---

_Review requested from @MichaReiser by @davidszotten on 2023-07-31 09:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:40 on 2023-07-31 11:52_

Nit: To avoid iterating over the string twice
```suggestion
                        string_content.contains(['"', '\''])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:26 on 2023-07-31 11:54_

```suggestion
#[derive(Copy, Clone)]
enum Quoting {
    CanChange,
    Preserve,
}
```

---

_@MichaReiser approved on 2023-07-31 12:01_

---

_@MichaReiser reviewed on 2023-07-31 12:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-31 12:04_

Not a very good one... You must implement your own `PreorderVisitor`. I would love to have a `children(node)` method that returns an iterator. That method would probably need to allocate a stack internally to queue the next children but it is so much more ergonomic than having to implement a visitor.

---

_@davidszotten reviewed on 2023-07-31 12:59_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_joined_str.rs`:18 on 2023-07-31 12:59_

note that this wasn't needed in the end. immediate children is enough

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/string.rs`:40 on 2023-07-31 13:00_

ah thanks, was hoping something like this existed but couldn't find it

---

_@davidszotten reviewed on 2023-07-31 13:00_

---

_@davidszotten reviewed on 2023-07-31 13:00_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/string.rs`:26 on 2023-07-31 13:00_

open to better names (of the enum) btw, there are lots of related things around quoting behaviour

---

_@MichaReiser reviewed on 2023-07-31 13:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:26 on 2023-07-31 13:56_

I like `Preserve` and tried to come up with a better name than `CanChange` but failed. I thought about `Normalize` to align with `normalize_string` but this isn't disabling normalization, it only affects the quotes. 

---

_@davidszotten reviewed on 2023-07-31 13:58_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/string.rs`:26 on 2023-07-31 13:58_

i meant more to replace `Quoting` (since we already have `StringQuotes` and `QuoteStyle`)

---

_Comment by @davidszotten on 2023-07-31 14:50_

hey @konstin did your recent ecosystem test pr also change (fix) it failing on errors? it looks like the most recently merged pr after that https://github.com/astral-sh/ruff/pull/6152 fails the checks (i was trying to figure out why i'm failing after rebasing onto main)

---

_Comment by @konstin on 2023-07-31 16:29_

yes, sorry, fixed in https://github.com/astral-sh/ruff/pull/6202. Part of the problem is that i forgot to make the formatter ecosystem checks required for merging (but i don't think github properly shows in the PR UI that we could still merge your PR even with that check failing).

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string.rs`:26 on 2023-08-01 06:23_

I think it's fine. The use is very local and it conveys way more information than a plain boolean.

---

_@MichaReiser reviewed on 2023-08-01 06:23_

---

_Comment by @MichaReiser on 2023-08-01 06:26_

This is awesome. Are you interested in looking into "proper" f-string formatting? It isn't as high a priority to us as increasing coverage on other nodes (see [August goals](https://github.com/astral-sh/ruff/issues/6203)), but it would be a real differentiator to black 

---

_Merged by @MichaReiser on 2023-08-01 06:26_

---

_Closed by @MichaReiser on 2023-08-01 06:26_

---

_Comment by @davidszotten on 2023-08-01 06:48_

>  looking into "proper" f-string formatting

sure. do we need some mechanism to opt out of this behaviour for all the tests that are comparing to black?

---

_Comment by @MichaReiser on 2023-08-01 07:03_

> > looking into "proper" f-string formatting
> 
> sure. do we need some mechanism to opt out of this behaviour for all the tests that are comparing to black?

Are you expecting many differences? If not, then I don't think it's worth bothering. We can otherwise make it an option on` PyFormatOptions` which we already support loading for tests. 

---

_Comment by @davidszotten on 2023-08-01 19:06_

i thought i'd seen a bunch but maybe i'm misremembering

i started looking (maybe we can make an issue to have these discussions there instead) and came across something to run by you:

currently `f"{{"` is parsed as 

```
ExprJoinedStr {values: [Constant(ExprConstant {
    value: Str(
        "{",
```

ie the escaped curly is rendered into the constant. is that what we want? if so i guess we need special handling when rendering to reproduce the escaping. or do we want to parse as something closer to the source and store the string `"{{"` instead?

do we do anything corresponding for regular string escapes like `'\"'`?

---

_Comment by @MichaReiser on 2023-08-03 09:12_

A discussion would be good because I tend to not look at merged PRs and the discussion also goes beyond of what this PR addressed. 

Generally, it is very common for ASTs to undo string escapes, because the goal is to represent the semantic of the program and not the representation of the program in the source (this is different to a CST that preserves the code as is). 

You can see that Ruff's parser removes the escape sequences of quotes too: [Playground](https://play.ruff.rs/?secondary=AST#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZXICkQAOSQCAA). I assume this happens inside of `parse_string` because the lexer isn't performing this operation (But I think it would be more appropriate for the lexer to perform this, but that requires Python's 3.12 f string parsing)

---
