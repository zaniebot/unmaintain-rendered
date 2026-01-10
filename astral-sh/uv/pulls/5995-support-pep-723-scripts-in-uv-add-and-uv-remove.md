```yaml
number: 5995
title: "Support PEP 723 scripts in `uv add` and `uv remove`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: add-pep732
created_at: 2024-08-10T14:45:13Z
updated_at: 2024-08-11T08:02:53Z
url: https://github.com/astral-sh/uv/pull/5995
synced_at: 2026-01-10T13:31:54Z
```

# Support PEP 723 scripts in `uv add` and `uv remove`

---

_Pull request opened by @blueraft on 2024-08-10 14:45_

## Summary

Resolves https://github.com/astral-sh/uv/issues/4667

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2024-08-10 14:48_

Thanks, look forward to reviewing, this feature is gonna be amazing.

If you haven’t already (sorry, on phone), can you ensure there’s test coverage for:

- Adding a dep with an existing script tag
- Adding a dep without an existing script tag (with a shebang present, with a docstring present, etc.)
- Removing the last dep in an existing script tag (I think it’s fine to leave the script tag rather than removing it entirely, even if the dep list is empty).

---

_Comment by @blueraft on 2024-08-10 15:09_

- [x] Adding a dep with an existing script tag 
- [ ] Adding a dep without an existing script tag (this is not supported yet, but I can take a look tomorrow)
- [x] Removing the last dep in an existing script tag 

---

_@charliermarsh reviewed on 2024-08-10 16:12_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:77 on 2024-08-10 16:12_

Nit: this should be `from_str`.

---

_@charliermarsh reviewed on 2024-08-10 16:12_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:22 on 2024-08-10 16:12_

Lets call this `raw` for consistency?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:264 on 2024-08-10 16:13_

These actually do support sources now.

---

_@charliermarsh reviewed on 2024-08-10 16:13_

---

_@charliermarsh reviewed on 2024-08-10 16:15_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:161 on 2024-08-10 16:15_

Since we break, doesn't the `python_script` get truncated?

---

_@charliermarsh reviewed on 2024-08-10 16:15_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:110 on 2024-08-10 16:15_

What is the "python script"? Can you show an example in the docstring here?

---

_@charliermarsh reviewed on 2024-08-10 16:20_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:1144 on 2024-08-10 16:20_

I think we should consider moving this up to where we parse the script for `uv run`, nearer to the top of the file.

---

_@charliermarsh reviewed on 2024-08-10 16:21_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2526 on 2024-08-10 16:21_

I think `--script` should be marked as conflicting with a bunch of other options, like: `--locked`, `--frozen`, `--dev`, etc. Or, we can warn in `add` and `remove` if those are set, to indicate that they're ignored, like in https://github.com/astral-sh/uv/pull/5977. Fine to just follow that pattern.

---

_Label `enhancement` added by @charliermarsh on 2024-08-10 16:21_

---

_Label `preview` added by @charliermarsh on 2024-08-10 16:21_

---

_Comment by @charliermarsh on 2024-08-10 16:21_

Pumped for this!

---

_Comment by @zanieb on 2024-08-10 16:25_

Wow so exciting!

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv-cli/src/lib.rs`:2526 on 2024-08-10 18:35_

I've added a warning for the other options, let me know if I missed something

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv/src/lib.rs`:1144 on 2024-08-10 18:35_

Done

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv-scripts/src/lib.rs`:110 on 2024-08-10 18:35_

By this I meant the python code in the rest of the script, I've added an example and some further explanation for this.

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv-scripts/src/lib.rs`:161 on 2024-08-10 18:35_

`python_script` gets all of the remaining lines for the files before we break so it should contain the rest of the script file.

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/add.rs`:264 on 2024-08-10 18:35_

Added support for sources and a test for this

---

_@blueraft reviewed on 2024-08-10 18:35_

---

_Review comment by @blueraft on `crates/uv-scripts/src/lib.rs`:77 on 2024-08-10 18:35_

Clippy is not happy with `from_str`

```
warning: method `from_str` can be confused for the standard trait method `std::str::FromStr::from_str`
```j

---

_@charliermarsh reviewed on 2024-08-10 18:51_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:77 on 2024-08-10 18:51_

I think it's ok to implement the trait instead of as an associated method here. But I can also do it before merging and show you as an example, either is fine.

---

_@blueraft reviewed on 2024-08-10 19:04_

---

_Review comment by @blueraft on `crates/uv-scripts/src/lib.rs`:77 on 2024-08-10 19:04_

Done

---

_Comment by @blueraft on 2024-08-10 19:16_

> Adding a dep without an existing script tag

Can you expand on this a little about what we want to support here?

Just to clarify, do we want to support the following use case with uv add?

Suppose the script file contains this code:

```python

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

If the user runs:

```bash
uv add rich requests --script example.py
```

Do we intend for uv add to transform the script by adding a metadata block like this at the top?

```python

# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests",
#   "rich",
# ]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

---

_Comment by @charliermarsh on 2024-08-10 19:18_

Yeah that’s what I was thinking.

My guess is it needs to appear after any shebang though (but maybe before any docstrings?)

---

_Comment by @blueraft on 2024-08-10 20:33_

Thank you, that makes sense. I'll look into adding that. 

---

_Comment by @blueraft on 2024-08-10 20:53_

> Adding a dep without an existing script tag 

This should now support adding a dependency even when there is no existing script tag in the file.

---

_Comment by @charliermarsh on 2024-08-11 00:27_

Great! I assume that means ready to review, I'll go through it now.

---

_@charliermarsh approved on 2024-08-11 01:16_

---

_@charliermarsh reviewed on 2024-08-11 01:16_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2526 on 2024-08-11 01:16_

Looks good. I marked `--dev` and `--optional` as conflicting, since those should truly error. The rest are more "redundant".

---

_Comment by @charliermarsh on 2024-08-11 01:19_

Great, thank you!

---

_Merged by @charliermarsh on 2024-08-11 01:40_

---

_Closed by @charliermarsh on 2024-08-11 01:40_

---

_Branch deleted on 2024-08-11 08:02_

---
