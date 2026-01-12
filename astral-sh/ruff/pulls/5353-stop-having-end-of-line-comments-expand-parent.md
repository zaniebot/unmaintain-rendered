```yaml
number: 5353
title: stop having end-of-line comments expand parent
type: pull_request
state: closed
author: davidszotten
labels: []
assignees: []
draft: true
base: main
head: testing-expand-parent
created_at: 2023-06-25T08:56:38Z
updated_at: 2023-06-26T10:45:02Z
url: https://github.com/astral-sh/ruff/pull/5353
synced_at: 2026-01-12T15:55:18Z
```

# stop having end-of-line comments expand parent

---

_@davidszotten_

this seems more in line with black (and seems to help with `StmtWith`
formatting)

only unexpected change in the diff afict is

```
while (
    some_condition(unformatted, args) # trailing some condition
    and anotherCondition or aThirdCondition  # trailing third condition
):  # comment
    print("Do something")
```

which for some reason (bug elsewhere?) now doesn't get wrapped


to keep dict formatting from getting worse, first add magic trailing comma support to dicts (separate commit) which 
seems mostly better, though still need to think about

`json = {"k": {"k2": {"k3": [1,]}}}` where we differ from black in
trailing commans for all the outer dicts



---

_Comment by @github-actions[bot] on 2023-06-25 09:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.06ms     5.1 MB/sec    1.01      8.0±0.11ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1871.2±3.40µs     8.9 MB/sec    1.00  1866.9±26.89µs     8.9 MB/sec
formatter/numpy/globals.py                 1.03    235.1±6.38µs    12.6 MB/sec    1.00    227.4±1.61µs    13.0 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.1 MB/sec    1.00      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.32ms     2.6 MB/sec    1.00     15.7±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.2 MB/sec    1.00      3.9±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.0±3.93µs     5.8 MB/sec    1.00    505.7±1.00µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.6 MB/sec    1.00      7.0±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1753.4±8.54µs     9.5 MB/sec    1.00  1710.3±22.85µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    193.6±2.10µs    15.2 MB/sec    1.00    191.1±2.94µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.05ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.42ms     4.1 MB/sec    1.08     10.6±0.63ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.10ms     7.7 MB/sec    1.11      2.4±0.18ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00   269.2±13.76µs    11.0 MB/sec    1.09   293.4±21.21µs    10.1 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.19ms     5.3 MB/sec    1.11      5.4±0.28ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.01     18.9±0.77ms     2.2 MB/sec    1.00     18.8±0.71ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.23ms     3.3 MB/sec    1.00      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   626.1±27.78µs     4.7 MB/sec    1.00   614.2±27.84µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.30ms     3.0 MB/sec    1.00      8.5±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.39ms     4.1 MB/sec    1.03     10.2±0.69ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.7 MB/sec    1.00      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   252.4±11.89µs    11.7 MB/sec    1.09   274.4±19.80µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.7±0.22ms     5.4 MB/sec    1.00      4.6±0.18ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_list.rs`:55 on 2023-06-26 05:54_

Nice! Would you mind splitting this change into its own PR?

---

_@MichaReiser reviewed on 2023-06-26 06:06_

Thanks for looking into this difference. 

This deviation has been a deliberate decision of me, but we haven't decided if we want to keep it. 

Overall, our goal is not 100% black compatibility. Our main goal is to format already *black formatted* files without changes, to ease adoption. My understanding is that the current formatting upholdes this property. 

The reasons why I decided to deviate are:

* Using `expand` can prevent *comment migration*. This is a problem specific to trailing comments because they potentially move from the end of node **A** to the end of node **B** after formatting once. This can be problematic because the formatter may decide for a different layout because `has_dangling_comments` now suddenly returns `false`, or the comment moves even further on the second format and now becomes a trailing comment of node **C**
* In my view, it's a desired property to respect the user's comment positioning when possible (I mean, we support the weirdest slice comment placements). That means, We shouldn't move the comment from the argument `b` in the following example if I deliberately choose to document the argument `b` (and not the function header). 

	```python
	function(
		a,
		b=[] # Using a list here is important because of X
	):  
	```
	
	Moving the comment to the end of the line changes the context of the comment and it now becomes unclear to what *here* is referencing. Preserving the placement can further prevent that `type-ignore` or other placement sensitive comments (noqa) are moved or increase scope.
* Aggressively collapsing comments can lead to some weird comment formatting (Ruff can run into this too, but it should happen less often)
	```python
	# Input
	while (
    	cond1  # almost always true
	    and cond2  # almost never true
	):
    	print("Do something")

	# Output
	while cond1 and cond2:  # almost always true  # almost never true
    	print("Do something")
	```

What's your perspective on the comment placement? @konstin I remember that you were leaning towards moving comments to the end too


---

_Comment by @davidszotten on 2023-06-26 06:42_

thanks for the explanation. l also prefer the current output, but was concerned about black compat, however

>  Our main goal is to format already black formatted files without changes, to ease adoption.

this is a great distinction i hadn't considered (no changes to black-formatted code vs same output as black on unformatted code). 


aside: is this how we use the fixtures from black's test suite? (format with black, _then with ruff). should it be?

---

_Closed by @davidszotten on 2023-06-26 06:42_

---

_Comment by @MichaReiser on 2023-06-26 06:48_

> aside: is this how we use the fixtures from black's test suite? (format with black, _then with ruff). should it be?

This is a good point. No. The fixtures compare Black's and Ruff's output after formatting an unformatted file. But I like what you're proposing to add another test that uses the expected black output and run it through ruff. 

---

_Comment by @konstin on 2023-06-26 06:54_

> What's your perspective on the comment placement? @konstin I remember that you were leaning towards moving comments to the end too

Generally favourable towards moving them to the end.

> Using expand can prevent comment migration

I'm fine with a certain amount of comment migration, i often like it when black does that. I feel like having one line comments be fixed and end-of-line comments migrate is a good compromise and gives the user convenience (the formatter will collapse my statements) and control (i can use an own line comment and it will stick).

> Moving the comment to the end of the line changes the context of the comment and it now becomes unclear to what here is referencing. Preserving the placement can further prevent that type-ignore or other placement sensitive comments (noqa) are moved or increase scope.

> Aggressively collapsing comments can lead to some weird comment formatting (Ruff can run into this too, but it should happen less often)

Those are good points, and i wouldn't be sure how to handle them.

---

_Comment by @MichaReiser on 2023-06-26 07:29_

> I'm fine with a certain amount of comment migration, i often like it when black does that. I feel like having one line comments be fixed and end-of-line comments migrate is a good compromise and gives the user convenience (the formatter will collapse my statements) and control (i can use an own line comment and it will stick).

This resonates with me and I see this working well in an editor. However, it may require manual review when formatting a new project. I added a new item to the formatter task to make a decision and linked to this conversation.

---

_Comment by @davidszotten on 2023-06-26 09:05_

> > I'm fine with a certain amount of comment migration, i often like it when black does that. I feel like having one line comments be fixed and end-of-line comments migrate is a good compromise and gives the user convenience (the formatter will collapse my statements) and control (i can use an own line comment and it will stick).

Do you have an example of a comment migration that you like (esp. one where comments got merged)?

---

_Comment by @konstin on 2023-06-26 09:40_

```python
model.train(
    learning_rate=0.0001, # 0.001 doesn't converge
    batch=128 # about 8GB
)
```
gets merged into
```python
model.train(learning_rate=0.0001, batch=128)  # 0.001 doesn't converge  # about 8GB
```

---

_Comment by @davidszotten on 2023-06-26 10:41_

interesting. i would prefer the separate rather than the merged output in that example (note that "that you like" in the above)

---
