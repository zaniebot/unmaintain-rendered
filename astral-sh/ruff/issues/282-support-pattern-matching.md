```yaml
number: 282
title: Support pattern matching
type: issue
state: closed
author: sobolevn
labels: []
assignees: []
created_at: 2022-09-29T09:26:20Z
updated_at: 2023-02-22T14:28:57Z
url: https://github.com/astral-sh/ruff/issues/282
synced_at: 2026-01-10T11:09:42Z
```

# Support pattern matching

---

_Issue opened by @sobolevn on 2022-09-29 09:26_

As I am learning the source code, it seems to me that python's pattern matching is not supported at all.

We have a visitor ready: https://github.com/charliermarsh/ruff/blob/c7349b69c12b7511d48fc45390eac8d7282062a5/src/ast/visitor.rs#L527-L571

But, that's about it.

Things to support (feel free to update):
- [ ] Unused variables check
- [ ] Variable names check (`MatchAs` pattern)
- [ ] Custom rules, like do not match `float`s



---

_Comment by @charliermarsh on 2022-09-29 19:24_

Yeah it makes some appearances in the RustPython AST but it doesn't seem to be supported yet by the actual parser.

---

_Comment by @woodruffw on 2023-01-10 04:13_

The parser in question is `rustpython-parser`, correct? No promises, but I can take a look at its current state and see whether I can contribute support for the `match` syntax.

---

_Comment by @woodruffw on 2023-01-10 04:19_

Oh, looks like someone beat me to it :slightly_smiling_face: https://github.com/RustPython/RustPython/pull/4323

---

_Comment by @charliermarsh on 2023-01-10 04:23_

