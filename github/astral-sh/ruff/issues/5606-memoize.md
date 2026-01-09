---
number: 5606
title: Memoize?
type: issue
state: closed
author: DimitriPapadopoulos
labels: []
assignees: []
created_at: 2023-07-08T08:31:34Z
updated_at: 2023-07-08T11:59:35Z
url: https://github.com/astral-sh/ruff/issues/5606
synced_at: 2026-01-07T13:12:15-06:00
---

# Memoize?

---

_Issue opened by @DimitriPapadopoulos on 2023-07-08 08:31_

I cannot find `memoize` in dictionaries, shouldn't it have been `memorize?`
https://github.com/astral-sh/ruff/blob/0b9af031fb8f51e7f9a72c31366bf5f87b90ca3c/crates/ruff_formatter/src/format_extensions.rs#L72
https://github.com/astral-sh/ruff/blob/0b9af031fb8f51e7f9a72c31366bf5f87b90ca3c/crates/ruff_formatter/src/prelude.rs#L5
https://github.com/astral-sh/ruff/blob/0b9af031fb8f51e7f9a72c31366bf5f87b90ca3c/crates/ruff_python_formatter/src/expression/binary_like.rs#L36

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Referenced in [astral-sh/ruff#5607](../../astral-sh/ruff/pulls/5607.md) on 2023-07-08 09:02_

---

_Comment by @MichaReiser on 2023-07-08 09:33_

Interesting. I guess the problem why the term isn't in a general purpose dictionary is because it is a term specific to computer science. https://en.wiktionary.org/wiki/memoize

---

_Comment by @DimitriPapadopoulos on 2023-07-08 09:53_

I wouldn't be surprised if some words “specific to computer science” were initially actual misspellings. I don't see a substantial difference between the definitions of `memorize` and `memoize`, one that would explain the new spelling. Besides, it seems to me the usual word in computer science for “_store (the result of a computation so that it can be subsequently retrieved without repeating the computation_” is **`cache`**.

It is true `memoize` has been used more “widely” [since 1980](https://books.google.com/ngrams/graph?content=memoize&year_start=1800&year_end=2019&corpus=en-2019), but [many orders of magnitude less than `memorize`](https://books.google.com/ngrams/graph?content=memoize%2Cmemorize&year_start=1800&year_end=2019&corpus=en-2019). I wonder how to find the seminal misspelling around 1970-1980, just to check whether it seems intentional or not. Perhaps it originates in Lisp and is intentional after all. From the [Advance Papers of the Fourth International Joint Conference on Artificial Intelligence,Tbilisi, Georgia, USSR, 3-8 Sept. 1975, Volume 2](https://www.google.com/books/edition/Advance_Papers/kp8dAQAAIAAJ):
> so they only have to be computed once ( “memoization” ), and using automatic simplification of the lower levels of access functions.

> whether and how to “memoize” data, and so on.

---

_Comment by @wookie184 on 2023-07-08 11:52_

The [wikipedia page for memoization](https://en.wikipedia.org/wiki/Memoization) suggests the spelling was intentional, and describes it as a specific type of caching

---

_Closed by @DimitriPapadopoulos on 2023-07-08 11:59_

---
