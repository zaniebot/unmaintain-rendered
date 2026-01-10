```yaml
number: 11457
title: Maintain synchronicity between the lexer and the parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - performance
  - parser
assignees: []
merged: true
base: main
head: dhruv/parser-phase-2
created_at: 2024-05-17T09:39:50Z
updated_at: 2024-06-05T14:42:33Z
url: https://github.com/astral-sh/ruff/pull/11457
synced_at: 2026-01-10T21:55:59Z
```

# Maintain synchronicity between the lexer and the parser

---

_Pull request opened by @dhruvmanila on 2024-05-17 09:39_

## Summary

This PR updates the entire parser stack in multiple ways:

### Make the lexer lazy

* https://github.com/astral-sh/ruff/pull/11244
* https://github.com/astral-sh/ruff/pull/11473

Previously, Ruff's lexer would act as an iterator. The parser would collect all the tokens in a vector first and then process the tokens to create the syntax tree.

The first task in this project is to update the entire parsing flow to make the lexer lazy. This includes the `Lexer`, `TokenSource`, and `Parser`. For context, the `TokenSource` is a wrapper around the `Lexer` to filter out the trivia tokens[^1]. Now, the parser will ask the token source to get the next token and only then the lexer will continue and emit the token. This means that the lexer needs to be aware of the "current" token. When the `next_token` is called, the current token will be updated with the newly lexed token.

The main motivation to make the lexer lazy is to allow re-lexing a token in a different context. This is going to be really useful to make the parser error resilience. For example, currently the emitted tokens remains the same even if the parser can recover from an unclosed parenthesis. This is important because the lexer emits a `NonLogicalNewline` in parenthesized context while a normal `Newline` in non-parenthesized context. This different kinds of newline is also used to emit the indentation tokens which is important for the parser as it's used to determine the start and end of a block.

Additionally, this allows us to implement the following functionalities:
1. Checkpoint - rewind infrastructure: The idea here is to create a checkpoint and continue lexing. At a later point, this checkpoint can be used to rewind the lexer back to the provided checkpoint.
2. Remove the `SoftKeywordTransformer` and instead use lookahead or speculative parsing to determine whether a soft keyword is a keyword or an identifier
3. Remove the `Tok` enum. The `Tok` enum represents the tokens emitted by the lexer but it contains owned data which makes it expensive to clone. The new `TokenKind` enum just represents the type of token which is very cheap.

This brings up a question as to how will the parser get the owned value which was stored on `Tok`. This will be solved by introducing a new `TokenValue` enum which only contains a subset of token kinds which has the owned value. This is stored on the lexer and is requested by the parser when it wants to process the data. For example: https://github.com/astral-sh/ruff/blob/8196720f809380d8f1fc7651679ff3fc2cb58cd7/crates/ruff_python_parser/src/parser/expression.rs#L1260-L1262

[^1]: Trivia tokens are `NonLogicalNewline` and `Comment`

### Remove `SoftKeywordTransformer`

* https://github.com/astral-sh/ruff/pull/11441
* https://github.com/astral-sh/ruff/pull/11459
* https://github.com/astral-sh/ruff/pull/11442
* https://github.com/astral-sh/ruff/pull/11443
* https://github.com/astral-sh/ruff/pull/11474

For context, https://github.com/RustPython/RustPython/pull/4519/files#diff-5de40045e78e794aa5ab0b8aacf531aa477daf826d31ca129467703855408220 added support for soft keywords in the parser which uses infinite lookahead to classify a soft keyword as a keyword or an identifier. This is a brilliant idea as it basically wraps the existing Lexer and works on top of it which means that the logic for lexing and re-lexing a soft keyword remains separate. The change here is to remove `SoftKeywordTransformer` and let the parser determine this based on context, lookahead and speculative parsing.

