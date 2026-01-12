```yaml
number: 2099
title: "[`pyupgrade`]: Remove outdated `sys.version_info` blocks"
type: pull_request
state: merged
author: colin99d
labels:
  - rule
assignees: []
merged: true
base: main
head: OldCodeBlocks
created_at: 2023-01-23T03:26:07Z
updated_at: 2023-02-02T14:27:05Z
url: https://github.com/astral-sh/ruff/pull/2099
synced_at: 2026-01-12T04:52:00Z
```

# [`pyupgrade`]: Remove outdated `sys.version_info` blocks

---

_Pull request opened by @colin99d on 2023-01-23 03:26_

A part of #827. Opening this up as a draft for visibility.

---

_Comment by @colin99d on 2023-01-25 03:13_

@charliermarsh I am hitting a blocker and was wondering if you could offer some assistance. My issue is linting the code below:
```
if sys.version_info < (2,0):
    print("This script requires Python 2.0 or greater.")
elif sys.version_info < (2,6):
    print("This script requires Python 2.6 or greater.")
elif sys.version_info < (3,):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,0):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,12):
    print("Python 3.0 or 3.1 or 3.2")
else:
    print("Python 3")
```
Currently, my program lints every line, and removes it if the condition meets the requirements, this means we end up with the following:
```
elif sys.version_info < (3,12):
    print("Python 3.0 or 3.1 or 3.2")
else:
    print("Python 3")
```
As you can see, this is not a valid python statement. I tried creating checkers that either look at the parent, or see if the statement is an if to remove the "el" from the "elif" in the next line, however; since ruff goes through all elif and if statements before running again, the code is already broken before the next loop through. So essentially I need to know whether to convert an elif statement to an if statement.

Pyupgrade currently solves this issue by only removing one if or elif at a time, so to fix the statement above you would have to call pyupgrade four times. 

Only running on if statements is not a viable solution here because sometimes the issue might only be on an elif statement. So far my best ideas would be to:
1. Somehow "skip" the rest of the statement until the next run through,
2. Save the state somehow, so it can know whether to skip (this seems like a pretty bad idea)
3. Allow the statement to be converted to the bad format, and then run a separate linter at the end that can fix it. This seems like a REALLY bad idea because if the linter breaks anywhere else, we will wreck people's code

---

_Comment by @colin99d on 2023-01-27 02:58_

It looks like I have a second issue that could be easily fixed if I can skip fixing the rest of the statement until the next iteration. Is there any functionality like that?

---

_Comment by @charliermarsh on 2023-01-27 03:10_

We avoid applying fixes that "overlap" in the text. So one thing you could do is... when you go to fix one of these, generate a fix that applies to the entire `if` chain, even if you're only changing one of the statements within it.

So if you had this:

```py
if sys.version_info < (2,0):
    print("This script requires Python 2.0 or greater.")
elif sys.version_info < (2,6):
    print("This script requires Python 2.6 or greater.")
elif sys.version_info < (3,):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,0):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,12):
    print("Python 3.0 or 3.1 or 3.2")
else:
    print("Python 3")
```

Then to fix the first one, you'd generate a `Fix::replace` with this text:

```py
if sys.version_info < (2,6):
    print("This script requires Python 2.6 or greater.")
elif sys.version_info < (3,):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,0):
    print("This script requires Python 3.0 or greater.")
elif sys.version_info < (3,12):
    print("Python 3.0 or 3.1 or 3.2")
else:
    print("Python 3")
```

All the fixes will overlap, and so it'll only apply one per iteration.

---

_Comment by @colin99d on 2023-01-27 03:11_

> We avoid applying fixes that "overlap" in the text. So one thing you could do is... when you go to fix one of these, generate a fix that applies to the entire `if` chain, even if you're only changing one of the statements within it.
> 
> So if you had this:
> 
> ```python
> if sys.version_info < (2,0):
>     print("This script requires Python 2.0 or greater.")
> elif sys.version_info < (2,6):
>     print("This script requires Python 2.6 or greater.")
> elif sys.version_info < (3,):
>     print("This script requires Python 3.0 or greater.")
> elif sys.version_info < (3,0):
>     print("This script requires Python 3.0 or greater.")
> elif sys.version_info < (3,12):
>     print("Python 3.0 or 3.1 or 3.2")
> else:
>     print("Python 3")
> ```
> 
> Then to fix the first one, you'd generate a `Fix::replace` with this text:
> 
> ```python
> if sys.version_info < (2,6):
>     print("This script requires Python 2.6 or greater.")
> elif sys.version_info < (3,):
>     print("This script requires Python 3.0 or greater.")
> elif sys.version_info < (3,0):
>     print("This script requires Python 3.0 or greater.")
> elif sys.version_info < (3,12):
>     print("Python 3.0 or 3.1 or 3.2")
> else:
>     print("Python 3")
> ```
> 
> All the fixes will overlap, and so it'll only apply one per iteration.

