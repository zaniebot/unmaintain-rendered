```yaml
number: 13284
title: "BUG: Rule `D413` does not catch missing single blank line or multiple blank lines at end of docstring"
type: issue
state: open
author: user27182
labels:
  - breaking
  - docstring
  - needs-decision
assignees: []
created_at: 2024-09-08T17:01:14Z
updated_at: 2025-10-07T15:10:57Z
url: https://github.com/astral-sh/ruff/issues/13284
synced_at: 2026-01-10T11:09:55Z
```

# BUG: Rule `D413` does not catch missing single blank line or multiple blank lines at end of docstring

---

_Issue opened by @user27182 on 2024-09-08 17:01_

There is a bug with rule [`D413`](https://docs.astral.sh/ruff/rules/missing-blank-line-after-last-section/). With this rule, a single blank line is expected at the end of multiline docstrings.

But, this does not work in a few cases. E.g.:

Calling `ruff check foo.py --select D413 --fix` on this code does not insert a blank line at the end:
``` python
def foo():
    """Bar.

    Multiline docstring.
    """
```

In another case, we see that this rule works for some section names such as `Returns`, e.g. here a new line is correctly inserted at the end:
```python
def foo():
    """Bar.

    Returns
    -------
    Nothing.
    """
```

But, it does not work for any general section heading. E.g. no blank line is inserted here after the `Section` section (but one is expected):
``` python
def foo():
    """Bar.

    Section
    -------
    Nothing.
    """
```

For the first case, I suppose there may be an argument that technically the docstring has no sections, and therefore the rule isn't being violated. And for the last case there may be an argument that only standard section names are supported (e.g. `Parameters`, `Returns`, `See Also`, etc.). But, in my view, I think that with this rule enabled, there should _always_ be a blank line expected (and inserted with `--fix`) at the end of the docstring. 

I have not tested this against the `pydocstyle` behavior though, not sure what the expected result is from that. Maybe a new rule could be added instead in case the current behavior is considered to be "correct"?

Also, related to https://github.com/astral-sh/ruff/issues/9451, with `D413` we should expect only a _single_ line at the end of a docstring. But this code passes (the two blank lines should be replaced with a single blank line):
``` python
def foo():
    """Bar.

    Multiline docstring.


    """
```

Same thing with this code, only one line is expected:
``` python
def foo():
    """Bar.

    Multiline docstring.

    Returns
    -------
    Nothing.

    
    """
```


---

_Label `docstring` added by @AlexWaygood on 2024-09-09 16:46_

---

_Comment by @MichaReiser on 2024-09-10 18:12_

> For the first case, I suppose there may be an argument that technically the docstring has no sections, and therefore the rule isn't being violated. And for the last case there may be an argument that only standard section names are supported (e.g. Parameters, Returns, See Also, etc.). But, in my view, I think that with this rule enabled, there should always be a blank line expected (and inserted with --fix) at the end of the docstring.

I think extending the rule to work after any, even unsupported section does make sense. 

I don't think it's the right call to extend the rule to also flag docstrings without a section. It would change the rule's intention in a very significant way:

> Checks for missing blank lines after the last section of a multiline docstring.

---

_Label `needs-decision` added by @MichaReiser on 2024-09-10 18:12_

---

_Label `breaking` added by @MichaReiser on 2024-09-10 18:14_

---

_Comment by @MichaReiser on 2024-09-10 18:15_

It's also worth noting that custom section names aren't recognized by other rules as well. For example, D214 doesn't report a section named `Section` but is raised for `Parameters` ([playground](https://play.ruff.rs/2dc8fa12-1065-465a-82a8-7e409586a92a))

---

_Comment by @user27182 on 2024-09-10 20:15_

> I don't think it's the right call to extend the rule to also flag docstrings without a section. It would change the rule's intention in a very significant way:
> 
> > Checks for missing blank lines after the last section of a multiline docstring.

Fair enough. I guess this comes down to semantics/definitions. Is a section only considered to be a section if it has a heading (e.g.  `Parameters`)? Or is the initial summary also considered a section, but without a heading?

Aside from technical definitions of what a section is, always having a single blank line makes the docstrings visually consistent, regardless of the presence of a heading or not. (In some cases, this is also useful to avoid RST syntax errors where a blank line may be required, though preventing syntax errors is likely out of scope for this rule). 

So if it's decided that a multiline docstring without any sections is excluded from `D413`, then I propose having a separate rule that would cover this case. The rule I am looking for is perhaps not:

> Checks for missing blank lines after the last section of a multiline docstring.

but instead

> Checks that multiline docstrings have a single blank line at the end.

And by a single blank line at the end, I mean something like this, where there a line of pure whitespace at the end:

``` python

def foo():
    """Bar.

    Multiline docstring.

    """
```

I mention this because technically, according to [PEP 257](https://peps.python.org/pep-0257/#handling-docstring-indentation) the third line here with the triple quotes is considered a blank line:

``` python
def foo():
    """
    This is the second line of the docstring.
    """
```

And so based on the PEP 257 definition, `ruff` isn't applying `D413` correctly, since the following example _already has_ a blank line at the end and therefore isn't missing one (but `ruff` disagrees, and will add a second line here).

``` python
def foo():
    """Bar.

    Returns
    -------
    Nothing.
    """
```

So perhaps "section" and "blank line" need to be more clearly defined and documented to ensure users better understand what to expect from this rule.

---

_Comment by @jhrr on 2024-09-17 08:59_

I'm also having this problem. The main issue for me is just to be able to ensure that there's an actual newline between the final closing `"""` no matter what (if it's a multiline docstring), just to enforce complete visual consistency everywhere. 

> So if it's decided that a multiline docstring without any sections is excluded from D413, then I propose having a separate rule that would cover this case.

As @user27182 says, all semantics aside, this would be my preferred solution also in order to satisfy that demand if this rule is not suitable for doing that job itself.

Thanks for all the effort on the library.

---
