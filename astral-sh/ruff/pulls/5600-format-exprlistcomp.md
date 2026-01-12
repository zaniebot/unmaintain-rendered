```yaml
number: 5600
title: format ExprListComp
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-expr-list-comp
created_at: 2023-07-07T19:05:31Z
updated_at: 2023-07-11T08:03:18Z
url: https://github.com/astral-sh/ruff/pull/5600
synced_at: 2026-01-12T15:55:18Z
```

# format ExprListComp

---

_@davidszotten_

includes `Comprehension`


The jaccard index on django goes from 0.915 to 0.923.

---

_Comment by @github-actions[bot] on 2023-07-07 19:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     10.3±0.51ms     3.9 MB/sec    1.00      9.8±0.33ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.08ms     7.5 MB/sec    1.00      2.2±0.08ms     7.6 MB/sec
formatter/numpy/globals.py                 1.03   258.5±16.54µs    11.4 MB/sec    1.00   252.1±14.26µs    11.7 MB/sec
formatter/pydantic/types.py                1.02      4.9±0.18ms     5.2 MB/sec    1.00      4.8±0.20ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.03     17.2±0.51ms     2.4 MB/sec    1.00     16.7±0.42ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.3±0.17ms     3.9 MB/sec    1.00      4.1±0.14ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   537.6±28.62µs     5.5 MB/sec    1.01   542.4±27.58µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.7±0.30ms     3.3 MB/sec    1.00      7.4±0.26ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      8.6±0.27ms     4.7 MB/sec    1.00      8.3±0.27ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1812.4±62.21µs     9.2 MB/sec    1.00  1780.8±82.95µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.0±10.98µs    14.0 MB/sec    1.02   213.7±13.08µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.11ms     6.8 MB/sec    1.00      3.7±0.10ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.27ms     3.5 MB/sec    1.00     11.6±0.35ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.6±0.10ms     6.4 MB/sec    1.00      2.6±0.11ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   296.6±16.65µs     9.9 MB/sec    1.00   296.2±20.78µs    10.0 MB/sec
formatter/pydantic/types.py                1.02      5.8±0.25ms     4.4 MB/sec    1.00      5.7±0.22ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.01     19.5±0.46ms     2.1 MB/sec    1.00     19.4±0.38ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.15ms     3.3 MB/sec    1.01      5.2±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   631.1±37.31µs     4.7 MB/sec    1.00   624.9±26.40µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.25ms     2.9 MB/sec    1.01      8.9±0.27ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.03     10.2±0.32ms     4.0 MB/sec    1.00      9.9±0.26ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.07ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    248.5±9.73µs    11.9 MB/sec    1.01   250.5±10.75µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.18ms     5.6 MB/sec    1.00      4.5±0.15ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-07-07 19:31_

---

_Marked ready for review by @davidszotten on 2023-07-07 21:07_

---

_Comment by @MichaReiser on 2023-07-10 06:53_

