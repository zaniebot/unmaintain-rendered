```yaml
number: 16930
title: Ensure Google-style docstring lines end in a correct character
type: issue
state: closed
author: gtkacz
labels:
  - rule
assignees: []
created_at: 2025-03-23T23:28:29Z
updated_at: 2025-03-27T20:10:26Z
url: https://github.com/astral-sh/ruff/issues/16930
synced_at: 2026-01-12T15:54:55Z
```

# Ensure Google-style docstring lines end in a correct character

---

_@gtkacz_

### Summary

From the official [Google Python style guide on docstrings](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings):
> A docstring should [...] [be] terminated by a period, question mark, or exclamation point.

Furthermore, in every example from the ["functions and methods" section](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods) all of the "Args", "Returns", and "Raises" line items end with a full stop (`.`).

I propose that two new rules be created:
1. `google-docstring-text-ends-in-correct-character`: Ensure that in every Google-style docstring, the summary and description portions of the docstring end in one of the proposed characters (`.`, `?`, `!`).
2. `google-docstring-line-items-end-in-fullstop`: Ensure that in every Google-style docstring, all of the "Args", "Returns", and "Raises" line items end with a full stop (`.`).

These should only be enforced if the `google` convention is active.

If these already exist, I'm sorry, I couldn't find them.

---

_Label `rule` added by @ntBre on 2025-03-24 14:23_

---

_Comment by @MichaReiser on 2025-03-27 20:10_

[D415](https://docs.astral.sh/ruff/rules/missing-terminal-punctuation/) already tests that the summary ends in a punctuation. Enforcing that the description ends in a punctuation is more challenging because it can contain arbitrary content. For example the following example from the google style guide doesn't end in a period.

```py
"""A one-line summary of the module or program, terminated by a period.

Leave one blank line.  The rest of this docstring should contain an
overall description of the module or program.  Optionally, it may also
contain a brief description of exported classes and functions and/or usage
examples.

Typical usage example:

foo = ClassFoo()
bar = foo.function_bar()
"""
```

The same problem exists for `Returns`, `Args` etc. sections. E.g, not every line in this example from the google style guide ends in a period.

```py
def fetch_smalltable_rows(
    table_handle: smalltable.Table,
    keys: Sequence[bytes | str],
    require_all_keys: bool = False,
) -> Mapping[bytes, tuple[str, ...]]:
    """Fetches rows from a Smalltable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by table_handle.  String keys will be UTF-8 encoded.

    Args:
        table_handle: An open smalltable.Table instance.
        keys: A sequence of strings representing the key of each table
          row to fetch.  String keys will be UTF-8 encoded.
        require_all_keys: If True only rows with values set for all keys will be
          returned.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {b'Serak': ('Rigel VII', 'Preparer'),
         b'Zim': ('Irk', 'Invader'),
         b'Lrrr': ('Omicron Persei 8', 'Emperor')}

        Returned keys are always bytes.  If a key from the keys argument is
        missing from the dictionary, then that row was not found in the
        table (and require_all_keys must have been False).

    Raises:
        IOError: An error occurred accessing the smalltable.
    """
```

There's also the chance that a section ends in an example, which doesn't necessarily get terminated with a period. 

I don't think we should add such rules because there's no clear heuristic that doesn't lead to too many false positives. 

---

_Closed by @MichaReiser on 2025-03-27 20:10_

---
