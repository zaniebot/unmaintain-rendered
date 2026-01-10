```yaml
number: 15195
title: allow publishing a more detailed graph
type: issue
state: open
author: purajit
labels:
  - needs-decision
  - analyze
assignees: []
created_at: 2024-12-30T06:09:04Z
updated_at: 2025-01-06T23:22:08Z
url: https://github.com/astral-sh/ruff/issues/15195
synced_at: 2026-01-10T11:09:56Z
```

# allow publishing a more detailed graph

---

_Issue opened by @purajit on 2024-12-30 06:09_

In addition to the options to publish the graph based on module paths and including third-party deps 
mentioned in https://github.com/astral-sh/ruff/issues/13431, publishing a `--detailed` graph would be great! It would allow people to build or
conceive of several other tools on top of ruff, and also allow ruff to swallow them up.

This could include information like:
* whether this is from a string or not
  * if it could go so deep as to being able to detect whether it's being mocked, dynamically imported, 
opened as a pkg resource, etc, that would be absolutely nuts, at least in obvious cases
* whether this is `TYPING`-only
* whether this is a top-level import or not
* whether private members are imported (I guess this starts pushing the graph towards being not 
just for modules but for module members)
* whether it's a relative import
* any attached annotations (particularly pylint, also semgrep)

I understand there would be some non-trivial restructure or reconsideration, since there could be multiple 
dependencies on the same module with differing characteristics

---

_Label `needs-decision` added by @MichaReiser on 2024-12-30 08:09_

---

_Label `analyze` added by @MichaReiser on 2024-12-30 08:09_

---

_Comment by @nickdrozd on 2025-01-06 20:01_

This is a great idea. Other info that would be nice to have:

- Stats about module imports
  - For a given module, how many modules import it?
  - Total number of imports
  - Modules that import the most / are imported the most
- Transitive imports, i.e. A imports from B, B imports from C, but also A imports from C (could indicate interface design flaw)
- Could enforce a maximum number of module imports, since too many imports could indicate design flaw.

---

_Comment by @purajit on 2025-01-06 23:22_

@nickdrozd aren't the first 2 derivable from the graph it already outputs? For example, my
project https://github.com/purajit/ruff-tools is all about transitive deps, as well as detecting
cyclical deps, etc. I feel any derivable information should be left up to individual users.

For the last one, that sounds like a new ruff linter rule so would be better as a separate proposal.

---
