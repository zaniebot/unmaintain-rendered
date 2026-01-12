```yaml
number: 11455
title: Ruff applies auto-fix in files with syntax errors
type: issue
state: closed
author: Dobatymo
labels:
  - linter
assignees: []
created_at: 2024-05-17T08:06:01Z
updated_at: 2024-07-02T08:52:53Z
url: https://github.com/astral-sh/ruff/issues/11455
synced_at: 2026-01-12T15:54:51Z
```

# Ruff applies auto-fix in files with syntax errors

---

_@Dobatymo_

When running `ruff format` ruff will not modify files if it encounters a `SyntaxError` (which is expected I think). However running `ruff check --fix` will encounter the `SyntaxError` as well, but will continue to "fix" and modify the files. I don't know if this is expected, but I find that behaviour surprising.

* List of keywords you searched for before creating this issue: fix, syntaxerror

Code snippet:
```python
typedef struct _STORAGE_DEVICE_NUMBER_EX {
    ("PartitionNumber", ULONG),
} STORAGE_DEVICE_NUMBER_EX, *PSTORAGE_DEVICE_NUMBER_EX;
```
`ruff check --fix` with remove the `;` at the end of the line (which I didn't expect since the file is not actually Python.

Version: ruff 0.4.4 (3e8878a1c 2024-05-09)

---

_Comment by @dhruvmanila on 2024-05-17 08:11_

Can you provide some more details? How are you running Ruff? Is it from the command-line or is it via an editor? If it is in an editor context, could it be that it's another extension which is removing the semicolon?

---

_Label `needs-info` added by @dhruvmanila on 2024-05-17 08:11_

---

_Comment by @Dobatymo on 2024-05-17 08:31_

Just using the normal command line. Windows 11. I made sure it must be ruff which modifies the file.

```
error: Failed to parse syntaxerror.py:1:9: Simple statements must be separated by newlines or semicolons
syntaxerror.py:1:9: E999 SyntaxError: Simple statements must be separated by newlines or semicolons
Found 2 errors (1 fixed, 1 remaining).
```

That's the output.

---

_Renamed from "ruff check --fix modifies files with syntaxerror" to "ruff check --fix modifies files with SyntaxError" by @Dobatymo on 2024-05-17 08:49_

---

_Comment by @dhruvmanila on 2024-05-17 12:19_

Ruff uses the file extension to determine whether it's a Python file or not as otherwise it's difficult to reliably determine whether a file contains Python source code or not. It seems that the file name is `syntaxerror.py` which has the `.py` extension.

Can you provide us any context as to why do you have a non-Python source code in a Python file?

---

_Comment by @Dobatymo on 2024-05-17 13:56_

I am working on simple C header to Python ctypes translator. And I was simply using ruff format to check if the files I generated are syntactically correct. I know I could have just used the python builtin parser, but having a nicely formatted file when the translation produced a syntactically valid file was a nice side effect.
Then for further investigation if the generated files are correct, I can `ruff check --fix` instead and was surprised to see that the file was modified even when it wasn't syntactially valid (something `ruff format` didn't do). So if this is expected behaviour this should be documented, but in my opinion it should not do any modifications (just like `ruff format`).

---

_Comment by @zanieb on 2024-05-17 17:56_

I think we'll move _more_ towards Ruff working on files with syntax errors. I'd recommend either excluding these files from lint checks or selecting `E999` to only check for syntax errors.

---

_Comment by @charliermarsh on 2024-05-18 17:30_

I honestly thought we didn't apply fixes when a file contains a syntax error (but looks like I'm wrong). I would be open to changing the behavior, not sure what @dhruvmanila thinks.

---

_Comment by @dhruvmanila on 2024-05-20 05:24_

I think this is the side effect of non-AST based rules which work with tokens, logical lines, etc. And, that Ruff checks all of the tokens up to the first error token. In your case, the [code produces a valid token stream](https://play.ruff.rs/a3fdde43-21a3-45f0-9a32-378a9655db69) and so it checks for violations which uses the token stream. Ruff doesn't know that this isn't a syntactically valid code unless it's processed by the parser.

> Then for further investigation if the generated files are correct, I can `ruff check --fix` instead and was surprised to see that the file was modified even when it wasn't syntactially valid (something `ruff format` didn't do).

The reason `ruff format` doesn't do this is because it works with the AST and the code you mentioned is invalid syntactically.

> I honestly thought we didn't apply fixes when a file contains a syntax error (but looks like I'm wrong).

This isn't exactly possible today because unless the parser processes the token stream, Ruff can't know whether it contains a syntax error. And, the code provided by the author produces a valid token stream. This will change during this week when we combine the lexing and parsing step and then Ruff can get this knowledge.

I'm not opposed to this idea although this does mean that we'd stop fixing code like the following:

```py
x;

# unterminated f-string
f"hello
```

In the future, we _do_ want to make Ruff capable of applying a fix even if the source code contains syntax error. This will be done after we expand Ruff's capabilities to allow diagnosing issues in a file containing syntax errors.

**td;dr** I'm more in favor of documenting this behavior rather than disallowing to generate fixes.

---

_Comment by @Dobatymo on 2024-05-20 07:11_

Better documentation sounds good!

---

_Label `needs-info` removed by @dhruvmanila on 2024-05-20 07:23_

---

_Label `documentation` added by @dhruvmanila on 2024-05-20 07:23_

---

_Comment by @dhruvmanila on 2024-05-20 07:30_

This seems like good place for the content to be in: https://docs.astral.sh/ruff/linter/#fixes

---

_Comment by @dhruvmanila on 2024-07-01 09:22_

I think we might have to disable fixes for source code with syntax errors with #11950 because otherwise Ruff could panic or go into infinite auto-fix loop where first auto-fix creates a need for the second auto-fix which then creates a need for the first auto-fix again. This infinite loop can be created quite easily when there's a syntax error. For example, https://github.com/astral-sh/ruff/issues/12094 or between `E226` and `E201` for `(/`.

---

_Comment by @MichaReiser on 2024-07-01 09:26_

Disabling fixes for token-based rules makse sense to me, unless we have a way to know whether a token range overlaps with a parser error range. 

Long term, I think fixing files with syntax errors would be nice but only if the specific code doesn't overlap with a syntax error. 

---

_Comment by @dhruvmanila on 2024-07-01 10:53_

> Disabling fixes for token-based rules makse sense to me, unless we have a way to know whether a token range overlaps with a parser error range.

This seems like a good idea although I'm not sure if that could cover all cases. I guess it might be more useful to look at the logical line for token-based rules and the enclosing AST node for AST-based rules.

---

_Comment by @dhruvmanila on 2024-07-01 11:02_

> Disabling fixes for token-based rules makse sense to me

I was more thinking of disabling fixes for _all_ rules and not just token-based although it might be ok to keep the fix for line-based rules on.

---

_Renamed from "ruff check --fix modifies files with SyntaxError" to "Ruff applies auto-fix in files with syntax errors" by @dhruvmanila on 2024-07-01 11:16_

---

_Label `documentation` removed by @dhruvmanila on 2024-07-01 11:17_

---

_Label `linter` added by @dhruvmanila on 2024-07-01 11:17_

---

_Closed by @dhruvmanila on 2024-07-02 08:52_

---

_Closed by @dhruvmanila on 2024-07-02 08:52_

---
