```yaml
number: 11048
title: raw pathlib.Path truthyness checking
type: issue
state: open
author: hairmare
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-04-19T20:26:18Z
updated_at: 2024-10-29T15:18:51Z
url: https://github.com/astral-sh/ruff/issues/11048
synced_at: 2026-01-12T15:54:50Z
```

# raw pathlib.Path truthyness checking

---

_@hairmare_

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

As a strong proponent of `ALL` my colleague and I have recently stumbled over [PTH](https://docs.astral.sh/ruff/rules/#flake8-use-pathlib-pth) (flake8-use-pathlib)

Specifically in an effort to replace `os.path` we ended up doing this by accident

```python
from pathlib import Path
if Path("does-not-exists"):
    ...
````

We forgot to `Path(...).exists()`, it should have been this:

```python
from pathlib import Path
if Path("does-not-exists").exists():
    ...
````

Given this happened in the context of an `if` clause.  Is ensuring that noone tries to check a bare `Path` for truthiness a check that could/should be introduced?

---

_Label `rule` added by @charliermarsh on 2024-04-19 21:51_

---

_Comment by @charliermarsh on 2024-04-19 21:51_

That seems like a reasonable rule to me, though not sure how best to categorize it.

---

_Comment by @MichaReiser on 2024-04-21 11:25_

I'm not sure if this rule is too specific. What about other types that are used to check for truthiness?

 
Should we make the rule more generic so that it warns about all types that don't implement `__bool__` and aren't well known types (e.g. list, primitives etc) and are used in a thruthiness check? 

---

_Comment by @trim21 on 2024-04-22 08:52_

> I'm not sure if this rule is too specific. What about other types that are used to check for truthiness?
> 
> Should we make the rule more generic so that it warns about all types that don't implement `__bool__` and aren't well known types (e.g. list, primitives etc) and are used in a thruthiness check?

I have another case for this...

```python
import xml.etree.ElementTree

et: xml.etree.ElementTree.Element = xml.etree.ElementTree.fromstring(
    """
<rss version="2.0">
<channel>
<title>title</title>
<link>
<![CDATA[ https://example.com ]]>
</link>
<pubDate>Mon, 22 Apr 2024 16:50:40 +0800</pubDate>
<item>
<title>
<![CDATA[ a title ]]>
</title>
<link>https://exmaple.com/not-expected-url</link>
<enclosure url="https://example.com/expected-url" length="16306828171" type="application/x-bittorrent"/>
<guid isPermaLink="false">guid</guid>
</item>
</channel>
</rss>
"""
)

raw = []

for item in et.findall("channel/item"):
    enclosure = item.find("./enclosure")
    # gotcha !
    # need to use `enclosure is not None`
    if enclosure:
        url = enclosure.attrib.get("url")
    else:
        # torrentleech
        url = item.findtext("./link")

    print(url)
```


---

_Comment by @hairmare on 2024-04-23 19:48_

A general `__bool__`  check does sound like an interesting approach worth further exploring and would help uncovering the error we initially encountered.

The etree example wouldn't be covered by a rule generalized in this way given the `Element` returned from a `find` lookup does implement `__bool__`. In most XML use cases, i usually recommend ensuring the data can be trusted by doing previous validation using XML tooling like XML Schema Definitions.


---

_Comment by @sbrugman on 2024-10-29 14:25_

I could not find any real-world examples of the initial example (that does not require type information). I'd suggest adding the type-inference label to the issue for the generic case.

---

_Label `type-inference` added by @MichaReiser on 2024-10-29 15:18_

---
