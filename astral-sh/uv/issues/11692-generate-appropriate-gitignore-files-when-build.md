```yaml
number: 11692
title: Generate appropriate .gitignore files when build-backend option is specified for init command
type: issue
state: open
author: hajifkd
labels:
  - enhancement
assignees: []
created_at: 2025-02-21T08:38:19Z
updated_at: 2025-11-07T03:39:12Z
url: https://github.com/astral-sh/uv/issues/11692
synced_at: 2026-01-12T16:00:44Z
```

# Generate appropriate .gitignore files when build-backend option is specified for init command

---

_@hajifkd_

### Summary

When a new project is created by the init command, `.gitignore` is also automatically created, probably here:
https://github.com/astral-sh/uv/blob/88aa6e26d15828af514038ba04459830ee2ed00b/crates/uv-configuration/src/vcs.rs#L69
However, currently, the gitignore file is constant and does not respect anything, especially build-backend options.
Users currently need to add additional specifications to gitignore by hand after the project is created.
For example, when I used maturin for the build-backend, I had to add several lines to gitignore.
Of course it was completely fine to add lines by hand, but I was wondering if it was intentional that, say, the `target` folder was not ignored, and a bit confused.
Maybe adding all possible gitignore lines would be useful although I'm not sure if it could cause potential conflict.


### Example

_No response_

---

_Label `enhancement` added by @hajifkd on 2025-02-21 08:38_

---

_Comment by @Gankra on 2025-02-21 17:23_

This would indeed be cool/nice, although it's a bit difficult when the entire premise of build-backends is that anyone can make a new one because they follow a rigid structure. We don't really have much build-backend-specific code at all as a result, so this would potentially add a big hazardous maintenance burden.

The most likely place where we might do something here is with [our own build backend, uv_build](https://github.com/astral-sh/uv/pull/11446), where we can safely and accurately maintain the gitignore without stepping on any other project's toes. 

---

_Comment by @mxngjxa on 2025-11-07 03:39_

### 💡 Suggestion: Add support for custom `.gitignore` templates in `uv init`

**Summary:**
Currently, `uv init` can initialize a Git repository via `--vcs git`, but it doesn’t allow specifying or customizing a `.gitignore` file. It either creates a default one (if any) or leaves it to the user to add manually.

**Proposal:**
Add an option such as:

```bash
uv init --gitignore <path or template>
```

This could:

* Accept a path to a custom `.gitignore` file, or
* Support predefined templates (e.g., `python`, `rust`, `none`) similar to GitHub’s `.gitignore` templates.

**Benefit:**
This would streamline project setup, reduce manual steps, and make `uv init` more flexible for teams with standard repository configurations.


---