I'm not entirely sure if I understand the Black difference that you're describing. There are two ways (besides what https://github.com/astral-sh/ruff/pull/5608 introduces but this hopefully isn't needed here) to control the priority

* Group sequences `group(one), group(two)`: The Printer expands group `two` before group `one` (`one` has a higher priority)
* Group nesting `group(one, group(two)): The printer expands *outer* groups before inner groups. In this case, the group `one` expands before group `two`. 

You may need to use a group sequence here (one group for the value, one group for the body)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1290 on 2023-07-10 06:59_

Nit: We should assert that this is indeed the `in` token. Is there a possibility that the expression is parenthesized? If so, you may also need to skip over any `)` or`(`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1337 on 2023-07-10 07:01_

Nit: We should assert that this is indeed the expected `if` token and not any other token. You may also need to deal with any `(` or `)` in case the expression can be parenthesized.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1352 on 2023-07-10 07:01_

```suggestion
    CommentPlacement::Default(comment)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_list_comp.rs`:34 on 2023-07-10 07:02_

```suggestion
                    elt.format(),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_list_comp.rs`:33 on 2023-07-10 07:04_

Question: `parenthesized` already adds a group. Is this additional group required or should it maybe only wrap the `value` rather than the value and `joined`? This way the printer would first try to expand `joined`, and only fallback to expanding `value` if the line still exceeds the configured line width.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:30 on 2023-07-10 07:13_

This is necessary because `leading_comments` only format trailing whitespace. This is necessary to avoid that the statement formatting and leading comments insert the same whitespace, resulting in doubling the whitespace on every formatting pass. 

```suggestion
        // TODO: why is this needed?
        if let Some(leading_item_comment) = leading_item_comments.first() {
            if leading_item_comment.line_position().is_own_line() {
                hard_line_break().fmt(f)?;
            }
            leading_comments(leading_item_comments).fmt(f)?;
        }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:51 on 2023-07-10 07:14_

```suggestion
                    target.format(),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:64 on 2023-07-10 07:16_

The trailing underscore reads a bit strange.

Nit: 
```suggestion
                for if_case in ifs {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:94 on 2023-07-10 07:18_

Nit: Can you explain why it is necessary to manually format the leading comments? This should only rarely be necessary.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:78 on 2023-07-10 07:18_

We can override `fmt_dangling_comments` here since you're handling it in the `fmt_fields` function

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:87 on 2023-07-10 07:20_

Can you use `traling_comments` instead? It inserts an empty line for own line comments or renders it as a line suffix with two spaces.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1281 on 2023-07-10 07:23_

Nit: I find it helpful to add some examples above every case:

```
// Comments between the `for` and target
// ```python
//  [a for # trailing for comment
// 	b in c]
// # or 
// [a for
// 	# leading b comment
//		b in c
// ]
// ```
```

---

_@MichaReiser approved on 2023-07-10 07:27_

Nice. Implementing list comprehension was on my TODO list for this week, I guess no more :) Would you mind commenting on https://github.com/astral-sh/ruff/issues/4798 next time before starting to work on a new syntax for better coordination? 

---

_Review requested from @konstin by @MichaReiser on 2023-07-10 07:27_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1290 on 2023-07-10 09:17_

I'd use `.expect("could not find `if` token in list comprehension")`, i think crashing is more reasonable here than trying to format under false assumptions

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_list_comp.rs`:33 on 2023-07-10 09:20_

nit: just `format_args!` (without `self`) after importing it

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_list_comp.rs`:33 on 2023-07-10 09:40_

i'll check, but it might also be a mistake, `parenthesized` landed after i'd written this code and i added it in when i spotted it landing

---

_@davidszotten reviewed on 2023-07-10 09:40_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/expression/expr_list_comp.rs`:33 on 2023-07-10 09:43_

but i should also admit i don't fully grok the rules around expanding and groups yet, and sometimes end up trying a bunch of variations on examples. your explanation in a previous comment above helps a bit though (group sequences and nesting)

---

_@davidszotten reviewed on 2023-07-10 09:43_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/comprehension.rs`:30 on 2023-07-10 09:46_

i was surprised because i hadn't had to do this in any of my previous prs (eg `for`, `try/except`)

---

_@davidszotten reviewed on 2023-07-10 09:46_

---

_@davidszotten reviewed on 2023-07-10 09:47_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/comprehension.rs`:64 on 2023-07-10 09:47_

yes, was struggling a bit with naming, esp in the places where i have both an if-node and an if-statement

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/format@statement__list_comp.py.snap`:53 on 2023-07-10 10:13_

black formats this as 
```python
[
    i
    for i in [
        1,
    ]
]
```
which i prefer, but i expect that to be unreasonably hard to implement (breaking the inner group but not the outer group)

---

_@konstin approved on 2023-07-10 10:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:30 on 2023-07-10 11:20_

I think you did this in your first version before you switched to `leading_alternate_branch_comments`

---

_@MichaReiser reviewed on 2023-07-10 11:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__list_comp.py.snap`:53 on 2023-07-10 11:23_

Hmm interesting. I think this goes into the one case that my expression formatting refactor doesn't cover yet (and I also don't fully understand yet). 

Black sometimes (for example on expression statement level) only expands by parenthesized expressions, but not by operators. But it does sometimes and inserts optional parentheses. I haven't understood the rules around this yet.  It could be related and we may want to wait with addressing it just yet

---

_@MichaReiser reviewed on 2023-07-10 11:23_

---

_@davidszotten reviewed on 2023-07-10 12:53_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:1290 on 2023-07-10 12:53_

oh interesting. i though the brackets would be part of the node. til

---

_@davidszotten reviewed on 2023-07-10 13:33_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/comprehension.rs`:51 on 2023-07-10 13:33_

👍 clippy usually complains about these (but not this one)

---

_@davidszotten reviewed on 2023-07-10 13:39_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/comprehension.rs`:94 on 2023-07-10 13:39_

hm, looks like it isn't (anymore?) 

---

_@davidszotten reviewed on 2023-07-10 15:57_

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/other/comprehension.rs`:87 on 2023-07-10 15:57_

actually this wasn't needed at all in the end

---

_@konstin reviewed on 2023-07-10 16:03_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1290 on 2023-07-10 16:03_

it's really inconsistent unfortunately (it's a particularity of our parser), e.g. tuple parentheses but others aren't are so `((1,2,3))` the range contains the inner parentheses but not the outer while in `(1 + 2)` they are not part of the range at all.

---

_Review comment by @davidszotten on `crates/ruff_python_formatter/src/comments/placement.rs`:1290 on 2023-07-10 18:35_

`find_only_token_in_range` also has this problem. will fix in this pr (and then use that function)

---

_@davidszotten reviewed on 2023-07-10 18:35_

---

_Comment by @davidszotten on 2023-07-10 19:45_

ready i think

---

_Merged by @MichaReiser on 2023-07-11 06:35_

---

_Closed by @MichaReiser on 2023-07-11 06:35_

---

_Comment by @davidszotten on 2023-07-11 06:51_

👍 

I’ll have a look at the other comprehensions next

---

_Comment by @MichaReiser on 2023-07-11 06:54_

> +1
> 
> I’ll have a look at the other comprehensions next

Nice. Do you have the permissions to create an issue from https://github.com/astral-sh/ruff/issues/4798 and assign it to you? If not, then I'll create it and assign it to you for you, if you let me know on which comprehension you want to work first.

---

_Comment by @davidszotten on 2023-07-11 07:41_

i don't think I have any repo permissions. i guess i can create issues (not sure what you by by creating "from 4798", do you mean and link from there?) but not assign 

from a quick look, set and dict are pretty much like list comps so seem quite straightforward. generator comprehensions are trickier because the brackets are optional in some contexts but mandatory in others

---

_Comment by @MichaReiser on 2023-07-11 07:44_

> i don't think I have any repo permissions. i guess i can create issues (not sure what you by by creating "from 4798", do you mean and link from there?) but not assign
> 
> from a quick look, set and dict are pretty much like list comps so seem quite straightforward. generator comprehensions are trickier because the brackets are optional in some contexts but mandatory in others

There's a button at the end that allows you to automatically create an issue: 

![image](https://github.com/astral-sh/ruff/assets/1203881/f11d3f7e-fcc5-490d-a252-ec1b01410dd4)

I created https://github.com/astral-sh/ruff/issues/5674 now. I can assign it to you if you leave a comment.

---

_Comment by @davidszotten on 2023-07-11 07:46_

> There's a button at the end that allows you to automatically create an issue:

the circle with the dot in? i don't have that 

---

_Comment by @davidszotten on 2023-07-11 07:49_

while there is still other work to do, maybe we can mark `set` and `list` comp as "easy (see list comp)" to encourage newcomers?

you can assign met to generator exp though i'll probably need a pointer on how to approach this bracket issue

---

_Comment by @MichaReiser on 2023-07-11 07:56_

> the circle with the dot in? i don't have that

Yes. Hmm, that's unfortunate. I created the issue https://github.com/astral-sh/ruff/issues/5676

> you can assign met to generator exp though i'll probably need a pointer on how to approach this bracket issue

I'm currently looking into the expression handling. I, incorrectly, assumed that Black only uses the *expand the right hand side before operands* on the top level. But there seem to be other places where it applies the same logic (generators, with items, ... potentially more). 

You can ignore this for now. 

---

_Comment by @davidszotten on 2023-07-11 08:03_

brackets:

ok. note that for generator expressions it's not just a question of style, as the brackets are _required in some contexts. see e.g. https://peps.python.org/pep-0289/

> This means that you can write:
>
>`sum(x**2 for x in range(10))`
>but you would have to write:
>
>`reduce(operator.add, (x**2 for x in range(10)))`
>and also:
>
> `g = (x**2 for x in range(10))`

---
