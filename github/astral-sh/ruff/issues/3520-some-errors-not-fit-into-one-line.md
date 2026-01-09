---
number: 3520
title: Some errors not fit into one line
type: issue
state: closed
author: qarmin
labels:
  - question
assignees: []
created_at: 2023-03-14T18:32:17Z
updated_at: 2023-05-10T06:37:58Z
url: https://github.com/astral-sh/ruff/issues/3520
synced_at: 2026-01-07T13:12:14-06:00
---

# Some errors not fit into one line

---

_Issue opened by @qarmin on 2023-03-14 18:32_

ruff 0.0.255

When testing files, I see a lot of oneline error info
```
error: Failed to parse Desktop/33/SetPortInfoFF5.py: invalid syntax. Got unexpected token '__init__' at line 13 column 10
error: Failed to parse Desktop/33/PpmImagePlugin4.py: Got unexpected token 󤄳 at line 133 column 35
error: Failed to parse Desktop/33/DTDValidateError4.py: Got unexpected token 𓸾 at line 4 column 1
error: Failed to parse Desktop/33/_test_fortran3.py: unexpected EOF while parsing at line 38 column 1
error: Failed to parse Desktop/33/netr_DELTA_ID_UNION8.py: Got unexpected token 򍾖 at line 14 column 31
error: Failed to parse Desktop/33/winapi7.py: Got unexpected token 􋉐 at line 33 column 1
error: Failed to parse Desktop/33/rtf3.py: invalid syntax. Got unexpected token Newline at line 90 column 45
error: Failed to parse Desktop/33/NETLOGON_SAM_LOGON_RESPONSE6.py: Got unexpected token 񰅣 at line 57 column 65
error: Failed to parse Desktop/33/decoders7.py: Got unexpected nesting at line 2 column 1
error: Failed to parse Desktop/33/comparisons5.py: Got unexpected token 󯻁 at line 1 column 13
error: Failed to parse Desktop/33/abag1.py: unexpected EOF while parsing at line 17 column 1
error: Failed to parse Desktop/33/ed255194.py: invalid syntax. Got unexpected token '|' at line 52 column 2
error: Failed to parse Desktop/33/Content3.py: unexpected EOF while parsing at line 21 column 0
error: Failed to parse Desktop/33/vesti8.py: Got unexpected token 򻲈 at line 3 column 29
error: Failed to parse Desktop/33/libglib-2.0.so.0.7200.3-gdb0.py: Got unexpected token 󵟷 at line 1 column 1
```
but some files produce multiline errors, which is inconsistent and looks bad:
```
error: Failed to parse fix_except0.py: invalid syntax. Got unexpected token """
    try_stmt< 'try' ':' (simple_stmt | suite)
                  cleanup=(except_clause ':' (simple_stmt | suite))+
                  tail=(['except' ':' (simple_stmt | suite)]
                        ['else' ':' (simple_stmt | suite)]
                        ['finally' ':' (simple_stmt | suite)]) >
    """ at line 39 column 14
fix_except0.py:39:15: E999 SyntaxError: invalid syntax. Got unexpected token """
    try_stmt< 'try' ':' (simple_stmt | suite)
                  cleanup=(except_clause ':' (simple_stmt | suite))+
                  tail=(['except' ':' (simple_stmt | suite)]
                        ['else' ':' (simple_stmt | suite)]
                        ['finally' ':' (simple_stmt | suite)]) >
    """
Found 1 error.

```