* **Context:** The transformer needs to know the position of the lexer between it being at a statement position or a simple statement position. This is because a `match` token starts a compound statement while a `type` token starts a simple statement. **The parser already knows this.**
* **Lookahead:** Now that the parser knows the context it can perform lookahead of up to two tokens to classify the soft keyword. The logic for this is mentioned in the PR implementing it for `type` and `match soft keyword.
* **Speculative parsing:** This is where the checkpoint - rewind infrastructure helps. For `match` soft keyword, there are certain cases for which we can't classify based on lookahead. The idea here is to create a checkpoint and keep parsing. Based on whether the parsing was successful and what tokens are ahead we can classify the remaining cases. Refer to #11443 for more details.

If the soft keyword is being parsed in an identifier context, it'll be converted to an identifier and the emitted token will be updated as well. Refer https://github.com/astral-sh/ruff/blob/8196720f809380d8f1fc7651679ff3fc2cb58cd7/crates/ruff_python_parser/src/parser/expression.rs#L487-L491. 

The `case` soft keyword doesn't require any special handling because it'll be a keyword only in the context of a match statement.

### Update the parser API

* https://github.com/astral-sh/ruff/pull/11494
* https://github.com/astral-sh/ruff/pull/11505

Now that the lexer is in sync with the parser, and the parser helps to determine whether a soft keyword is a keyword or an identifier, the lexer cannot be used on its own. The reason being that it's not sensitive to the context (which is correct). This means that the parser API needs to be updated to not allow any access to the lexer.

Previously, there were multiple ways to parse the source code:
1. Passing the source code itself
2. Or, passing the tokens

Now that the lexer and parser are working together, the API corresponding to (2) cannot exists. The final API is mentioned in this PR description: https://github.com/astral-sh/ruff/pull/11494.

### Refactor the downstream tools (linter and formatter)

* https://github.com/astral-sh/ruff/pull/11511
* https://github.com/astral-sh/ruff/pull/11515
* https://github.com/astral-sh/ruff/pull/11529
* https://github.com/astral-sh/ruff/pull/11562
* https://github.com/astral-sh/ruff/pull/11592

And, the final set of changes involves updating all references of the lexer and `Tok` enum. This was done in two-parts:
1. Update all the references in a way that doesn't require any changes from this PR i.e., it can be done independently
	* https://github.com/astral-sh/ruff/pull/11402
	* https://github.com/astral-sh/ruff/pull/11406
	* https://github.com/astral-sh/ruff/pull/11418
	* https://github.com/astral-sh/ruff/pull/11419
	* https://github.com/astral-sh/ruff/pull/11420
	* https://github.com/astral-sh/ruff/pull/11424
2. Update all the remaining references to use the changes made in this PR

For (2), there were various strategies used:
1. Introduce a new `Tokens` struct which wraps the token vector and add methods to query a certain subset of tokens. These includes:
	1. `up_to_first_unknown` which replaces the `tokenize` function
	2. `in_range` and `after` which replaces the `lex_starts_at` function where the former returns the tokens within the given range while the latter returns all the tokens after the given offset
2. Introduce a new `TokenFlags` which is a set of flags to query certain information from a token. Currently, this information is only limited to any string type token but can be expanded to include other information in the future as needed. https://github.com/astral-sh/ruff/pull/11578
3. Move the `CommentRanges` to the parsed output because this information is common to both the linter and the formatter. This removes the need for `tokens_and_ranges` function.

## Test Plan

- [x] Update and verify the test snapshots
- [x] Make sure the entire test suite is passing
- [x] Make sure there are no changes in the ecosystem checks
- [x] Run the fuzzer on the parser
- [x] Run this change on dozens of open-source projects

### Running this change on dozens of open-source projects

The following open source projects were used:

- https://github.com/python-pillow/Pillow
- https://github.com/aio-libs/aiohttp
- https://github.com/ansible/awx
- https://github.com/ansible/ansible
- https://github.com/apache/airflow
- https://github.com/apache/spark
- https://github.com/apache/superset
- https://github.com/keras-team/autokeras
- https://github.com/boto/boto3
- https://github.com/kovidgoyal/calibre
- https://github.com/celery/celery
- https://github.com/pallets/click
- https://github.com/python/cpython
- https://github.com/dagster-io/dagster
- https://github.com/unit8co/darts
- https://github.com/dask/dask
- https://github.com/simonw/datasette
- https://github.com/encode/django-rest-framework
- https://github.com/django/django
- https://github.com/pallets/flask
- https://github.com/frappe/erpnext
- https://github.com/graphql-python/graphene
- https://github.com/benoitc/gunicorn
- https://github.com/home-assistant/core
- https://github.com/encode/httpx
- https://github.com/huggingface/transformers
- https://github.com/ipython/ipython
- https://github.com/pallets/jinja
- https://github.com/keras-team/keras
- https://github.com/localstack/localstack
- https://github.com/spotify/luigi
- https://github.com/ManimCommunity/manim
- https://github.com/matplotlib/matplotlib
- https://github.com/bloomberg/memray
- https://github.com/Netflix/metaflow
- https://github.com/mkdocs/mkdocs
- https://github.com/python/mypy
- https://github.com/networkx/networkx
- https://github.com/numba/numba
- https://github.com/numpy/numpy
- https://github.com/openai/openai-python
- https://github.com/dbcli/pgcli
- https://github.com/pypa/pip
- https://github.com/plotly/dash
- https://github.com/pre-commit/pre-commit
- https://github.com/psf/requests
- https://github.com/arrow-py/arrow
- https://github.com/pydantic/pydantic
- https://github.com/pytest-dev/pytest
- https://github.com/pytorch/pytorch
- https://github.com/qutebrowser/qutebrowser
- https://github.com/ray-project/ray
- https://github.com/readthedocs/readthedocs.org
- https://github.com/redis/redis-py
- https://github.com/rq/rq
- https://github.com/saleor/saleor
- https://github.com/scikit-learn/scikit-learn
- https://github.com/scipy/scipy
- https://github.com/mwaskom/seaborn
- https://github.com/getsentry/sentry
- https://github.com/spyder-ide/spyder
- https://github.com/sqlalchemy/sqlalchemy
- https://github.com/encode/starlette
- https://github.com/statsmodels/statsmodels
- https://github.com/streamlit/streamlit
- https://github.com/sympy/sympy
- https://github.com/tensorflow/tensorflow
- https://github.com/Textualize/rich
- https://github.com/Textualize/textual
- https://github.com/TheAlgorithms/Python
- https://github.com/tornadoweb/tornado
- https://github.com/encode/uvicorn
- https://github.com/google/yapf
- https://github.com/yt-dlp/yt-dlp

Now, the following tests were done between `main` and this branch:
1. Compare the output of `--select=E999` (syntax errors)
2. Compare the output of default rule selection
3. Compare the output of `--select=ALL`

**Conclusion: all output were same**

## What's next?

The next step is to introduce re-lexing logic and update the parser to feed the recovery information to the lexer so that it can emit the correct token. This moves us one step closer to having error resilience in the parser and provides Ruff the possibility to lint even if the source code contains syntax errors.

---

_Label `parser` added by @dhruvmanila on 2024-05-21 04:45_

---

_Comment by @codspeed-hq[bot] on 2024-05-30 11:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/parser-phase-2)

### Merging #11457 will **improve performances by 28.11%**

<sub>Comparing <code>dhruv/parser-phase-2</code> (e3d09f3) with <code>main</code> (a9b6c4f)</sub>



### Summary

`⚡ 20` improvements
`✅ 10` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/parser-phase-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[large/dataset.py]` | 1.4 ms | 1.1 ms | +27.61% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 284.3 µs | 226 µs | +25.81% |
| ⚡ | `lexer[numpy/globals.py]` | 36.7 µs | 29.6 µs | +24.04% |
| ⚡ | `lexer[pydantic/types.py]` | 639.9 µs | 503.1 µs | +27.17% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 98.3 µs | 76.7 µs | +28.11% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 16.9 ms | 15.9 ms | +6.53% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 4.1 ms | 3.9 ms | +4.89% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 8.2 ms | 7.7 ms | +6.7% |
| ⚡ | `linter/default-rules[large/dataset.py]` | 4.3 ms | 3.7 ms | +16.05% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 1,014.3 µs | 945.5 µs | +7.28% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 2.1 ms | 1.9 ms | +11.47% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 406.2 µs | 359.6 µs | +12.94% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 20.2 ms | 19.1 ms | +5.8% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 4.9 ms | 4.7 ms | +5.57% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 10 ms | 9.5 ms | +5.94% |
| ⚡ | `parser[large/dataset.py]` | 5.7 ms | 5.2 ms | +10.44% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1,158 µs | 990 µs | +16.97% |
| ⚡ | `parser[numpy/globals.py]` | 122.1 µs | 105.5 µs | +15.84% |
| ⚡ | `parser[pydantic/types.py]` | 2.3 ms | 2.1 ms | +9.7% |
| ⚡ | `parser[unicode/pypinyin.py]` | 371.2 µs | 334.9 µs | +10.85% |