Charlie this is absolutely perfect! Thank you so much!!!

---

_Comment by @charliermarsh on 2023-01-27 03:14_

No worries, sorry it took me a while to respond. It might take some work to find the outermost `if` parent, but hopefully it's at least _possible_.

---

_Comment by @colin99d on 2023-01-28 03:35_

@charliermarsh, I believe I found an issue with the `rustpython_parser::lexer`. When I run the following program in python, it runs fine:
```
import six

if True:
    if six.PY2:
        print("PY2")
    else:
        print("PY3")

```
However, when I attempt to use the lexer to get the tokens in the statement I get the following issue:
`value: LexicalError { error: IndentationError, location: Location { row: 3, column: 4 } }`. When printing the tokens out I get the following before the error occurs:
```
If
Name { name: "six" }
Dot
Name { name: "PY2" }
Colon
Newline
Indent
Name { name: "print" }
Lpar
String { value: "PY2", kind: String, triple_quoted: false }
Rpar
Newline
```
Based on this, and the position of the location where the error occurs, it looks like the parser is failing one the `else` token. Do you have any idea what could be going on here?

---

_Comment by @charliermarsh on 2023-01-28 03:59_

I think you need to dedent the entire block before lexing.

Right now, you're feeding it:

```py
if six.PY2:
        print("PY2")
    else:
        print("PY3")
```

...since you're starting at the `if`. So it looks like invalid indentation.

