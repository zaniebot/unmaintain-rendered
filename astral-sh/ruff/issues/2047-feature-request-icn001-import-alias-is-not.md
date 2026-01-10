```yaml
number: 2047
title: "Feature Request: ICN001 import-alias-is-not-conventional checking `from package import module as alias` imports"
type: issue
state: closed
author: Zeddicus414
labels: []
assignees: []
created_at: 2023-01-21T03:17:31Z
updated_at: 2023-01-21T20:43:53Z
url: https://github.com/astral-sh/ruff/issues/2047
synced_at: 2026-01-10T11:09:45Z
```

# Feature Request: ICN001 import-alias-is-not-conventional checking `from package import module as alias` imports

---

_Issue opened by @Zeddicus414 on 2023-01-21 03:17_

It appears that ICN001 can be configured via this
```toml
[tool.ruff.flake8-import-conventions.extend-aliases]
"xml.dom.minidom" = "md"
```

To catch this import problem:
```python
import xml.dom.minidom as wrong
```

But it doesn't appear to be possible to configure the tool to catch this import as unconventional:
```python
from xml.dom import minidom as wrong
```

I'd like to try adding this feature. 

Based on my exploration, it appears that the `check_conventional_import` function just isn't called when the import is a `StmtKind::ImportFrom`. 
https://github.com/charliermarsh/ruff/blob/4e4643aa5d1587c024ce98f0c51363a4830b491d/src/checkers/ast.rs#L1012

My first thought would be to call the `check_conventional_import` function on the `ImportFrom`s and have something like `minidom = "md"` as enough to work in the config file... although, I'm not sure if the config needs some more syntax to ensure that these two imports are not treated the same:
```python
import minidom as md
from xml.dom import minidom as md
```

---

_Comment by @charliermarsh on 2023-01-21 03:38_

Sounds good -- I agree that we should support this.

I don't _think_ we need new syntax, since those two cases can be disambiguated as follows:

```py
import minidom as md  # "minidom" = "md"
from xml.dom import minidom as md  # "xml.dom.mindom" = "md"
```

---

_Comment by @charliermarsh on 2023-01-21 03:40_

(And yes -- you're looking in the right place! You'll need to construct the full module path (like `xml.dom.minidom`) and pass it to `check_conventional_import`, with one invocation per imported "member".)

---

_Comment by @Zeddicus414 on 2023-01-21 03:53_

I'm not sure if there is an easier way to do it, but I've got lots of `dbg!` statements all over the place in my local code.

This is what I have so far:
https://github.com/Zeddicus414/ruff/tree/ICN001-check-from-imports

But it does currently need `"minidom" = "md"` in the config to catch `from xml.dom import minidom as md`. I'll try to modify the function to account for both imports with just this line in the config: `"xml.dom.minidom" = "md"`

---

_Comment by @charliermarsh on 2023-01-21 04:07_

Take a look at the code on line 1175:

```rs
let full_name = helpers::format_import_from_member(
    level.as_ref(),
    module.as_deref(),
    &alias.node.name,
);
```

I _think_ that might help.

---

_Comment by @Zeddicus414 on 2023-01-21 04:15_

haha, that looks very useful!

I saw `level.as_ref()` and said to myself, "Isn't that what a borrow is supposed to be doing?" then my mind turned to mush reading the docs about the `AsRef` trait.

I think I'll try to get further on this tomorrow when I can think clearly again.

You've been crazy responsive today and very helpful. Thank you.

---

_Comment by @charliermarsh on 2023-01-21 05:03_

For sure! I'm not _always_ at my computer :joy: but when I am, I try to be as helpful as I can for new contributors. Appreciate your work here!


---

_Comment by @charliermarsh on 2023-01-21 05:04_

(BTW -- in that context, `level.as_ref()` is just a way to convert `&Option<usize>` to `Option<&usize>`. We prefer the latter on public APIs, but sometimes callers have the former and need to cast it.)

---

_Comment by @Zeddicus414 on 2023-01-21 18:59_

Alright, to the best of my knowledge, my branch is working. I'm hoping that how I achieved the result is okay. Please let me know if you are concerned about me duplicating code too much. I'm not sure if I did this the most DRY way possible.

Feel free to take a look if you'd like. Otherwise, I'm going to try to get my little test cases actually running under the test runner and work through the steps in the contribution guide to get this mergable.

---

_Comment by @Zeddicus414 on 2023-01-21 19:10_

Something noteworthy is that if you configure this:
```toml
"..src.LongClassNamedThing" = "Thing"
```
It will throw an ICN001 for this python code:
```python
from ..src import LongClassNamedThing as wrong
```

At the moment, I consider this kind of a useless behavior that is neither a feature or a bug... I didn't write a test case for this since I consider it sort of undocumented/undefined behavior.

---

_Closed by @charliermarsh on 2023-01-21 20:43_

---
