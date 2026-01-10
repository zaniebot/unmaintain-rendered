```yaml
number: 14579
title: Implement an auto-import proof-of-concept
type: pull_request
state: open
author: charliermarsh
labels:
  - no-build
  - no-test
assignees: []
draft: true
base: main
head: charlie/auto-import
created_at: 2025-07-12T18:58:22Z
updated_at: 2025-07-15T19:30:19Z
url: https://github.com/astral-sh/uv/pull/14579
synced_at: 2026-01-10T06:53:02Z
```

# Implement an auto-import proof-of-concept

---

_Pull request opened by @charliermarsh on 2025-07-12 18:58_

## Summary

I don't know if this will ship but I wanted to try it. You can now run `uv add --script /path/to/script.py --auto` to automatically populate the PEP 723 tag with the inferred script dependencies.

For now, this uses the module name-to-package name mapping provided by [pipreqs](https://github.com/bndr/pipreqs/blob/2ac9e731c69dee31470cccd23c6b503a4a161778/pipreqs/mapping) since it was the simplest thing to prototype, but I think it's plausible that there are more robust mappings out there. For example:

- Replit ships a generated mapping in [UPM](https://github.com/replit/upm/blob/main/internal/backends/python/pkgs.json) which also includes download counts.
- Marimo ships one [here](https://github.com/marimo-team/marimo/blob/7c5cd6fc2701186c45fcc4b4c4f4536ada14ea8f/marimo/_runtime/packages/module_name_to_pypi_name.py#L7).
- The Conda ecosystem has something [here](https://github.com/regro/cf-graph-countyfair/blob/33709efce8b81e349202b607859ed73bc206afda/mappings/pypi/name_mapping.json) that's also generated, but it's partly focused on mapping Conda to PyPI names (and it omits `sklearn`, perhaps as a result?).

We could also generate our own, or provide our own server to perform this resolution.

The other major consideration here is that pulling in the Ruff parser does add some weight to the binary.


---

_Label `no-build` added by @charliermarsh on 2025-07-12 18:58_

---

_Label `no-test` added by @charliermarsh on 2025-07-12 18:58_

---

_Comment by @charliermarsh on 2025-07-12 23:08_

I believe this adds ~1.4MB to the release binary before compression:
```
❯ du main
71328	main

❯ du auto-import
74264	auto-import
```

---

_Comment by @geofft on 2025-07-13 03:41_

Weird idea: what if this is part of Ruff instead of uv? I would guess that this uses more code from Ruff than from uv. I think all you need out of uv is the ability to parse existing requirements in pyproject.toml format to determine if there's already a requirement that provides the dependency you want, which shouldn't require very much parsing since you don't actually need to make sense of any version constraints or markers that you see, and and the ability to edit TOML. You don't actually need any packaging/environment stuff beyond what's being vendored from pipreqs, I think.

And conceptually this could be a `ruff check --fix --select RUFnnn`. (Bonus points if astral-sh/ruff#10457 happens and one thing that the RUFnnn fix can do is add itself to the inline metadata block if not present. :) )

One argument is that the way it edits a file seems a little more like what I expect out of Ruff than out of uv. While `uv add --script` exists, it's pretty straightforward and deterministic about what it does. For tools like Ruff and Black where they might surprise me, I'm used to using `--diff` etc. confirm what they do. Ruff already has those options and uv does not. Also you can roll this into `ruff check --watch` and the LSP to automatically add the dependency when you add the import statement.

One possible argument for keeping it in uv is that presumably the same logic makes sense for actual pyproject.toml files. (Though it still seems useful to have an LSP in that scenario....) You could also imagine wanting this for interactive Python sessions or notebooks where yo'd actually want to install the package, hm.

Another argument for keeping it in uv: what if you're importing something that's provided by some other PyPI package or a local package, but the import name happens to have a different hit in the database? uv can instantiate the environment and go look at it.

---

_Comment by @geofft on 2025-07-15 19:30_

Also just wanted to drop a link to the recently-drafted [PEP 794 – Import Name Metadata](https://peps.python.org/pep-0794/) which is relevant to this problem space. It adds two core metadata fields, `Import-Name` for importable names uniquely provided by the dist, and `Import-Namespace` for importable names that can also be provided by other non-conflicting dists. An example from the PEP:
```toml
[project]
import-names = ["azure.mgmt.search"]
import-namespaces = ["azure", "azure.mgmt"]
```

---