---

_Comment by @github-actions[bot] on 2024-05-31 03:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @dhruvmanila on 2024-05-31 14:13_

---

_Review requested from @carljm by @dhruvmanila on 2024-05-31 14:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-31 14:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-31 14:13_

---

_Review request for @carljm removed by @dhruvmanila on 2024-06-03 04:17_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-06-03 04:17_

---

_@MichaReiser approved on 2024-06-03 06:58_

Excellent work Dhruv and thanks for the very detailed PR summary. 

It would be interesting to run our hyperfine benchmarks on some selected projects to see if we also see non-microbenchmark improvements.

---

_Comment by @dhruvmanila on 2024-06-03 11:47_

As a side effect of the changes in this PR, Ruff will detect any violations related to comments in a file containing syntax errors. For example, in https://play.ruff.rs/948508af-407e-49e5-8c12-7409e59a5e73 (code below), the second comment is not highlighted because there's a lexical error (unterminated string). But, now Ruff will highlight both comments for `ERA001`.

```py
# import os

"hello

# import sys
```

---

_Merged by @dhruvmanila on 2024-06-03 12:53_

---

_Closed by @dhruvmanila on 2024-06-03 12:53_

---

_Branch deleted on 2024-06-03 12:53_

---

_Label `performance` added by @dhruvmanila on 2024-06-05 14:42_

---
