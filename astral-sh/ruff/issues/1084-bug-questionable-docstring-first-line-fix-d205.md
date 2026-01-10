---
number: 1084
title: "[bug?] Questionable docstring first line fix (`D205`)"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-05T22:51:49Z
updated_at: 2023-03-10T20:49:51Z
url: https://github.com/astral-sh/ruff/issues/1084
synced_at: 2026-01-10T01:22:38Z
---

# [bug?] Questionable docstring first line fix (`D205`)

---

_Issue opened by @smackesey on 2022-12-05 22:51_

I ran `ruff --fix` (with `D205` selected) to format our docstrings and it did this to docstrings with leading paragraphs:

```
### Before

def get_class_definitions(name: str, schema: dict) -> Dict[str, Dict[str, SchemaType]]:
    """
    Parses an Airbyte source or destination schema, turning it into a representation of the
    corresponding Python class structure - a dictionary mapping class names with the fields
    that the new classes should have.

### After

def get_class_definitions(name: str, schema: dict) -> Dict[str, Dict[str, SchemaType]]:
    """
    Parses an Airbyte source or destination schema, turning it into a representation of the

    corresponding Python class structure - a dictionary mapping class names with the fields
    that the new classes should have.
```

These leading paragraphs are indeed violations of Google style, where the summary is supposed to fit on one line. But I don't think the behavior of `--fix` here makes sense, as it assumes a multiline leading paragraph is really a one-line summary followed by a second paragraph, when in most cases it's probably just a paragraph. 


---

_Comment by @charliermarsh on 2022-12-05 23:54_

Yeah this has come up before, and I'm not happy with the current behavior. Not sure this should even be fixable because of false positives like this. Maybe if there's _more_ than one line between the summary and description? Or perhaps if the summary line appears on the first line? But that could still be a multi-line summary.

---

_Comment by @smackesey on 2022-12-05 23:57_

IMO it should not be fixable. The only thing that would make sense to me is if the first line ends with a period, but that would still produce the occasional false positive.

---

_Referenced in [astral-sh/ruff#1091](../../astral-sh/ruff/pulls/1091.md) on 2022-12-06 00:00_

---

_Comment by @charliermarsh on 2022-12-06 00:01_

Yeah, what _can_ be safely fixed is this:

```py
def get_class_definitions(name: str, schema: dict) -> Dict[str, Dict[str, SchemaType]]:
    """
    Here's the summary line followed by two empty lines (instead of one).


    Parses an Airbyte source or destination schema, turning it into a representation of the
    corresponding Python class structure - a dictionary mapping class names with the fields
    that the new classes should have.
```

---

_Closed by @charliermarsh on 2022-12-06 00:01_

---

_Referenced in [astral-sh/ruff#1672](../../astral-sh/ruff/issues/1672.md) on 2023-01-06 01:24_

---

_Comment by @smackesey on 2023-03-10 20:49_

To follow up on this-- we have many multiline docstring summaries, but would still like a variant of this rule to work. I realize that if the summary is on multiple lines, it can be hard to determine where it actually ends-- but have you considered detecting e.g. section starts, rst directives, or bulleted lists?

So this:

```
def foo():
"""Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque adipiscing faucibus
felis, ac ullamcorper nisl commodo ac. Etiam fermentum, urna nec viverra euismod, massa
 dui lobortis urna, ac porttitor elit nisl vel lacus. Suspendisse commodo sapien vel nibh aliquet semper. 
Args:
```

Becomes:

```
def foo():
"""Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque adipiscing faucibus
felis, ac ullamcorper nisl commodo ac. Etiam fermentum, urna nec viverra euismod, massa
 dui lobortis urna, ac porttitor elit nisl vel lacus. Suspendisse commodo sapien vel nibh aliquet semper. 

Args:
```

---
