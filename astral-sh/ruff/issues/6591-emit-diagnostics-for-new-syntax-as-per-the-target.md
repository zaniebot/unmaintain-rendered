```yaml
number: 6591
title: Emit diagnostics for new syntax as per the target Python version
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - parser
assignees: []
created_at: 2023-08-15T06:42:56Z
updated_at: 2025-03-18T15:13:27Z
url: https://github.com/astral-sh/ruff/issues/6591
synced_at: 2026-01-10T11:09:48Z
```

# Emit diagnostics for new syntax as per the target Python version

---

_Issue opened by @dhruvmanila on 2023-08-15 06:42_

Our parser doesn't take into account the Python version aka [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) setting while parsing the source code. This means that we would allow having a match statement when the target Python version is 3.9 or lower. We want to signal this to the user.

One solution is to provide a diagnostic after the parsing is done. This diagnostic would indicate to the user that there's a syntax usage which isn't valid for the current target version. This is what's being done in Rome and Pyright[^1]. We would still continue linting the file and emit other diagnostics as well.

Following is a non-exhaustive list of syntax changes with the minimum required Python version:

### 3.13
- [x] Type defaults for type parameters ([PEP 696](https://peps.python.org/pep-0696/)) (#16447)

### 3.12
- [x] Formalized grammar for f-strings ([PEP 701](https://peps.python.org/pep-0701/)) (#16543)
- [x] [`type` statements](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement) (#16478)
- [x] [`type` parameter lists ](https://docs.python.org/3/reference/compound_stmts.html#type-parameter-lists) (#16479)

### 3.11
- [x] [`except*` syntax](https://docs.python.org/3/reference/compound_stmts.html#except-star) (#16446)
- [x] Star expressions are allowed in index operations via [PEP 646](https://peps.python.org/pep-0646/#change-1-star-expressions-in-indexes) (#16544)
- [x] Star expressions are allowed in annotations for `*args` via [PEP 646](https://peps.python.org/pep-0646/#change-2-args-as-a-typevartuple) (#16545)

### 3.10
- [x] [`match` statement](https://docs.python.org/3/reference/compound_stmts.html#match) (#16090)

### 3.9
- [x] Relaxing grammar restrictions on decorators ([PEP 614](https://peps.python.org/pep-0614/)) (#16386)
- [x] Parenthesized context managers (formally made part of the language spec in Python 3.10, but usable -- and commonly used -- in Python >=3.9) (#16523)
- [x] Starred unpacking expressions are allowed in `for` statements on Python 3.9+ (officially made part of the language spec in Python 3.11) ([cpython#90881](https://github.com/python/cpython/issues/90881)) (#16558)
- [x] Assignment expressions can now be used unparenthesized in sequence indexes (but not slices) ([What’s New In Python 3.10](https://docs.python.org/3/whatsnew/3.10.html#other-language-changes) (#16404))
  - These also worked on 3.9 when testing manually

### 3.8
- [x] Assignment expressions ([PEP 572](https://peps.python.org/pep-0572/)) (#16383)
- [x] Positional-only parameters ([PEP 570](https://peps.python.org/pep-0570/)) (#16481)
- [x] Parenthesized keyword-argument calls (e.g. `f((a)=1)`) are no longer allowed on Python 3.8+ ([cpython#78822](https://github.com/python/cpython/issues/78822)) (#16482)
- [x] Iterable unpacking in `return` and `yield` statements no longer needs to be parenthesized on Pythonn 3.8+ ([cpython#76298](https://github.com/python/cpython/issues/76298)) (#16485)

[^1]: `Pyright: Type alias statement requires Python 3.12 or newer` 

---

_Comment by @dhruvmanila on 2023-08-15 06:46_

The Match and Type alias statement can be implemented in the [AST Statement analyzer](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/checkers/ast/analyze/statement.rs) within the match arms of the respective statement and checking the [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) setting.

I would suggest to add this as a new rule under the `RUF` category and all of the above would be in the same rule code.

---

_Label `good first issue` added by @dhruvmanila on 2023-08-15 06:47_

---

_Label `rule` added by @dhruvmanila on 2023-08-15 06:47_

---

_Label `good first issue` removed by @dhruvmanila on 2023-08-15 08:21_

---

_Comment by @td-anne on 2023-12-07 09:52_

There might be other new syntax worth addressing:

- [ ] 3.11 `except *`
- [ ] 3.11 Variadic generics
- [ ] 3.8 Walrus operator (if there's interest in supporting pre-3.8)
- [ ] 3.10 Type unions with `|` (?)
- [ ] 3.10 Parenthesized with statements (`with (a_gen() as a, b_gen() as b):`
- [ ] 3.12 type parameter syntax (`def max[T](list[T]) -> T:`)
- [ ] 3.12-only f-strings (?)
- [ ] 3.9 dict unions with `|` (?)
- [ ] 3.9 lower-case `list[]`/`dict[]` in type annotations
- [ ] 3.9 PEP 617 extended generator syntax

Some of these might be closer to functional changes than syntax and hard to detect. But perhaps the easy ones could be folded in? There are advantages to python 3.12 that might make people want to develop with it even though their code is meant to run on python 3.9 (say).

---

_Comment by @s-cork on 2024-04-11 00:11_

> - [ ] 3.8 Walrus operator (if there's interest in supporting pre-3.8)

Just to say we have a use case for targeting 3.7. Thanks for the great work on this!

---

_Comment by @achimnol on 2024-04-22 02:41_

So far, most new syntaxes were not "auto-applied" during formatting, but PEP-701 f-string placeholder updates are auto-applied to all codes. This prevents backporting those codes to older Python versions seamlessly as Ruff does not report syntax errors in older Python versions. Looking forward to see this issue to be resolved. :eyes:

---

_Label `parser` added by @dhruvmanila on 2024-04-29 10:02_

---

_Label `help wanted` added by @charliermarsh on 2024-06-04 15:10_

---

_Comment by @zanieb on 2024-06-04 15:12_

We'd appreciate contributions here if someone is interested in working on the project. We can do this relatively incrementally, but there is some initial complexity to determine how diagnostics are propagated.

@dhruvmanila / @charliermarsh will provide some additional details.

---

_Comment by @dhruvmanila on 2024-06-05 07:58_

> There might be other new syntax worth addressing:

@td-anne For visibility, I've updated the PR description based on your list but this issue is mainly focused on the syntax itself and not the semantic meaning. For example, dictionary union (`{'a': 1} | {'b': 2}`) is valid syntactically but raises a `TypeError` at runtime. So, the PR description is updated with the following:

> * [ ]  3.11 `except *`
> * [ ]  3.8 Walrus operator (if there's interest in supporting pre-3.8)
> * [ ]  3.10 Parenthesized with statements (`with (a_gen() as a, b_gen() as b):`
> * [ ]  3.12 type parameter syntax (`def max[T](list[T]) -> T:`)
> * [ ]  3.12-only f-strings (?)

But, the following can possibly be done as a separate rule and is not part of what we're proposing here.
> * [ ]  3.11 Variadic generics
> * [ ]  3.10 Type unions with `|` (?)
> * [ ]  3.9 dict unions with `|` (?)
> * [ ]  3.9 lower-case `list[]`/`dict[]` in type annotations

Can you expand on the following? I don't see any new syntax related to PEP 617.
> * [ ]  3.9 PEP 617 extended generator syntax



---

_Comment by @AlexWaygood on 2024-06-05 08:05_

> [ ] 3.11 Variadic generics

These did involve the introduction of some new syntax: unpacking using `*` at the outermost level of an expression in a type annotation was invalid syntax on earlier versions of Python. See https://peps.python.org/pep-0646/#grammar-changes

---

_Comment by @dhruvmanila on 2024-06-05 08:09_

I see. Thanks for pointing that out. I'll update the PR description.

---

_Comment by @dhruvmanila on 2024-06-06 10:30_

The goal here is to update Ruff to detect syntax which is invalid as per the [`target-version`](https://docs.astral.sh/ruff/settings/#target-version). There are two ways to achieve this:

1. Update the parser to detect this, add new parse error types
2. Add a new single rule

For (1), 
* There needs to be additional changes to update the linter to show all parse errors as diagnostics. Currently, only the first parse error is shown to the user. This will be updated by me soon. 
* Pyright also uses this approach.
* The parser has all the context. For example, parentheses context for parenthesized with-items

For (2),
* It can utilize the infrastructure provided by the linter including ignoring it via `noqa` comment
* It's separate from the parser itself and can plug into the diagnostic system directly
	* This has an added benefit that the formatter will still run even if an unsupported syntax is being used.
	* On the other hand, f-string formatting might become simpler because the formatter doesn't need to consider the target version

I'm more leaning towards (2). An open question here is to whether implement this as a new rule or add it to `E999`. The new rule can be added under the `ruff` group with the name as `unsupported-syntax`.

---

_Comment by @AlexWaygood on 2024-06-06 10:40_

I support detecting these syntax errors via the linter (Option (2)) wherever possible. I think it'll be great to have these errors be `# noqa`-able and to be able to plug into the diagnostic system directly. However, I don't necessarily see a need to be dogmatic about it -- if there are specific syntax errors (such as parenthesized `with` items) that would be much easier to detect in the parser, then possibly we could detect some in the parser.

I'd much prefer to have these syntax errors be detected as part of `E999` rather than adding a new lint rule. Although we'll be using a different part of ruff to detect them, from the perspective of users a syntax error is a syntax error. I think it will be pretty confusing for them if some syntax errors are detected by one rule and other syntax errors are detected by another rule. The effect of a syntax error for a user is always the same -- your code is never going to be compiled by Python, let alone run!

---

_Comment by @MichaReiser on 2024-06-06 12:38_

> I'd much prefer to have these syntax errors be detected as part of E999 rather than adding a new lint rule. Although we'll be using a different part of ruff to detect them, from the perspective of users a syntax error is a syntax error. I think it will be pretty confusing for them if some syntax errors are detected by one rule and other syntax errors are detected by another rule. The effect of a syntax error for a user is always the same -- your code is never going to be compiled by Python, let alone run!

I think one key difference between the two is that you should never suppress a syntax error that is an error regardless of the python version (I don't even know if we support suppressing them). I don't know if there are good reasons for suppressing python-version specific syntax errors but I could see an argument for that. I don't think these should be suppressed by inline noqa comments but a user might decide to suppress them with `per-file-ignores` (although a new pyrpoject.toml with the right requires-python version would be better)

---

_Comment by @AlexWaygood on 2024-06-06 12:43_

The only reasons I can see for wanting to suppress a syntax error diagnostic are:
- Deliberate syntax errors (e.g. in test files or data files)
- A bug in Ruff, where it incorrectly reports a syntax error even though there isn't one

If you have the right target version in your config file, I can't _personally_ see any reason why you'd want to be able to suppress Python-version-specific syntax error diagnostics, or treat them any differently from syntax that's invalid on all Python versions.

---

_Comment by @achimnol on 2024-08-03 10:25_

Any updates on this? We are still holding back the ruff version due to the f-string backporting issue.

---

_Comment by @InSyncWithFoo on 2025-01-15 18:37_

Now that [`E999`](https://docs.astral.sh/ruff/rules/syntax-error/) has been removed, such diagnostics should probably be emitted from within the parser, which currently does not have access to the targeted Python version.

[`PythonVersion` the enum](https://github.com/astral-sh/ruff/blob/4f3209a3ec8993b2df0c2f8e53324121671d9863/crates/ruff_linter/src/settings/types.rs#L43-L56) is instead defined in the `ruff_linter` crate. Should it be exposed to or moved to `ruff_python_parser` instead?

---

_Comment by @MichaReiser on 2025-01-15 19:10_

There are a few open design questions on where the best place is for those checks to be implemented. A very likely outcome is that some are implemented in the parser while others are implemented in the linter. @ntBre plans to work on this soon and can then advise on where we want to implement which checks

---

_Label `help wanted` removed by @AlexWaygood on 2025-01-15 19:11_

---

_Assigned to @ntBre by @ntBre on 2025-02-05 22:08_

---

_Comment by @ntBre on 2025-03-18 15:13_

#16543 was the last error being tracked here, so I think we can close this! 🎉 

---

_Closed by @ntBre on 2025-03-18 15:13_

---
