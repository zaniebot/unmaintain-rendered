---
number: 11056
title: Improve backport friendliness of quotes in Python 3.12 f-string placeholders
type: issue
state: closed
author: achimnol
labels:
  - formatter
assignees: []
created_at: 2024-04-20T13:01:27Z
updated_at: 2024-11-05T22:30:23Z
url: https://github.com/astral-sh/ruff/issues/11056
synced_at: 2026-01-10T01:22:50Z
---

# Improve backport friendliness of quotes in Python 3.12 f-string placeholders

---

_Issue opened by @achimnol on 2024-04-20 13:01_

I'm maintaining a codebase with multiple release branches targeting different Python versions by release. The latest one uses Python 3.12 while prior versions use Python 3.10 or 3.11.

The recent update of Ruff has introduced Python 3.12 f-string support and the Ruff v0.4 formatter replaces all single-quotes in placeholders to double-quotes.

Example:
`f"a value from dict: {data['key']}"` (the previous way to write string literals in f-string placeholdrs)
→
`f"a value from dict: {data["key"]}"` (allowed in Python 3.12 but a syntax error in Python 3.11 or older)

The problem is that when I **backport** this line to a release branch targeting Python 3.11 or older (but using the same Ruff version), Ruff *silently ignores* the broken syntax. It does not auto-convert the existing codes in Python 3.11, but also does not give errors about the backported codes. Linting will pass, but it will fail when someone executes/imports the code.

In contrast, Black (24.4) loudly fails because the Python parser reports a syntax error.

There could be the following options to resolve the issue:
- Let Ruff 0.4 targeting Python 3.11 or older report the syntax error loudly, just like Black.
- Let Ruff 0.4 targeting Python 3.11 or older auto-convert the double-quotes in f-string placeholders back to single-quotes (or vice-versa depending on the quote style).
- Add an option to keep the single-quotes in f-string placholders intact in Python 3.12 or later.


---

_Renamed from "Option to preserve single-quotes in placeholders of f-strings to allow backporting Python 3.12 codes to Python 3.11" to "Improve backport freidliness of quotes in Python 3.12 f-string placeholders" by @achimnol on 2024-04-20 13:06_

---

_Renamed from "Improve backport freidliness of quotes in Python 3.12 f-string placeholders" to "Improve backport friendliness of quotes in Python 3.12 f-string placeholders" by @achimnol on 2024-04-20 13:06_

---

_Comment by @MichaReiser on 2024-04-20 13:38_

Thanks for the detailed report and we're sorry that you're experiencing this. I think backward compatibility could actually be the reason for changing the default to use single quotes in nested f-strings (@dhruvmanila). 

We have plans to implement lint rules that allow catching unsupported syntax depending on the configured python version. See https://github.com/astral-sh/ruff/issues/6591

---

_Comment by @dhruvmanila on 2024-04-22 09:34_

Thank you for raising this issue!

> I think backward compatibility could actually be the reason for changing the default to use single quotes in nested f-strings (@dhruvmanila).

Yeah, I think that's a strong reason to not change the quotes and it should be the case until the parser supports target version.

---

_Label `formatter` added by @dhruvmanila on 2024-04-22 09:35_

---

_Comment by @dhruvmanila on 2024-04-22 09:39_

Reading through the suggested solutions, (1) would be done by #6591, I don't think (2) is ideal because that could be complicated.

I think for now it's reasonable to avoid changing the quotes even for 3.12 target version.

Related: https://github.com/astral-sh/ruff/issues/10273, https://github.com/astral-sh/ruff/issues/10184

---

