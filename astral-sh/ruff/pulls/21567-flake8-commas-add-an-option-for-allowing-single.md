```yaml
number: 21567
title: "[flake8-commas] Add an option for allowing single-argument functions (COM812)"
type: pull_request
state: closed
author: davidt
labels:
  - configuration
  - needs-decision
assignees: []
base: main
head: push-nzpmoolkmvxw
created_at: 2025-11-21T18:14:42Z
updated_at: 2025-11-24T17:23:37Z
url: https://github.com/astral-sh/ruff/pull/21567
synced_at: 2026-01-12T15:57:28Z
```

# [flake8-commas] Add an option for allowing single-argument functions (COM812)

---

_@davidt_

## Summary

Resolves #18258
See also #13410

The following patterns currently are incompatible with enabling the COM812 rule:

```python
some_string = gettext(
    'This is a long string which '
    'is marked for internationalization'
)

some_function(
    a_long +
    expression
)
```

The fundamental intent for COM812 is to minimize diffs when adding additional entries to a list, dict, or function call. In the case of the above, without an option, users are limited to one of these options:

```python
# Include a trailing comma. This is ugly, and if for some reason another
# sentence was added, the comma has to be removed.
s = gettext(
    'This is a long string '
    'which is marked for i18n.',
)

# Put the closing `)` on the same line. Adding another argument to this
# requires changing the `)` to a `,`, so there's no minimization of the
# diff here either.
some_function(
    a_long +
    expression)


# Assigning an expression to a variable and then calling the function
# works in some cases, but it bloats the code, and explicitly cannot
# work in the case of `gettext` where the scanner that extracts strings
# for the .po files doesn't know much about Python, and can only extract
# string literals inside of the `gettext(...)` calls.
var = (
    a_long +
    expression
)
some_function(var)

broken = (
    'This string will not be properly '
    'extracted by gettext scanners.'
)
s = gettext(broken)
```

In order to enable use of COM812 in codebases with this pattern, this change adds a new configuration key:
```toml
[lint.flake8-commas]
allow_single_arg_function_calls = true
```

Setting this allow the COM812 rule to flag everything it normally does, but with an exception for a function which is called with a single argument.


## Test Plan

A new test case has been added which uses the existing COM81* test fixture, and the resulting snapshot has been carefully reviewed to verify correctness. One additional case has been added to that file to verify that single-element dictionaries are not affected.

---

_Label `needs-decision` added by @ntBre on 2025-11-21 18:24_

---

_Label `configuration` added by @ntBre on 2025-11-21 18:26_

---

_Comment by @amyreese on 2025-11-22 01:30_

Are there other examples where the workaround (assigning to a variable before calling) isn't viable beyond `gettext()`? Would it be better to just have a single exception to the rule rather than adding a whole new configuration option just to allow using this rule in projects that also use gettext?

---

_Comment by @davidt on 2025-11-24 16:40_

I don't know for certain, but even in the gettext case, there are multiple potential names that get scanned for (at least `gettext()` and `_()`, historically django also provided a `ugettext()`). It's not exactly a one-and-done solution.

That said, I feel like forcing people to choose between defining a variable and adding a comma is way too strong for a linter rule where the goal of the rule is to make diffs easier to review. In my experience, a single-argument function is likely to always remain a single-argument function, and optimizing for readability is much more important than optimizing for diffs.

---

_Comment by @MichaReiser on 2025-11-24 17:23_

Thanks for looking into this.

I feel like this option defeats the purpose of the rule. The purpose is to reduce the diff size and, by enabling this rule, you opt-in that this is the top priority, including single-argument function calls.

I don't currently see enough demand for this configuration option and, therefore, close this PR. I'll leave the issue open so that other users can express their interest.

---

_Closed by @MichaReiser on 2025-11-24 17:23_

---