Yeah that's the one! There's been a little bit of progress in that PR. There's also someone who's been looking to migrate the entire parser to `rust-peg` (https://github.com/RustPython/RustPython/pull/4423) which would be a much bigger change. Then @andersk had augmented the parser to support parenthesized context managers (https://github.com/RustPython/RustPython/pull/4352) and had mentioned perhaps looking into match statements next. 

---

_Comment by @charliermarsh on 2023-01-14 15:16_

I'd love to sponsor someone (financially, that is) to work on this issue (in RustPython). If you're interested, shoot me a DM on [Twitter](https://twitter.com/charliermarsh) or email me at charlie.r.marsh@gmail.com :)

---

_Comment by @jad-haddad on 2023-01-31 10:35_

Actually when there is a `match` expression in a python module, ruff fails silently?
I spent an hour debugging why ruff isn't sorting my imports even though the iSort rules are enabled, when I commented out the match expression ruff was able to sort again, is that expected?

---

_Comment by @charliermarsh on 2023-01-31 12:25_

@JadHADDAD92 - So sorry that you lost time to this. It _is_ the expected behavior, because syntax errors have their own error code (`E999`). So if you don't have that code enabled (e.g., if you do `ruff foo --select I`), you won't see those errors at all. It's consistent with how Ruff works, but I agree it's confusing. (Flake8 does have the same behavior.)

A few alternatives:

1. Always log an error (separate from a lint violation) if we fail to parse a file. It's hard to imagine this being unhelpful ever.
2. Always include `E999` even if it's not explicitly enabled. This is tricky and complicates the rule-selection semantics.


---

_Comment by @jad-haddad on 2023-01-31 12:45_

I find the first alternative to be the better behavior, because I presume all ruff rules depend on a successful parse of RustPython (right?) therefore if we lint a module which contains a syntax error (failed to parse), and ruff returns 0 as if the file is well linted, it brings no clarity of what happened.

---

_Comment by @charliermarsh on 2023-01-31 12:54_

Not all rules depend on a successful parse. Some can work off the token stream, or even partial stream. Some rules only look at raw lines (like the line-length violations). And some work off the filesystem (e.g., the rule to detect missing `__init__.py` files). The majority do, but some rules can complete without one.

We could: always log an `error` message if a file fails to parse + exit 1.


---

_Comment by @jad-haddad on 2023-02-02 09:36_

@charliermarsh is there an issue for this (logging an error when failing to parse) that I can subscribe to?

---

_Comment by @charliermarsh on 2023-02-02 13:01_

@JadHADDAD92 - No but thank you for the reminder -- I just created #2473, hopefully I can knock it out today.

---

_Comment by @ofek on 2023-02-11 17:28_

What direction are we going in order to support this?

---

_Comment by @charliermarsh on 2023-02-18 04:23_

I'm working on it here: https://github.com/RustPython/RustPython/pull/4519. There are a few problems to solve but that implementation already supports many of the variants.

---

_Comment by @charliermarsh on 2023-02-19 20:22_

I'm making good progress on this (I think?). If anyone wants to link to an OSS codebase that makes good use of match statements (for testing), it'd be appreciated.

---

_Comment by @phillipuniverse on 2023-02-19 21:34_

@charliermarsh there are good match examples in the 2nd edition Fluent Python book. [This snippet](https://github.com/fluentpython/example-code-2e/blob/28d6d033156831a77b700064997c05a40a83805f/18-with-match/lispy/py3.10/lis.py#L145) looks like it would catch some edge cases. In fact, if you [search in that repo for the word “match”](https://github.com/fluentpython/example-code-2e/search?q=match&type=) you’ll get examples of that word being used as a keyword as well as a normal variable.

---

_Comment by @SRv6d on 2023-02-19 21:36_

Pyright has some good examples.
https://github.com/microsoft/pyright/blob/main/packages/pyright-internal/src/tests/samples/match1.py for example or any other `match*.py` file in that directory.

---

_Comment by @ofek on 2023-02-19 22:05_

Also test `if match := re.search(...):`

---

_Comment by @henribru on 2023-02-19 22:12_

Python's own test suite for pattern matching seems like a good litmus test: https://github.com/python/cpython/blob/main/Lib/test/test_patma.py

https://github.com/brandtbucher/patmaperformance also has some examples.

---

_Comment by @charliermarsh on 2023-02-19 23:26_

Parser looking good so far. I got it to parse Black's fixtures which cover a lot of tricky cases.

---

_Comment by @charliermarsh on 2023-02-19 23:27_

(It also handles soft keywords as pointed out by @ofek. I've also run it over Hatch, Airflow, and Pandas, and confirmed that the output is unchanged, so it's not incorrectly detecting any `match` statements from e.g. variable assignments.)

---

_Comment by @charliermarsh on 2023-02-19 23:28_

(Oh, I guess the Black test suite is a subet of `test_patma.py` :))

---

_Comment by @charliermarsh on 2023-02-20 04:07_

I expect this to go out some time this week, hopefully in the next few days. The PR is up as #3047 (plus depends on https://github.com/RustPython/RustPython/pull/4519 in RustPython).

---

_Closed by @charliermarsh on 2023-02-21 18:52_

---

_Comment by @danstewart on 2023-02-22 11:11_

Thank you!

Heads up for anyone else using the VS code extension you will need to set your ruff path:
```
"ruff.path": ["./.venv/bin/ruff"]
```

Looks like the extension has it's own ruff executable, which [has already been updated](https://github.com/charliermarsh/ruff-vscode/pull/137) to use the latest ruff but hasn't been released yet.

---

_Comment by @mikeroll on 2023-02-22 11:20_

@danstewart most likely you want this instead:
```
"ruff.importStrategy": "fromEnvironment"
```
which will find `ruff` in whatever the venv associated with the vscode workspace is, without having to hardcode the path.

---

_Comment by @danstewart on 2023-02-22 11:22_

Thanks, that's much better.

---

_Comment by @charliermarsh on 2023-02-22 14:28_

@danstewart - I cut a new pre-release last night that includes v0.0.251, so you can pull it in if you switch to the pre-release channel. I'll probably cut a main-channel release today or tomorrow.

---
