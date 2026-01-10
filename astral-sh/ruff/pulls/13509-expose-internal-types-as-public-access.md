```yaml
number: 13509
title: Expose internal types as public access
type: pull_request
state: merged
author: zao111222333
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2024-09-25T09:21:27Z
updated_at: 2024-09-26T15:43:56Z
url: https://github.com/astral-sh/ruff/pull/13509
synced_at: 2026-01-10T20:59:36Z
```

# Expose internal types as public access

---

_Pull request opened by @zao111222333 on 2024-09-25 09:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Thanks for providing such a powerful project! 
I try to use `ruff_python_parser` as tokenizer for my toy python REPL project, https://github.com/zao111222333/pyapp.
Everything is so great besides some necessary types/APIs is internal rightnow. And I request expose them. Thanks again!

btw, here is a small example for the python REPL, `ruff_python_parser` is used to highlight & incomplete detect.
![](https://raw.githubusercontent.com/zao111222333/pyapp/refs/heads/main/demo.svg)

## Test Plan

I believe those changes will not change the functionality, so no test plan :)

---

_Review requested from @MichaReiser by @zao111222333 on 2024-09-25 09:21_

---

_Review requested from @dhruvmanila by @zao111222333 on 2024-09-25 09:21_

---

_Comment by @github-actions[bot] on 2024-09-25 09:40_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:75 on 2024-09-25 10:39_

This should not be necessary. You can use the `ruff_python_ast` directly

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:237 on 2024-09-25 10:40_

Can you explain me the use case for adding a `Default` implementation for `Parsed` and `Tokens` ?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:38 on 2024-09-25 10:40_

Same here, it's unclear what the use case for `Default` is. 

---

_@MichaReiser requested changes on 2024-09-25 10:42_

Hi. Nice that you're finding our parser useful. 

I'm open to making `LexicalErrorType` public but I rather not make the other changes.



---

_Closed by @zao111222333 on 2024-09-25 18:59_

---

_Reopened by @zao111222333 on 2024-09-25 19:18_

---

_Comment by @zao111222333 on 2024-09-25 19:27_

> Hi. Nice that you're finding our parser useful.
> 
> I'm open to making `LexicalErrorType` public but I rather not make the other changes.

Hi, thank you for the suggestions. 
I need to store the tokens (`Tokens`/`Vec<Token>`) and errors (`Vec<ParseError>`), the initalization is required at beginning, so I add `Defualt` impl for some internal structs in previous commit.
And now I change to access the `Vec<Token>` and `Vec<ParseError>`, so that I can store the `Vec` and init with `Vec::new()`.
Here is my update

---

_Comment by @MichaReiser on 2024-09-26 06:37_

> Hi, thank you for the suggestions.
> I need to store the tokens (Tokens/Vec<Token>) and errors (Vec<ParseError>), the initalization is required at beginning, so I add Defualt impl for some internal structs in previous commit.
> And now I change to access the Vec<Token> and Vec<ParseError>, so that I can store the Vec and init with Vec::new().
> Here is my update

Do you have a link to some code that I can look at? I would prefer to keep those types as private as possible.

---

_Comment by @zao111222333 on 2024-09-26 07:22_

> Do you have a link to some code that I can look at? I would prefer to keep those types as private as possible.

Sure, pls have a look at:
+ Store the `tokens` & `errors`, update them once the input code is changed
https://github.com/zao111222333/pyapp/blob/main/src/app.rs#L98-L99
https://github.com/zao111222333/pyapp/blob/main/src/app.rs#L163-L172
+ Incomplete detect & indent caculate
https://github.com/zao111222333/pyapp/blob/main/src/app.rs#L110-L151
+ Static code highlight
https://github.com/zao111222333/pyapp/blob/main/src/app.rs#L206-L364




---

_Comment by @zao111222333 on 2024-09-26 07:33_

btw, I have modified my commit, which I believe is not as rude as before. Pls see
https://github.com/astral-sh/ruff/pull/13509/commits/11ab0dbbdb1d901244e6ae941a35eaf86cd2a752

---

_Comment by @MichaReiser on 2024-09-26 08:34_

Thanks for sharing your code. I don't think we need most of these changes.

I suggest that you:

* Change `MyHelper` to store a `Parsed` 
* You can call `parse_unchecked_source("")` to get a "default" parsed. This is slightly more expensive than calling `Parsed::default` (which doesn't exist) but shouldn't be that expensive because the lexer and parser can exit immediately (it's an empty string)

We only need to make `LexicalErrorType` public, which I think is a good change.

---

_Comment by @MichaReiser on 2024-09-26 08:38_

And the repl looks very impressive! 

---

_Label `internal` added by @MichaReiser on 2024-09-26 15:30_

---

_@MichaReiser approved on 2024-09-26 15:30_

---

_Comment by @zao111222333 on 2024-09-26 15:32_

Thank you for your suggestion! I also believe that is a good way to get default parsed. And here is the changes as your suggestion.
Thanks again :)

---

_Comment by @MichaReiser on 2024-09-26 15:34_

Thank you. This looks good

---

_Merged by @MichaReiser on 2024-09-26 15:34_

---

_Closed by @MichaReiser on 2024-09-26 15:34_

---