_Referenced in [lablup/backend.ai#2053](../../lablup/backend.ai/pulls/2053.md) on 2024-04-23 07:40_

---

_Comment by @achimnol on 2024-04-23 07:44_

(1) should suffice my needs, because developers could be warned explicitly about the breakage.
(3) (avoiding changing the quotes) would be a good option for us as well.

---

_Comment by @dhruvmanila on 2024-04-23 08:14_

Regarding (1), I'm not sure when it would be picked up but ideally it could be after #1774 and before I start the work on error resilience in the parser.

Regarding (3), I think I'd like avoid adding a config option before finalizing the [f-string formatting style](https://github.com/astral-sh/ruff/discussions/9785). It's currently in preview.

---

_Comment by @mvaled on 2024-05-02 07:08_

I consider this a bug if `target-python` is set to py311 or older.

For instance:

```
$ cat src/ruffformatter/__init__.py 
def hello() -> str:
    return f"Hello {"a" + "b"}"


$ cat pyproject.toml 
[project]
name = "ruffformatter"
version = "0.1.0"
description = "Add your description here"
dependencies = []
readme = "README.md"
requires-python = ">= 3.8"
license = { text = "MIT" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.rye]
managed = true
dev-dependencies = [
    "ruff==0.4.2",
]

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/ruffformatter"]

[tool.ruff]
target-version = "py311"

$ rye run ruff format src/
1 file left unchanged
```

But the file is not syntactically valid in Python 3.11:

```
$ rye run python --version
Python 3.11.7

$ rye run python src/ruffformatter/__init__.py 
  File "/home/manu/tmp/ruffformatter/src/ruffformatter/__init__.py", line 2
    return f"Hello {"a" + "b"}"
                     ^
SyntaxError: f-string: expecting '}'
```

---

_Comment by @MichaReiser on 2024-05-02 07:17_

@mvaled What I see from the output is that the formatter didn't change the file. The formatter won't change the quote style when targeting python 3.11 or older. That means it's up to you to use the right quotes. 

---

_Comment by @mvaled on 2024-05-02 12:21_

@MichaReiser But neither `ruff check` catches that as an error.

IMO, either `ruff format` or `ruff check` should treat that as error.  Since Python reports that as syntax error.

---

_Comment by @MichaReiser on 2024-05-02 13:16_

Agree, that's planned in https://github.com/astral-sh/ruff/issues/6591 that @dhruvmanila mentioned above

---

_Comment by @charliermarsh on 2024-05-02 17:01_

Yeah we don't enforce syntax errors right now -- that's known, and isn't a bug but rather a limitation (we just haven't implemented it). What _would_ be a bug is if we _changed_ your code to use syntax that isn't supported by your target version.

---