(This is a guess, I admittedly didn't look at the code.)

---

_Comment by @colin99d on 2023-01-28 18:27_

> I think you need to dedent the entire block before lexing.
> 
> Right now, you're feeding it:
> 
> ```python
> if six.PY2:
>         print("PY2")
>     else:
>         print("PY3")
> ```
> 
> ...since you're starting at the `if`. So it looks like invalid indentation.
> 
> (This is a guess, I admittedly didn't look at the code.)

I think you are exactly right, is there an example somewhere in the code of this being done already? I noticed dedent does not work if the first line has no indent.

---

_Comment by @charliermarsh on 2023-01-28 18:50_

You could look at how we handle `fix_multiple_with_statements`. It uses LibCST, but you could use the same idea.

---

_Marked ready for review by @colin99d on 2023-01-29 23:06_

---

_Comment by @colin99d on 2023-01-29 23:12_

This is ready to go! One thing I could use some feedback on is that I am not removing comments from the removed block if they are the last line of the else block. I am not sure if there is a way to resolve this, but if there is let me know! I do not see this as an issue that blocks the PR. Windows tests are failing for this pr too, even though I just merged with main.

---

_Comment by @neutrinoceros on 2023-01-29 23:21_

> One thing I could use some feedback on is that I am not removing comments from the removed block if they are the last line.

As the author of the original feature in pyupgrade, I think this is the behaviour to be expected.

pyupgrade doesn't attempt to clean up any code that might be left hanging in this situation, to avoid generating syntax errors.

For instance:
```python
if sys.version_info >= (3, 9):
    pass
else:
    ... # special treatment
```
is transformed to (with `--py39-plus`)
```python
pass
```

Comments are less of a problem, but I think it's still preferable to leave them intact, as it's much easier to manually clean them up later if necessary than it might be to just realise they are gone if it wasn't.


---

_Comment by @colin99d on 2023-01-29 23:29_

Thanks for your feedback and sounds good!

---

_Comment by @colin99d on 2023-01-30 00:23_

@charliermarsh, would you also like me to remove the Six functionality from this PR?

---

_Comment by @charliermarsh on 2023-01-30 00:24_

@colin99d - I think it'd be appropriate. What about you?

---

_Comment by @colin99d on 2023-01-30 02:49_

Got the six stuff removed!

---

_Comment by @neutrinoceros on 2023-01-30 20:32_

I got a question regarding how this is meant to be used in practice: is there any equivalent to pyupgrade's `--py3x-plus` flags in ruff, or does ruff parse the `[project] python-requires` field from `pyproject.toml` ? is it doing something else ? I haven't been able to find this in docs.

---

_Comment by @charliermarsh on 2023-01-30 20:36_

@neutrinoceros - The relevant API is [`target-version`](https://github.com/charliermarsh/ruff#target-version), which you set in `pyproject.toml` (or `ruff.toml`) -- but it's not accepted on the command-line. It's similar to Black's API.

---

_Comment by @neutrinoceros on 2023-01-30 20:49_

thank you !
In case you're not aware, there's a PR in black to use the project metadata directly when possible, which I think would be useful  in ruff too

see https://github.com/psf/black/pull/3219

---

_Comment by @charliermarsh on 2023-01-30 21:06_

Oh nice! I need to look at how Black does that. This was mentioned in #2039.

---

_Comment by @charliermarsh on 2023-02-01 00:06_

I'm hoping to review this tonight.

---

_Renamed from "Pyupgrade: Old code blocks" to "[`pyupgrade`]: Remove outdated `sys.version_info` blocks" by @charliermarsh on 2023-02-01 02:22_

---

_Label `rule` added by @charliermarsh on 2023-02-01 02:56_

---

_@charliermarsh reviewed on 2023-02-01 04:47_

---

_Review comment by @charliermarsh on `src/rules/pyupgrade/mod.rs`:67 on 2023-02-01 04:47_

@sbrugman - Do you have any idea why these are failing on Windows? I tried with both `to_string_lossy()` and `display()` but no luck.

---

_Comment by @charliermarsh on 2023-02-01 04:48_

@colin99d - Did a pass over this, I was able to simplify a few things a little bit, but I also made it slightly more timid in some cases... Do you have any interest in looking through or even testing the code prior to merging?


---

_Comment by @colin99d on 2023-02-01 14:18_

> @colin99d - Did a pass over this, I was able to simplify a few things a little bit, but I also made it slightly more timid in some cases... Do you have any interest in looking through or even testing the code prior to merging?

Would love to! I can do this tonight.

---

_Comment by @colin99d on 2023-02-01 14:57_

What is the reason for not refactoring single line if statements anymore? I am assuming there were some edge cases it was not handling well.

---

_Comment by @charliermarsh on 2023-02-01 15:03_

I thought it would just be simpler to not worry about them given that they're pretty rare (e.g. Black doesn't preserve them). It's possible that the implementation was totally sound... I just get worried about cases like multi-statement lines and continuations:

```py
if True: a = 1; b = 2

if True: a = 1; \
  b = 2
```

But this isn't a rigorous answer at all. Maybe we should revisit.

---

_Comment by @colin99d on 2023-02-01 15:05_

> I thought it would just be simpler to not worry about them given that they're pretty rare (e.g. Black doesn't preserve them). It's possible that the implementation was totally sound... I just get worried about cases like multi-statement lines and continuations:
> 
> ```python
> if True: a = 1; b = 2
> 
> if True: a = 1; \
>   b = 2
> ```
> 
> But this isn't a rigorous answer at all. Maybe we should revisit.

I have not seen this in practice before EVER, so maybe we ignore it, and if someone leaves an issue, and it gains some traction, we can invest into it.

---

_Comment by @charliermarsh on 2023-02-01 15:08_

There is one failing case right now, which I think we can only solve with LibCST -- something like this:

```py
if True:
    if sys.version_info >= (3, 9):
        expected_error = [
"<stdin>:1:5: Generator expression must be parenthesized",
"max(1 for i in range(10), key=lambda x: x+1)",
"    ^",
        ]
    elif PYPY:
        expected_error = []
    else:
        expected_error = []
```

Or even harder:

```py
if True:
    if sys.version_info >= (3, 9):
        """this
        is valid"""

        """the indentation on
        this line is significant"""

        "this is" \
"allowed too"

        ("so is"
"this for some reason")
```

`dedent` isn't safe for these reasons.

---

_Comment by @charliermarsh on 2023-02-01 17:04_

(Fixed.)

---

_Comment by @charliermarsh on 2023-02-01 17:50_

Ok, I think this is good to go.

---

_Comment by @charliermarsh on 2023-02-02 00:20_

@colin99d - LMK if you wanna give this a read tonight or if I should go ahead and merge. It's not urgent -- I probably won't release tonight anyway.

---

_Merged by @charliermarsh on 2023-02-02 12:49_

---

_Closed by @charliermarsh on 2023-02-02 12:49_

---

_Comment by @charliermarsh on 2023-02-02 12:49_

Merging for now but LMK if you have any follow-up feedback.

---

_Comment by @colin99d on 2023-02-02 14:03_

Sorry, had something come up last night. Looks good to me!!!

---

_Comment by @charliermarsh on 2023-02-02 14:27_

All good, no prob at all!

---
