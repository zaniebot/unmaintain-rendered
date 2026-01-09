---
number: 8687
title: "Proposal: `uv doc`"
type: issue
state: open
author: nanthony007
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-10-29T21:18:58Z
updated_at: 2025-03-05T04:19:57Z
url: https://github.com/astral-sh/uv/issues/8687
synced_at: 2026-01-07T13:12:18-06:00
---

# Proposal: `uv doc`

---

_Issue opened by @nanthony007 on 2024-10-29 21:18_

# `uv doc` as an API documentation tool

Is there any consideration into extending the functionality of `uv` into the documentation generation landscape? The idea would be mimicking `cargo doc` (`rustdoc`) to generate _static_ documentation for available offline. Accessing a project's user-defined functions along with their signatures and corresponding doc-strings seems, on the surface, to be trivial although I am sure I am failing to consider some edge cases. It then becomes a matter of parsing the doc-string as Markdown into its various components (assuming either Google or Numpy style) and rendering the corresponding HTML components in an interactive and connected way (i.e. including cross-references, navigation, links, code snippets for examples, search, etc). The idea here would be to mimic `cargo doc` functionality including the ability to also document (or not) private objects (assuming they were defined using a leading underscore?) and potentially to also enable the testing/executing of examples to ensure they stay up-to-date although admittedly I am unsure if this would even be possible in Python. As you can see there is a lot of room for scope creep here, but below I define some opportunities and some boundaries that I see. Overall I think this would be a great addition to the ecosystem.

Currently there is work that has already been completed in this space, notably [Sphinx](https://www.sphinx-doc.org/en/master/), [Mkdocs](https://www.mkdocs.org) -- specifically [mkdocstrings](https://mkdocstrings.github.io), and [pdoc](https://pdoc.dev) so there is a lot of existing work to build upon regarding best practices for accessing the required Python data and parsing the various doc-string formats. 

So why would `uv` get involved? 

1. Some of these tools use small web servers that (`mkdocs serve`, admittedly, could run locally, but make sharing more difficult. Static documentation exports could be shared or published internally especially for non-public facing code.
2. `uv` could improve rendering times of the documentation significantly.
3. `uv` could improve the Python landscape by making API documentation more accessible likely (although this is speculation) leading to more adoption, especially if adoption is as simple as "run this command to get API docs".
4. `uv` could develop a new, or embrace an existing doc-string style ([PEP #257](https://peps.python.org/pep-0257/)) for users. This might also assist with cross-reference links specifically. Astral is in a position here to provide guidance similar to the "How to write documentation" section of the Rust book.
6. Astral can put their own branding and styling on the documentation. Here I mean not only themes and CSS-driven styling (which would likely need to be extensible to please users), but also Copyright information.

What are some scope limitations here?

1. This would be for generating **API documentation** and prose-based documentation would still best be suited to static site generators like MkDocs.
2. This would likely work best with well-typed Python libraries and would apply less to applications and even less to un-typed applications.
3. A natural question that occurred to me is: "Would Astral then have hosting for these API docs similar to docs.rs? That would allow online access of published PyPI packages as well as versioning of the documentation itself." and I think the answer here is no and that this is out of scope since it would likely require integrating closely with PyPI for when packages are published/updated and is also probably cost prohibitive.
4. Another question was "If the docs have Astral branding (i.e. Copyright -- not so much the theme) could they charge a premium to remove it?" and while I think the theoretical answer here is yes I will leave it up to the team to decide.

Overall I think this could be a good addition to the `uv` toolchain and think `pdoc` has the most similar basic functionality (Sphinx being much larger and feature-full) to what I am describing. `pdoc` was originally authored by BurntSushi who may be able to provide further guidance, but I will avoid adding this to his inbox :)

EDIT: I would be willing to work on this but would eventually need guidance on branding/styling from core maintainers. 

---

_Label `enhancement` added by @zanieb on 2024-10-29 22:26_

---

_Label `wish` added by @zanieb on 2024-10-29 22:26_

---

_Comment by @samypr100 on 2024-10-30 01:56_

Cool idea, I'd assume this likely involves both `uv` and `ruff` (code wise)

---

_Comment by @nanthony007 on 2024-12-03 02:44_

I got a preliminary piece of code working using `pyo3`, basically boils down to:

```python
let doc = obj.getattr("__doc__");
```
after parsing the module tree and identifying user defined functions.

However as I am posting this I am realizing that neither `uv` nor `ruff` use `pyo3`. 

---

_Referenced in [astral-sh/uv#11662](../../astral-sh/uv/issues/11662.md) on 2025-02-20 11:21_

---

_Comment by @miaomiao1992 on 2025-03-05 04:19_

pydoc is not as good as cargo doc

this issue proposed a good idea

---
