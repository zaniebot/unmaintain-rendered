---
number: 14555
title: UP031 may be undesirable in raw strings and regular expressions
type: issue
state: open
author: neutrinoceros
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-23T08:30:23Z
updated_at: 2024-11-26T14:53:36Z
url: https://github.com/astral-sh/ruff/issues/14555
synced_at: 2026-01-10T01:22:55Z
---

# UP031 may be undesirable in raw strings and regular expressions

---

_Issue opened by @neutrinoceros on 2024-11-23 08:30_

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
Here's a real life example taken from astropy
```python
# t.py
import re
KEYWORD_LENGTH = 8
re.compile(r"^[A-Z0-9_-]{0,%d}$" % KEYWORD_LENGTH)
```

This code isn't flagged for rule UP031 under ruff 0.7
```
❯ uvx --with 'ruff<0.8.0' ruff check t.py --select UP031
All checks passed!
```
but running the latest ruff 0.8.0:
```
❯ uvx --with 'ruff==0.8.0' ruff check t.py --select UP031
t.py:3:12: UP031 Use format specifiers instead of percent format
  |
1 | import re
2 | KEYWORD_LENGTH = 8
3 | re.compile(r"^[A-Z0-9_-]{0,%d}$" % KEYWORD_LENGTH)
  |            ^^^^^^^^^^^^^^^^^^^^^ UP031
  |
  = help: Replace with format specifiers

Found 1 error.
```

On the other hand, this is the form of that string that I need to write in order to satisfy the newly expended rule UP031
```python
rf"^[A-Z0-9_-]{{0,{KEYWORD_LENGTH}}}$"
```
note that I needed to escape raw `{` and `}` to `{{` and `}}` respectively. I don't think this makes the code more readable, and may actually be counterproductive. Admittedly this is a small example were it doesn't hurt too much but there could legitimately be much more involved cases in the wild.

Would it be possible to change this rule so that it ignores raw strings altogether ?

---

_Referenced in [astropy/astropy#17430](../../astropy/astropy/issues/17430.md) on 2024-11-23 08:32_

---

_Label `rule` added by @MichaReiser on 2024-11-25 08:45_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-25 08:45_

---

_Comment by @MichaReiser on 2024-11-25 08:53_

Thanks for bringing this up. This is an interesting case. 

I agree that the need for escaping the `{` adds some cognitive overhead, but I personally don't think it's too bad and I prefer trading the `%` and `%d` with two extra escapes.

> Would it be possible to change this rule so that it ignores raw strings altogether ?

To me it's not clear how escaping `{` with `{{` is much different from having to write `\{`. Both cases require escaping.  That's why this is more about whether UP031 should be raised on strings containing any curly braces that require escaping. I would find it surprising if I opted into UP031, and some strings wouldn't be flagged (for not very apparent reasons of why). 

I'm interested in hearing more opinions.



---

_Comment by @eerovaher on 2024-11-25 14:55_

I'm not convinced the f-string is better than printf-style formatting in this particular case, but there is also the option of using [template strings](https://docs.python.org/3/library/string.html#template-strings). The example snippet could be rewritten as
```python
import re
from string import Template
KEYWORD_LENGTH = 8
re.compile(Template("^[A-Z0-9_-]{0,$N}$$").substitute(N=KEYWORD_LENGTH))
```
That being said, template strings are more convenient if the string contains many literal `{}` pairs and the example string contains only one, but they are a valid way of replacing printf-style formatting, which strengthens the case for UP031 not making any exceptions.

---

_Comment by @eli-schwartz on 2024-11-25 23:33_

> To me it's not clear how escaping `{` with `{{` is much different from having to write `\{`. Both cases require escaping.

No, the latter case doesn't require escaping at all, per the original description where `r''` strings are successfully used for precisely that reason.

I'd argue that users of raw strings are making a pretty clear statement that they would prefer to avoid having to use escapes. Although perhaps I'd also argue that f-strings should only be used when they subjectively look prettier than the alternative (and that all use cases for PEP 701 look bad) and that therefore UP031 is too unreliable due to the inability to control the heuristics used and should simply be disabled in project configs. So maybe it doesn't matter.

---

_Comment by @MichaReiser on 2024-11-26 06:42_

> No, the latter case doesn't require escaping at all, per the original description where r'' strings are successfully used for precisely that reason.

My point here was mainly that I don't see a difference between converting a normal string containing curly braces to an f-string to converting a raw string containing curly braces to an f-string. 

```py
"a { len: %d }" % LENGTH
r"a { len: %d }" % LENGTH
```

For both cases, it's necessary to escape `{` when rewriting the format string to an f-string.

```py
"a { len: {{LENGTH}} }"
r"a { len: {{LENGTH}} }"
```

> I'd argue that users of raw strings are making a pretty clear statement that they would prefer to avoid having to use escapes. Although perhaps I'd also argue that f-strings should only be used when they subjectively look prettier 

Yeah, that's a hard metric to get right for all users, but I'm all ears if someone has suggestions. I'd suggest that users feel free to use noqa comments if they feel that it arguably looks worse in the few one-off cases where this happens. 



---

_Referenced in [astropy/astropy#17452](../../astropy/astropy/pulls/17452.md) on 2024-11-26 09:22_

---

_Comment by @bashonly on 2024-11-26 14:53_

> My point here was mainly that I don't see a difference between converting a normal string containing curly braces to an f-string to converting a raw string containing curly braces to an f-string.

IME, complex regex patterns with literal curly braces are where UP031 can cause the most trouble, e.g. this code snippet...

```py
        fields_m = re.finditer(
            r'''(?x)
                (?P<key>%s)\s*:\s*function\s*\((?P<args>(?:%s|,)*)\){(?P<code>[^}]+)}
            ''' % (_FUNC_NAME_RE, _NAME_RE),
            fields)
```

...would become...
```py
        fields_m = re.finditer(
            rf'''(?x)
                (?P<key>{_FUNC_NAME_RE})\s*:\s*function\s*\((?P<args>(?:{_NAME_RE}|,)*)\){{(?P<code>[^}}]+)}}
            ''',
            fields)
```
...making it significantly harder to read and maintain (IMO)

---