Example file - [fix_except0.py.zip](https://github.com/charliermarsh/ruff/files/10972513/fix_except0.py.zip)


---

_Comment by @charliermarsh on 2023-03-14 18:37_

Not much we can do here as we don't control those errors (they come from LALRPOP, from RustPython). We could omit them entirely, but that seems even worse to me.

---

_Comment by @charliermarsh on 2023-03-14 18:37_

Maybe omitting them (just showing `SyntaxError`) is correct?

---

_Label `question` added by @charliermarsh on 2023-03-14 18:37_

---

_Comment by @MichaReiser on 2023-03-15 08:33_

We could potentially truncate after the first line. 

One thing that's on my mind is to re-work Diagnostics in general and, as part of that, I would have pushed towards printing diagnostics with more context by default (multiline diagnostics). Ideally, the system would support a "verbose" mode that prints extra context information that may be too verbose by default. 

Why I'm bringing this up. Has it been an intentional decision only to print one-line diagnostics when running the `check` command and if so, what was its motivation?

---

_Comment by @charliermarsh on 2023-03-19 03:37_

@MichaReiser - It's mostly born out of a "consistent-with-Flake8-by-default" attitude. We do support printing Clippy-like context via the `--show-source` flag (which can also be set in `pyproject.toml`):

![Screen Shot 2023-03-18 at 11 36 43 PM](https://user-images.githubusercontent.com/1309177/226152193-8a74de4c-3ccf-4397-bd10-8a1197daf19e.png)

But, clearly, it's not the default. That's not based on a principled stance other than that Flake8 also supports a `--show-source` flag and also doesn't default to it. We could definitely _consider_ making that the default.


---

_Comment by @jfmengels on 2023-03-22 20:43_

> We could definitely consider making that the default.

Please do :pray:

---

_Comment by @MichaReiser on 2023-03-22 22:36_

> > We could definitely consider making that the default.
> 
> 
> 
> Please do :pray:

Hy @jfmengels nice to see you here. I'm reading many of your blog posts to get inspiration for ruff's configuration and behaviour. Thanks for taking to write down your knowledge.

---

_Comment by @jfmengels on 2023-03-23 09:01_

> I'm reading many of your blog posts to get inspiration for ruff's configuration and behaviour. Thanks for taking to write down your knowledge.

Happy to help! Static analysis is a field that I care about a lot, and I want better tools for everyone. And it's not like there is any competition between linters, since we tend to target different languages 😄 
I'm also trying to learn things from Ruff (performance mostly), though since the implementation of `elm-review` is done in a very different language, I don't know how much I will be able to put to use in practice.

But let's not side-track the initial topic anymore 😅, always happy to discuss static analysis in other places. 

---

_Comment by @youknowone on 2023-04-20 14:55_

Is this came from `ParseErrorType::UnrecognizedToken`? The multiline output is totally unexpected. It must be a bug of RustPython.


---

_Referenced in [RustPython/RustPython#4902](../../RustPython/RustPython/issues/4902.md) on 2023-04-20 14:57_

---

_Comment by @DimitrisJim on 2023-04-25 04:10_

> but some files produce multiline errors, which is inconsistent and looks bad

but that specific file linked just happens to have a multi-line unexpected token (a triple quoted string literal in this case):

```python
    PATTERN  """
    try_stmt< 'try' ':' (simple_stmt | suite)
                  cleanup=(except_clause ':' (simple_stmt | suite))+
                  tail=(['except' ':' (simple_stmt | suite)]
                        ['else' ':' (simple_stmt | suite)]
                        ['finally' ':' (simple_stmt | suite)]) >
    """
```

hence why the output is multi-line. 

---

_Comment by @youknowone on 2023-04-25 05:37_

Ah, that was confusing due to contents of the string. Still it looks like the error message need to be escaped (and prorably shortened too?) when being formatted, is it?

---

_Comment by @DimitrisJim on 2023-04-26 10:20_

> Ah, that was confusing due to contents of the string

Exactly! That's why I wanted to run locally and check and realized its just part of the source code.

Not sure if we shorten/trim here, we can choose either really. I think it makes more sense to show the token that is invalid in its entirety considering that large-ish tokens like this one are not really the norm. Either way, if needed we'll discuss this further in the issue you opened. 

---

_Comment by @MichaReiser on 2023-04-26 16:47_

> Maybe omitting them (just showing `SyntaxError`) is correct?

This seems reasonable to me. #3931 adds a new `DisplayParseError` that implements our custom rendering for `SyntaxErrors. We can customize it to change the rendering of `UnrecognizedToken` to only show the message, or render the first line only (truncate with `...`). I can take a stab at it 

---

_Assigned to @MichaReiser by @MichaReiser on 2023-04-26 16:47_

---

_Comment by @youknowone on 2023-04-26 17:20_

@DimitrisJim That's reasonable. Probably escaping will be enough.

---

_Referenced in [astral-sh/ruff#4124](../../astral-sh/ruff/pulls/4124.md) on 2023-04-26 21:46_

---

_Closed by @MichaReiser on 2023-05-10 06:37_

---
