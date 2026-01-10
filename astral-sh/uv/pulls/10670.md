```yaml
number: 10670
title: Add elvish activation script
type: pull_request
state: open
author: balthild
labels:
  - enhancement
  - needs-decision
  - virtualenv
assignees: []
base: main
head: elvish
created_at: 2025-01-16T08:59:28Z
updated_at: 2025-06-29T19:17:38Z
url: https://github.com/astral-sh/uv/pull/10670
synced_at: 2026-01-10T06:53:01Z
```

# Add elvish activation script

---

_Pull request opened by @balthild on 2025-01-16 08:59_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Supports activation script for elvish

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I filled the template variables manually and tried in a project. It works well.

```shell
D:\Projects\Python\project-name on  main [?] is 📦 v0.1.0 via 🐍 v3.13.1 
❯ python -V
Python 3.13.1

D:\Projects\Python\project-name on  main [?] is 📦 v0.1.0 via 🐍 v3.13.1 
❯ source .venv\Scripts\activate.elv

D:\Projects\Python\project-name on  main [?] is 📦 v0.1.0 via 🐍 v3.12.8 (project-name) 
❯ python -V
Python 3.12.8

D:\Projects\Python\project-name on  main [?] is 📦 v0.1.0 via 🐍 v3.12.8 (project-name) 
❯ deactivate

D:\Projects\Python\project-name on  main [?] is 📦 v0.1.0 via 🐍 v3.13.1 
❯ python -V
Python 3.13.1
```

<!-- How was it tested? -->


---

_Label `enhancement` added by @Gankra on 2025-01-16 15:15_

---

_Label `virtualenv` added by @Gankra on 2025-01-16 15:15_

---

_Label `needs-decision` added by @zanieb on 2025-01-16 15:36_

---

_Comment by @zanieb on 2025-01-16 15:36_

We'll need to decide if we want to maintain support for additional shells.

---

_Comment by @TyceHerrman on 2025-02-18 06:26_

@zanieb, +1 of someone that would appreciate support for elvish! 

---

_Comment by @priscira on 2025-06-29 08:34_

Hello, I have also recently implemented an `elvish`‘s `activate.elv` script. Our approach is quite similar, but I found that the `use` command of `elvish` does not support `re-import`. This means that after executing `deactivate`, if we want to enter the `venv` environment again, it will not take effect.
> [Elvish Document about re-importing](https://elv.sh/ref/language.html#re-importing):
> Modules are cached after one import. Subsequent imports do not re-execute the module; they only serve the bring it into the current scope. Moreover, the cache is keyed by the path of the module, not the name under which it is imported.

I haven't tried your script yet, but if your script needs to be activated through `use ./xxx/activate`, there is a high probability that this issue will also occur.

---

_Comment by @balthild on 2025-06-29 19:17_

I use eval:

```elvish
fn source {|path|
  eval (slurp < $path)
}
```

---
