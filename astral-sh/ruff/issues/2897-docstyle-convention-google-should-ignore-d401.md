```yaml
number: 2897
title: "docstyle.convention = \"google\" should ignore D401"
type: issue
state: closed
author: PhilReinhold
labels:
  - bug
  - docstring
assignees: []
created_at: 2023-02-14T17:32:59Z
updated_at: 2023-02-14T20:42:22Z
url: https://github.com/astral-sh/ruff/issues/2897
synced_at: 2026-01-12T15:54:43Z
```

# docstyle.convention = "google" should ignore D401

---

_@PhilReinhold_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

This is tested on ruff 0.0.245

e.g. this [example from the google style guide](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods)

`test.py`:

```python
def fetch_smalltable_rows(table_handle: smalltable.Table,
                          keys: Sequence[Union[bytes, str]],
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

`pyproject.toml`

```toml
[tool.ruff]
select = ["D"]
pydocstyle.convention = "google"
```

then `ruff test.py` gives

```
test.py:8:5: D401 First line of docstring should be in imperative mood: "Fetches rows from a Smalltable."
Found 1 error.
```

I'll note that pydocstyle agrees D401 is ignored for google style: http://www.pydocstyle.org/en/stable/error_codes.html#default-conventions

Thanks for all of your hard work developing ruff! This tool has made a big improvement in my workflow.

---

_Comment by @charliermarsh on 2023-02-14 18:42_

Oh this might be an oversight... Will confirm. You're right that it should be ignored.

---

_Label `bug` added by @charliermarsh on 2023-02-14 20:38_

---

_Label `docstring` added by @charliermarsh on 2023-02-14 20:38_

---

_Comment by @charliermarsh on 2023-02-14 20:39_

Thanks, I think we added this rule _after_ we ported the conventions, so it didn't exist at the time!

---

_Closed by @charliermarsh on 2023-02-14 20:42_

---
