```yaml
number: 8857
title: "docstring code formatter: remove \"invalid Python\" check"
type: issue
state: open
author: BurntSushi
labels:
  - performance
  - docstring
  - formatter
  - wish
assignees: []
created_at: 2023-11-27T16:12:10Z
updated_at: 2023-11-29T13:31:43Z
url: https://github.com/astral-sh/ruff/issues/8857
synced_at: 2026-01-12T15:54:48Z
```

# docstring code formatter: remove "invalid Python" check

---

_@BurntSushi_

#8811 added a docstring code snippet formatter. As part of the initial implementation, it is actually possible for the reformatter to transform valid Python to invalid Python, usually as a result of corner cases related to triple quoting. Since these are odd cases, for expediency, the initial implementation checks if the reformatted code is valid. If it isn't, then it bails out of reformatting and skips the code snippets entirely.

Ideally, we would be able to have more confidence in our code snippet reformatter to the point that we could remove this check for invalid Python code. Doing this will likely require some refactoring for how nested triple quotes are handled.

Here's a good example from the tests that doesn't work today:

https://github.com/astral-sh/ruff/blob/d6148b6b1490bc85a2790020759c687d6bb6da96/crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py#L307-L316

Namely, the code snippet there _ought_ to be formatted, but today, it is skipped because the reformatting currently generates invalid Python code.

---

_Label `performance` added by @BurntSushi on 2023-11-27 16:12_

---

_Label `formatter` added by @BurntSushi on 2023-11-27 16:12_

---

_Label `wish` added by @BurntSushi on 2023-11-27 16:12_

---

_Comment by @MichaReiser on 2023-11-27 23:25_

Do we have a benchmark that tests the performance of docstring formatting? Adding one could help to understand the performance characteristics better and validate any improvements to it. 

Do we always perform the re-parse today or do we use a heuristic when the re-parse is necessary (only when the source text contains triple quoted strings?). A text search should be multiple times faster than a full re-parse.

---

_Label `docstring` added by @MichaReiser on 2023-11-27 23:29_

---

_Comment by @BurntSushi on 2023-11-28 14:58_

There aren't any benchmarks for code snippet formatting yet. Using a heuristic to avoid a re-parse seems plausible, but only if we buy that the cases where invalid Python can be produced are limited to triple quoting. (I think I buy that, otherwise I don't see how it could break out of the enclosing docstring.)

For an ad hoc benchmark, if I run the formatter with and without `docstring-code` enabled, then runtime is about the same. I ran it a few times and couldn't notice a difference. (Not that this is a good substitute for a real benchmark, but perhaps suggestive that docstring code formatting doesn't have a huge impact on perf.)

---

_Comment by @MichaReiser on 2023-11-28 23:51_

> For an ad hoc benchmark, if I run the formatter with and without docstring-code enabled, then runtime is about the same. I ran it a few times and couldn't notice a difference. (Not that this is a good substitute for a real benchmark, but perhaps suggestive that docstring code formatting doesn't have a huge impact on perf.)

I assume you did that for a project that has docstrings?

If you happen to have a test file with code snippets, then you could add it to this benchmark:

https://github.com/astral-sh/ruff/blob/e62e245c61db90c8cee7056d640f091f8153cf39/crates/ruff_benchmark/benches/formatter.rs#L28-L42

And change the options to always enable the docstring code option here

https://github.com/astral-sh/ruff/blob/e62e245c61db90c8cee7056d640f091f8153cf39/crates/ruff_benchmark/benches/formatter.rs#L72

The benchmark will then run automatically as part of codspeed.

---

_Comment by @BurntSushi on 2023-11-29 13:31_

> I assume you did that for a project that has docstrings?

Yeah. I ran it on CPython and polars.

I added a new issue tracking adding benchmarks specifically targeted to code snippet reformatting: https://github.com/astral-sh/ruff/issues/8909

---