_Referenced in [lablup/backend.ai#2826](../../lablup/backend.ai/pulls/2826.md) on 2024-09-10 06:48_

---

_Comment by @achimnol on 2024-09-10 07:54_

> What would be a bug is if we changed your code to use syntax that isn't supported by your target version.

PEP-701 f-string literals are **auto-applied** to the Python 3.12 codes and they breaks up things when backported _as-is_.  The problem is that Ruff just applies the quotation change without giving any option to skip the change or proactive checks when executed in prior target versions.  For instance, the pattern matching syntax are not auto-applied but the user should deliberately rewrite existing codes, meaning that the user may simply defer introduction of it regardless of the Ruff/Python versions.  But we do not have such freedom of control for f-strings.  If this were a matter of style, it would be fine, but it is a breaking syntax error...

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Comment by @MichaReiser on 2024-10-18 13:55_

@dhruvmanila and I talked about this in our 1:1 and I think there are good reasons to changing our current default:

* alternating quotes eases copying code between projects regardless of the target python version
* Alternating quotes is probably what all projects use today and what the community considers to be the standard. Using alternating quotes reduces the diff size
* I do find alternating quote styles easier to parse. It's easier to match the quotes. 

---

_Comment by @MichaReiser on 2024-10-20 11:30_

The exception to the above behavior should be if flipping the quotes reduces the number of necessary escapes:

```python
f"{f"this isn't using single quotes because it contains single quotes that would need escaping"}"}"
```

Keeping the same quotes here should be fine because this syntax is Python 312 or newer

---

_Referenced in [astral-sh/ruff#13860](../../astral-sh/ruff/pulls/13860.md) on 2024-10-21 14:28_

---

_Closed by @MichaReiser on 2024-10-23 05:57_

---

_Comment by @matthewlloyd on 2024-11-05 19:40_

Hi team, I've been using Ruff format with target version 3.12 in preview mode for several months and had converted all of my code to take advantage of Python 3.12's support for nesting double quotes within double-quoted f-strings. After upgrading to the latest Ruff version, I noticed it’s reverting all my nested double quotes to single quotes inside double-quoted f-strings, following the style used in earlier Python versions.

While I understand the rationale for alternating quotes as the default style, I personally prefer using consistent double quotes throughout, and I enjoyed finally not having to switch quotes inside f-strings. The ability to maintain consistent quotes while writing f-strings was considered an improvement in 3.12, and I find some advantages to be:

- Reduced cognitive load. You don't have to remember to switch quotes, it's one less muscle memory to maintain, it reduces mental burden and cognitive overhead, and it simplifies the syntax and improves readability (in my opinion).
- Seamless copying and pasting: Expressions within f-strings can be cut and pasted freely without needing to adjust quote styles.

Would it be possible for you to make this quote styling configurable? This flexibility would be very useful for developers who prefer consistent double quotes, especially with Python 3.12+'s new f-string support.

Re: https://github.com/astral-sh/ruff/pull/13860

---

_Comment by @MichaReiser on 2024-11-05 19:54_

Hy @matthewlloyd I'm sorry that you experienced this churn. 

I wont argue about readability. What's more readable is very personal. 

> Seamless copying and pasting: Expressions within f-strings can be cut and pasted freely without needing to adjust quote styles.

I think that should still be given because the formatter will change the quotes for you. 

> Reduced cognitive load. You don't have to remember to switch quotes, it's one less muscle memory to maintain, i

Is this about using nested quotes? Because you can write the f-string with whatever quotes you prefer without worrying if they're the right quotes. The formatter will change the quotes for you.

---

_Comment by @matthewlloyd on 2024-11-05 20:31_

@MichaReiser Thanks for the reply, no worries. There are two separate issues here I think.

1. Cognitive burden on the _reading_ side. 
    - Consistent quotes within f-strings relieve the burden of mentally parsing and switching between quote types within nested expressions.
    -  Consistent quotes allow for uniform formatting, making the f-string structure clearer and reducing interruptions to the reader's focus on the logic and content of expressions rather than the syntax, particularly in longer or more intricate strings with nested quotes.
    - Codebases benefit from uniform use of quote types, which aligns with stylistic preferences and guidelines in Python without introducing special cases for f-strings.
1. Cognitive burden on the _writing_ side. Of course, yes, as long as the code will parse (which it will either way), the formatter will fix whatever the developer writes. However, there is still overhead.
      - Avoiding the need to alternate between single and double quotes decreases the risk of syntax errors, especially in complex expressions or multi-layered interpolations, when modifying code that has already been formatted.
      - Cognitive overhead from the inconsistency between the quote styles in the newly written code and the rest of the code, between the time the new code is written and the time it gets formatted. This may be a short time if the developer frequently reformats the code, or it may be a longer time if it doesn't get formatted until it gets committed.
      - For those using e.g. pre-commit hooks, procedural overhead from the greater likelihood of a diff, and the additional noise generated in a diff, when the formatter does its work. This may require the developer to perform additional steps, e.g. review and stage the formatter's changes.

I think it's unfortunate to have essentially "lost" one of the improvements introduced in Python 3.12 for Ruff-formatted code merely for the sake of maintaining backwards compatibility with older versions of Python.

---

_Comment by @MichaReiser on 2024-11-05 20:46_

Thanks for the added context.

I'm hesitant about introducing a new setting to configure f-string quotes unless there's a huge demand for it (not saying that your concerns aren't valid).  And I still think that the current behavior is better. For example, changing quotes also breaks syntax highlighting in many tools. But I'm also happy to revisit the default if that's where the community stands. 

I suggest that we create a new issue for this so that we can track this better. @matthewlloyd 


---

_Comment by @matthewlloyd on 2024-11-05 22:30_

> For example, changing quotes also breaks syntax highlighting in many tools.

I would note that to the extent this is an issue, it is a temporary one. The two most popular Python IDEs, VS Code and PyCharm/IntelliJ, accounting for more than 70%+ of the market share, already support the new syntax.

Likewise, choosing one style over another for reasons of backwards compatibility with older versions of Python is a decision that won't age well because pre-3.12 versions will be end-of-life after October 2027.

> I suggest that we create a new issue for this so that we can track this better. @matthewlloyd

Great, I'll open an issue, thank you.

---

_Referenced in [astral-sh/ruff#14118](../../astral-sh/ruff/issues/14118.md) on 2024-11-05 22:46_

---

_Referenced in [lablup/backend.ai#3347](../../lablup/backend.ai/issues/3347.md) on 2025-01-02 13:47_

---

_Referenced in [astral-sh/ruff-action#46](../../astral-sh/ruff-action/issues/46.md) on 2025-01-16 21:55_

---

_Referenced in [astral-sh/ruff#16385](../../astral-sh/ruff/pulls/16385.md) on 2025-02-26 16:19_

---
