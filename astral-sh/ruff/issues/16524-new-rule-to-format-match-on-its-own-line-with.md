```yaml
number: 16524
title: "New rule to format `match` on its own line with `pytest.warns` or `pytest.raises`"
type: issue
state: closed
author: user27182
labels:
  - rule
assignees: []
created_at: 2025-03-05T17:49:11Z
updated_at: 2025-03-05T17:55:32Z
url: https://github.com/astral-sh/ruff/issues/16524
synced_at: 2026-01-10T11:09:57Z
```

# New rule to format `match` on its own line with `pytest.warns` or `pytest.raises`

---

_Issue opened by @user27182 on 2025-03-05 17:49_

### Summary

New rule idea, maybe for pytest style [`PT`](https://docs.astral.sh/ruff/rules/#flake8-pytest-style-pt) ?

If I use `pytest.raises` or `pytest.warns` with a long string for `match`, the test may be formatted as:
``` python
with (
    pytest.raises(
        AttributeError,
        match=re.escape(
            'foo bar baz qux quux corge grault `garply` waldo fred plugh xyzzy thud spam eggs'
        ),
    ),
):
```

This has a lot of nested brackets which makes it hard to read. It would be better if `ruff` could auto-format this by extracting `match` as a new variable on its own line.

``` python
match = 'foo bar baz qux quux corge grault `garply` waldo fred plugh xyzzy thud spam eggs'
with pytest.raises(AttributeError, match=re.escape(match)):
```

---

_Comment by @MichaReiser on 2025-03-05 17:55_

Thanks for the suggestion. 

I can see how this is desirable but it is a bit too opinionated to fit into Ruff's current rule set. It might also be challenging to decide when this is desired. E.g. what if the `raises` breaks for other reasons other than `match`? 



---

_Closed by @MichaReiser on 2025-03-05 17:55_

---

_Label `rule` added by @MichaReiser on 2025-03-05 17:55_

---
