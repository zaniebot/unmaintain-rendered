```yaml
number: 4099
title: "Preserve whitespace around `ListComp` brackets in `C419`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/C419/preserve-comments
created_at: 2023-04-25T19:18:58Z
updated_at: 2023-05-13T16:39:10Z
url: https://github.com/astral-sh/ruff/pull/4099
synced_at: 2026-01-12T15:55:14Z
```

# Preserve whitespace around `ListComp` brackets in `C419`

---

_@dhruvmanila_

## Summary

While converting from a list comprehension to a generator expression, the square brackets are not preserved as that are only present on a list comprehension.

We'll convert the brackets into a paren node and add it at the _end_ and _start_ of the left and right parens respectively.

fixes: #4094

---

_Comment by @dhruvmanila on 2023-04-25 19:26_

Hmm, wait this doesn't look correct (missing the initial insta snapshot).

---

_Converted to draft by @dhruvmanila on 2023-04-25 19:26_

---

_Comment by @github-actions[bot] on 2023-04-25 19:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.00     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    424.1±1.35µs     7.0 MB/sec    1.00    418.5±0.72µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.01      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1502.7±21.51µs    11.1 MB/sec    1.00   1495.0±2.96µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.1±0.23µs    17.7 MB/sec    1.01    168.3±0.32µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1056.0±0.63µs    15.8 MB/sec    1.00   1053.2±0.68µs    15.8 MB/sec
parser/numpy/globals.py                    1.01    107.1±0.65µs    27.6 MB/sec    1.00    106.4±0.34µs    27.7 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     18.0±0.32ms     2.3 MB/sec    1.00     17.8±0.42ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.10ms     3.8 MB/sec    1.00      4.4±0.14ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   515.3±10.52µs     5.7 MB/sec    1.00   513.9±18.19µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.16ms     3.4 MB/sec    1.01      7.5±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      9.1±0.33ms     4.5 MB/sec    1.00      8.8±0.21ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1902.9±66.09µs     8.8 MB/sec    1.00  1856.2±51.47µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.03   213.9±10.18µs    13.8 MB/sec    1.00   208.5±11.68µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.11ms     6.4 MB/sec    1.00      4.0±0.12ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.8±0.10ms     6.0 MB/sec    1.01      6.8±0.12ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1309.1±44.54µs    12.7 MB/sec    1.01  1326.0±85.49µs    12.6 MB/sec
parser/numpy/globals.py                    1.03    137.6±8.45µs    21.4 MB/sec    1.00    134.2±4.65µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.08ms     8.7 MB/sec    1.01      3.0±0.10ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-04-26 14:30_

***Context:***

> Ok, so it seems like we're not copying over the `lbracket` and `rbracket` fields from the `ListComp` node. Now, this is a challenge as we need to convert it into a `GeneratorExp` which only has the `lpar` and `rpar` fields. We'll need to pick the required parts from the Bracket nodes into the Parenthesis nodes.

This seems a bit difficult but here are some of my ideas:

1. Convert the `lbracket` and `rbracket` nodes into `LeftParen` and `RightParen`, then append and prepend it respectively to the generator's `lpar` and `rpar` vectors. This means that there will an [extraneous parenthesis](https://beta.ruff.rs/docs/rules/#pyupgrade-up) added. **(Currently implemented)**
	
	```python
	any(
		[
			i for i in range(10)  # comment
		]
	)

	# Above will be converted to
	any(
		(  # <-- these are the extra parenthesis
			i for i in range(10)  # comment
		)
	)
	```
2. Copy the `rbracket` into the `whitespace_after_arg` as that's where it belongs and then append _only_ the comment part from the `Comma` node as it is currently done so. If comma is not present, then the same comment belongs to `whitespace_after_arg` which means that the comment from `rbracket` will be prepended to it instead. This will be a special case as it'll handle only this specific issue and ignore the comment from `lbracket`, if any.

	```python
	any(
		[
			i for i in range(10)  # rbracket comment
		]  # whitespace_after_arg comment (comma is absent)
	)

	# Above should be converted to
	any(
			i for i in range(10)  # rbracket comment # whitespace_after_arg comment (comma is absent)
	)
	```

/cc @charliermarsh @MichaReiser your input would be appreciated :)

---

_Comment by @charliermarsh on 2023-04-29 03:28_

I've read this a few times and keep finding myself undecided. I think adding extra parentheses is undesirable, so I'm leaning towards Option 2. What I don't fully understand is the role on the comma in that logic -- how does it differ when the comment is present vs. absent?

---

_Comment by @dhruvmanila on 2023-04-30 11:20_

> What I don't fully understand is the role on the comma in that logic -- how does it differ when the comment is present vs. absent?

