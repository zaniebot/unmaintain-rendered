```yaml
number: 10168
title: Improve documentation of the preview mode
type: pull_request
state: merged
author: hoel-bagard
labels:
  - documentation
assignees: []
merged: true
base: main
head: hoel/improve_preview_docs
created_at: 2024-02-29T10:41:54Z
updated_at: 2024-03-05T03:14:32Z
url: https://github.com/astral-sh/ruff/pull/10168
synced_at: 2026-01-10T22:47:01Z
```

# Improve documentation of the preview mode

---

_Pull request opened by @hoel-bagard on 2024-02-29 10:41_

## Summary

This PR was prompted by the discussion in #10153.
It adds CLI tab examples next to the `pyproject.toml` and `ruff.toml`  examples. It should be helpful for users wanting to try out the preview mode without modifying or creating a  `.toml` file.
It also adds a paragraph to try to make the effect of the preview mode less confusing.

---

_Comment by @hoel-bagard on 2024-02-29 10:43_

@buhtz does this added paragraph makes it less misleading in your opinion ?

---

_Comment by @buhtz on 2024-02-29 11:52_

It is an improvement.

Please see https://docs.astral.sh/ruff/rules/too-many-blank-lines/

> This rule is unstable and in [preview](https://docs.astral.sh/ruff/preview/). The --preview flag is required for use.

This is also misleading. I would suggest something like this (in all preview rules):

> The two switches `--select` and `--preview` are required for use.

---

_Comment by @hoel-bagard on 2024-02-29 12:25_

The `--select` part is required for nearly all rules (by default only `["E4", "E7", "E9", "F"]` are selected), so I don't think it makes sense to specify it only on preview rules.

---

_Comment by @buhtz on 2024-02-29 12:52_

> The `--select` part is required for nearly all rules (by default only `["E4", "E7", "E9", "F"]` are selected), so I don't think it makes sense to specify it only on preview rules.

But the docs state different. The docs say that you can use the `--preview` switch and all unstable rules are active. That is how some readers might understand it. And it would make sense by the way. My intention was to activate all unstable-rules to support your development via testing that rules.

---

_Review requested from @zanieb by @MichaReiser on 2024-02-29 12:57_

---

_Label `documentation` added by @MichaReiser on 2024-02-29 12:57_

---

_Review comment by @MichaReiser on `docs/preview.md`:8 on 2024-02-29 12:58_

Nit: I would avoid explicitly naming `output-format` here. We otherwise have to come up with a new preview feature everytime we remove the feature mentioned in the documentation.

---

_@MichaReiser approved on 2024-02-29 12:59_

Nice! I like it. I let @zanieb have a look at it too. 

---

_Comment by @MichaReiser on 2024-02-29 13:01_

> The two switches --select and --preview are required for use.

Whether you need to enable the rule using `select`, `extend-select` or similar depends on your current configuration. Using `--select` (or similar) is also not specific to `--preview` but applies to all rules. That's why what we have now makes more sense to me.

---

_@hoel-bagard reviewed on 2024-02-29 13:10_

---

_Review comment by @hoel-bagard on `docs/preview.md`:8 on 2024-02-29 13:10_

I hesitated when writing it, better keep it general indeed.
Removed in b0a9db688.

---

_@zanieb reviewed on 2024-03-02 14:21_

---

_Review comment by @zanieb on `docs/preview.md`:83 on 2024-03-02 14:21_

The `.` is superfluous in all of these examples, that's the default behavior these days.

---

_@zanieb reviewed on 2024-03-02 14:22_

---

_Review comment by @zanieb on `docs/preview.md`:83 on 2024-03-02 14:22_

We're using `extend-select` in all of the other examples, why use `--select` here?

---

_@zanieb reviewed on 2024-03-02 14:23_

---

_Review comment by @zanieb on `docs/preview.md`:64 on 2024-03-02 14:23_

If you want to add more here, I'd rephrase entirely to make it more general

> If `HYP001` were in preview, it would _not_ be enabled by adding it to the selected rule set.

---

_@zanieb reviewed on 2024-03-02 14:24_

---

_Review comment by @zanieb on `docs/preview.md`:132 on 2024-03-02 14:24_

Similarly, I'd just generalize these e.g.

```suggestion
However, it would be enabled in any of the above cases if you enabled preview mode:
```

---

_Review comment by @zanieb on `docs/preview.md`:8 on 2024-03-02 14:26_

I find this a little confusing. We have a flag that determines if preview rules require "explicit" selection but by default they do not i.e. "implicit" selection of the rule via a prefix is sufficient. I'd be much more direct here:

```suggestion
Enabling preview mode does not select all preview rules by default. See the [rules section](#using-rules-that-are-in-preview) for details on selecting preview rules.
```

I'd also consider putting it in a note admonition.

---

_@zanieb reviewed on 2024-03-02 14:26_

---

_@zanieb reviewed on 2024-03-02 14:31_

---

_Review comment by @zanieb on `docs/preview.md`:6 on 2024-03-02 14:31_

```suggestion
Preview mode enables a collection of unstable features such as new lint rules and fixes, formatter style changes, interface updates, and more. Warnings about deprecated features may turn into errors when using preview mode.
```

---

_@hoel-bagard reviewed on 2024-03-03 09:45_

---

_Review comment by @hoel-bagard on `docs/preview.md`:83 on 2024-03-03 09:45_

> The . is superfluous in all of these examples, that's the default behavior these days.

I chose to use the `.` to be consistent with the rest of the documentation (for example, see [the tutorial page](https://docs.astral.sh/ruff/tutorial/)). If it's better to remove the `.`, then I can open another PR to remove it from the rest of the documentation, what do you think ?

> We're using extend-select in all of the other examples, why use --select here?

It was a mistake, I fixed it in 9f5d9bcdf.

---

_Review comment by @hoel-bagard on `docs/preview.md`:64 on 2024-03-03 09:48_

Done in 9b6b7bd19.

---

_@hoel-bagard reviewed on 2024-03-03 09:48_

---

_Comment by @github-actions[bot] on 2024-03-03 10:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@buhtz reviewed on 2024-03-03 11:18_

I like that solution.

---

_@zanieb reviewed on 2024-03-03 15:17_

---

_Review comment by @zanieb on `docs/preview.md`:83 on 2024-03-03 15:17_

Sure we can address that separately, thanks!

---

_@hoel-bagard reviewed on 2024-03-03 23:27_

---

_Review comment by @hoel-bagard on `docs/preview.md`:83 on 2024-03-03 23:27_

Ok, I'll do that then. I've removed the `.` in this commit [356c4b0](https://github.com/astral-sh/ruff/pull/10168/commits/356c4b0580f63a918936a04b061b6503a28470d0).

---

_Merged by @charliermarsh on 2024-03-05 03:08_

---

_Closed by @charliermarsh on 2024-03-05 03:08_

---
