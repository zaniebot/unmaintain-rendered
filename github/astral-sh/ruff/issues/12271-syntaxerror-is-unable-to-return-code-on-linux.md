---
number: 12271
title: SyntaxError is unable to return code on Linux
type: issue
state: closed
author: WutingjiaX
labels:
  - question
assignees: []
created_at: 2024-07-10T11:42:29Z
updated_at: 2024-07-15T08:53:07Z
url: https://github.com/astral-sh/ruff/issues/12271
synced_at: 2026-01-07T13:12:15-06:00
---

# SyntaxError is unable to return code on Linux

---

_Issue opened by @WutingjiaX on 2024-07-10 11:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
"SyntaxError", ""code": null", "linux", "Debian"

I run the **same** code on MacOS and Linux(Debian GNU/Linux 10 (buster)), with the cmd: 
```
ruff check --output-format=json test.py
```
ruff version 0.5.1

The code in the _test.py_ is any Python code that may cause syntax errors， such as：
```
import .
```
the output on linux is:
```
[
  {
    "cell": null,
    "code": null,
    "end_location": {
      "column": 9,
      "row": 1
    },
    "filename": "/home/code/test.py",
    "fix": null,
    "location": {
      "column": 8,
      "row": 1
    },
    "message": "SyntaxError: Expected an import name",
    "noqa_row": null,
    "url": null
  },
  {
    "cell": null,
    "code": null,
    "end_location": {
      "column": 1,
      "row": 2
    },
    "filename": "/home/code/test.py",
    "fix": null,
    "location": {
      "column": 9,
      "row": 1
    },
    "message": "SyntaxError: Expected one or more symbol names after import",
    "noqa_row": null,
    "url": null
  }
]
```

but on MacOS,the output is:
```
[
  {
    "cell": null,
    "code": "E999",
    "end_location": {
      "column": 9,
      "row": 1
    },
    "filename": "/Users/bytedance/Library/Application Support/JetBrains/PyCharm2023.3/light-edit/test.py",
    "fix": null,
    "location": {
      "column": 8,
      "row": 1
    },
    "message": "SyntaxError: Expected an import name",
    "noqa_row": 1,
    "url": "https://docs.astral.sh/ruff/rules/syntax-error"
  }
]%
```

I can't understand why the output is different (especially if one code field has a value and the other doesn't)




---

_Comment by @MichaReiser on 2024-07-10 12:03_

I suspect that you're using two different Ruff versions. Can you try running `ruff --version` on both systems and verify that they're the same?

---

_Comment by @WutingjiaX on 2024-07-10 13:03_

> I suspect that you're using two different Ruff versions. Can you try running `ruff --version` on both systems and verify that they're the same?

@MichaReiser oh！you're right，my local version on MacOS is 0.4.9 （though I have the same pyproject.toml）， But why is it that there is no code in higher versions instead

---

_Comment by @MichaReiser on 2024-07-10 13:11_

This has been changed as part of 0.5. For more details, see the [announcement blog post](https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors). Do you have a specific use for the error code?

---

_Closed by @MichaReiser on 2024-07-10 13:11_

---

_Reopened by @MichaReiser on 2024-07-10 13:12_

---

_Comment by @WutingjiaX on 2024-07-11 03:00_

@MichaReiser 
Thank you for your answer！If the code can be null, it needs some consideration when deserializing (such as using pydantic,it should be optional instead of string). But this is not a big problem



---

_Closed by @WutingjiaX on 2024-07-11 03:00_

---

_Comment by @dhruvmanila on 2024-07-11 03:15_

> @MichaReiser Thank you for your answer！If the code can be null, it needs some consideration when deserializing (such as using pydantic,it should be optional instead of string). But this is not a big problem

Yeah, I think I should've added something around the fact that the type of `cell` and `noqa_row` is now `string | null` in the output formats. Sorry for the confusion!

---

_Label `question` added by @dhruvmanila on 2024-07-11 03:16_

---

_Comment by @kaste on 2024-07-12 19:13_

This change breaks the Sublime Text plugin which assumed "code: str".  https://github.com/kaste/SublimeLinter-contrib-ruff/issues/2

This is easy to fix and I have to anyway but in Sublime Text the "rule name" aka "code" actually matters as the "diagnostics" can be styled per linter name, per error-type ("severity"), and per rule-name.  E.g. I literally use a bomb icon for E999 errors.

(Why is that an Optional[str] anyway.  If I do `item["code"]`, I ask what the code is and since there is a key, you basically answer: Yeah sure I have the code, it's ... a nah ... don't know.)


---

_Comment by @dhruvmanila on 2024-07-15 08:53_

> (Why is that an Optional[str] anyway. If I do `item["code"]`, I ask what the code is and since there is a key, you basically answer: Yeah sure I have the code, it's ... a nah ... don't know.)

You can find some context in the [blog post](https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors) and some discussion in the [original PR](https://github.com/astral-sh/ruff/pull/11901) but the tldr is that the rule itself is deprecated i.e., `E999` doesn't exists in Ruff anymore. This means we can't use the code anywhere. Syntax errors are now always shown and one cannot select / ignore them. Regarding the type, it's `Optional[str]` because it's the value that's optional and not the key itself. The key will always be present but as there's no rule code for a syntax error, it will be `null` instead.

---