Ok, so the `Arg` node contains the `comma` field which is optional and then there's `whitespace_after_arg` as well. If the argument is not followed by the comma then the comment will be in `whitespace_after_arg` node otherwise it'll be in the `comma` node.

https://github.com/Instagram/LibCST/blob/889ce56b0fcb9724ef980c7f2305d04f6bb7cd34/native/libcst/src/nodes/expression.rs#L321-L332

```python
all(
    [x.id for x in bar],  # comment
)

# vs

all(
	[x.id for x in bar]
)
```

The issue we're facing is that the comment is in the `rbracket` node of `ListComp` node which, along with `lbracket` is being ignored (no brackets in generator expressions).

---

_Comment by @MichaReiser on 2023-05-01 09:44_

I'm unfamiliar with LibCST, and I've failed to find a clear definition of how libCST decides to attach trivia (to which node/property). So what I'm saying here may not make sense in the context of LibCST. 

I prefer your second option, and you're algorithm for copying the trivia makes sense to me. Do we need some logic for copying any trivia attached to the opening bracket too? What if it is a standalone comprehension or will this never trigger C419?

```python
[
		i for i in range(10)  # comment
]
```



---

_Comment by @dhruvmanila on 2023-05-01 12:04_

> Do we need some logic for copying any trivia attached to the opening bracket too?

I believe we should but I don't see any use case for having a comment on the opening bracket (correct me if I'm wrong):

```python
any(
	[  # where would this comment be used?
		i.id for i in range(5)
	]
)

# If so, that should be converted to
any(
	i.id for i in range(5)  # where would this comment be used?
)
```

In case we want to support it, I believe it should be _added_ or _appended_ to the generator expression node as shown above.

And, in the worst case, if there are comments in all three places:

```python
any(
	[  # rbracket comment
		i.id for i in range(5)  # comprehension comment
	]  # lbracket comment
)

any(
	i.id for i in range(5)  # comprehension comment  # lbracket comment  # rbracket comment
)
```

One more thing to note is that `black` formats the way shown below. Note that there's no comment at the opening bracket, if there was, then `black` wouldn't format it.

```python
any(
    [
        i.id for i in range(5)  # comprehension comment
    ]  # lbracket comment
)

any([i.id for i in range(5)])  # comprehension comment  # lbracket comment
```

> What if it is a standalone comprehension or will this never trigger C419?

This won't be triggered for standalone comprehensions. The rule is only for `any` and `all` function calls with one argument.

---

_Comment by @MichaReiser on 2023-05-01 12:16_

> In case we want to support it, I believe it should be added or appended to the generator expression node as shown above.

My take is that ruff should never remove any comments and we can either not provide a fix in that case or implement the proper placement. 



> ```python
> any(
> 	[  # rbracket comment
> 		i.id for i in range(5)  # comprehension comment
> 	]  # lbracket comment
> )
> 
> any(
> 	i.id for i in range(5)  # comprehension comment  # lbracket comment  # rbracket comment
> )
> ```

I would prefer writing the `rbracket_comment` before the `lbracket_comment` to ensure comments maintain the same sequence as they had in the source document. This is important because the comment explanations may depend on their ordering. Or is that what you're suggesting but you mixed up `rbracket` and `lbracket` in the source example?

What do you think of attaching the `# bracket comment` to the whole comprehension expression?

```python
# rbracket comment
i.id for i in range(5)  # comprehension comment
```

Are there any more edge cases, for example, when `[` or `]` has any leading comments:

```py
> any(
> 	# lbracket leading
> 	[  # lbracket trailing
> 		i.id for i in range(5)  # comprehension comment
> 	# rbracket_leading
> 	]  # rbracket trailing
> )
```

Or is this not an issue because of how libCST represents comments?

---

_Comment by @dhruvmanila on 2023-05-01 13:54_

Um wait, I think I switched the `lbracket` and `rbracket` notations in the code. Another mistake I made was that the "comprehension comment" belongs to `rbracket` node while the "lbracket comment" (which should be "rbracket comment") belongs to `rpar` node (which is copied correctly).

So, `lbracket` contains the comment occurring _after_ the token while `rbracket` contains the comment occurring _before_ the token and it's the same for parenthesis.

---

> My take is that ruff should never remove any comments and we can either not provide a fix in that case or implement the proper placement.

I agree on that. The problem is stemming from the fact that we're _removing_ the tokens (left and right brackets) which means that whatever trivia belonged to that token is being removed as well.

> I would prefer writing the `rbracket_comment` before the `lbracket_comment` to ensure comments maintain the same sequence as they had in the source document. This is important because the comment explanations may depend on their ordering. Or is that what you're suggesting but you mixed up `rbracket` and `lbracket` in the source example?

My first thought was that order wouldn't matter as both would belong to the same line but I can understand your point of view.

> What do you think of attaching the `# bracket comment` to the whole comprehension expression?

Oh wait! This suggestion led me to another idea (thanks!): So, there's another node in `libCST` which is [`EmptyNode`](https://github.com/Instagram/LibCST/blob/889ce56b0fcb9724ef980c7f2305d04f6bb7cd34/native/libcst/src/nodes/whitespace.rs#L84-L91) and this can be used to have a line with _only_ a comment in it along with the correct indentation. I haven't looked as to how to implement it, but this is what would happen:

```python
any(
    [  # lbracket comment
        i.bit_count() for i in range(5)  # rbracket comment
    ]  # rpar comment
)


any(
    # lbracket comment
        i.bit_count() for i in range(5)  # rbracket comment  # rpar comment
)
```

1. The "lbracket comment" will be an `EmptyNode` which will be added to `whitespace_before_args`.
2. The "rbracket comment" will be added to `whitespace_after_args`
3. Similarly, the "rpar comment" will be either be added or appended to `whitespace_after_args` depending on (2). Or do we want to keep this in it's own line?

What do you think?

> Or is this not an issue because of how libCST represents comments?

This won't be an issue as those belong with the parenthesis which are copied correctly over to the generator expression **(line 1162, 1163)**:

https://github.com/charliermarsh/ruff/blob/814731364afa995ae4a65da5e0dd85791a64b4c2/crates/ruff/src/rules/flake8_comprehensions/fixes.rs#L1159-L1164



---

_Comment by @charliermarsh on 2023-05-02 01:05_

> My take is that ruff should never remove any comments and we can either not provide a fix in that case or implement the proper placement.

Unfortunately we do remove comments in some situations. Ruff's comment-handling for autofixes is "best effort" right now. In some cases, with complex expressions, we _do_ skip the autofix entirely if comments are present, but it's done on a case-by-case basis.


---

_Comment by @MichaReiser on 2023-05-02 07:28_

@dhruvmanila I'm really impressed by how carefully and thoughtfully you exercise the comment placement! It's a joy to read your explanations. 

> What do you think?

I love it. That would match my expectations. I also thought about whether we want to keep the `#rpar comment` on its own line if the source contains a newline between the comprehension's end and the `]` but I don't think that's necessary because the newline is only present because of the `(...)` wrapping of the comprehension. 



---

_Comment by @dhruvmanila on 2023-05-02 16:02_

> @dhruvmanila I'm really impressed by how carefully and thoughtfully you exercise the comment placement! It's a joy to read your explanations.

Thanks a lot for those kind words! I really appreciate it and also inspired by your detailed explanations :)

> I love it. That would match my expectations. I also thought about whether we want to keep the `#rpar comment` on its own line if the source contains a newline between the comprehension's end and the `]` but I don't think that's necessary because the newline is only present because of the `(...)` wrapping of the comprehension.

Got it. I'll try to finish this by tonight.



---

_Marked ready for review by @dhruvmanila on 2023-05-06 10:33_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:1168 on 2023-05-08 12:06_

I love these comments! Thank you so much for taking the time to write them. It makes reviewing (and reading the code in the future) so much easier.

---

_@MichaReiser approved on 2023-05-08 12:08_

Excellent

---

_Comment by @charliermarsh on 2023-05-08 18:58_

Doesn't need to block the merge here, but do we need to apply similar treatment to any of the other comprehension-transform rules?

---

_Comment by @dhruvmanila on 2023-05-08 23:28_

> Doesn't need to block the merge here, but do we need to apply similar treatment to any of the other comprehension-transform rules?

I had the same thought, let me check. In general wherever the parenthesis or brackets are being dropped, that's where this problem will occur.

---

_Comment by @dhruvmanila on 2023-05-08 23:45_

I've provided a few examples but there will be a few more. As mentioned that whenever the parenthesis or brackets are being dropped, that's where this will happen. It seems to be happening in most of comprehension fixes.

I believe we should have a generalized way of resolving this.

---

## C404

```python
dict(
    # first comment
    [
        # second comment
        (i, i)
        for i in range(3)
    ]
)

{
    # first comment
    i: i
        for i in range(3)
}
```

## C405

```python
set(
    # some comment
    (1, 2)
)

{1, 2}
```

## C406

```python
dict(
    # first comment
    [
        # second comment
        (1, 2)
    ]
)

{
    # first comment
    1: 2
}
```

## C409

```python
tuple(
    # some comment
    [1, 2, 3, 4]
)

(1, 2, 3, 4)
```

## C410

```python
list(
    # some comment
    [1, 2, 3, 4]
)

[1, 2, 3, 4]
```

## C411

```python
list(
    # comment
    [i * i for i in x]
)

[i * i for i in x]
```

---

_Merged by @MichaReiser on 2023-05-09 06:43_

---

_Closed by @MichaReiser on 2023-05-09 06:43_

---

_Branch deleted on 2023-05-13 16:39_

---
