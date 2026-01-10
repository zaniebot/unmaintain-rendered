```yaml
number: 944
title: Update editor settings reference with new format
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/editor-settings
created_at: 2025-08-06T08:17:49Z
updated_at: 2025-08-07T05:01:24Z
url: https://github.com/astral-sh/ty/pull/944
synced_at: 2026-01-10T02:34:10Z
```

# Update editor settings reference with new format

---

_Pull request opened by @dhruvmanila on 2025-08-06 08:17_

## Summary

This PR updates the documentation to reflect the changes made in https://github.com/astral-sh/ruff/pull/19614 and https://github.com/astral-sh/ty-vscode/pull/106.

## Test Plan

<details><summary>Screenshot of the new editor settings reference page:</summary>
<p>

<img width="4992" height="3702" alt="image" src="https://github.com/user-attachments/assets/e3afd5a3-8b5b-43c3-addd-e1d6636ab1b7" />

</p>
</details> 


---

_Label `documentation` added by @dhruvmanila on 2025-08-06 08:17_

---

_@dhruvmanila reviewed on 2025-08-06 09:12_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:9 on 2025-08-06 09:12_

I'm not sure if we want to provide this kind of information to the user facing docs, happy to hear others thoughts.

---

_Marked ready for review by @dhruvmanila on 2025-08-06 09:13_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:9 on 2025-08-06 12:20_

I'd remove it. It's a nice feature, but not really relevant when configuring the editor (it only describes how ty will respond to configuration changes).

I think the bigger question here is if and how we want to group the settings.

I think I would remove *Runtime settings* but keep *Initialization options*. I'm not a huge fan of the terminology because it isn't really clear for users what initialization options are. Given that the only supported options are logging related, should we rename the section to `Logging options` (and maybe move it to the very bottom?)

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:9 on 2025-08-06 12:26_

Not sure if I like it more. But this avoids using language server that many times:

```
Disables language services functionality such as code completion, hover, go to definition, etc.
```

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:12 on 2025-08-06 12:26_

```suggestion
This is useful if you want to use ty exclusively for type checking in combination with another language
server for features like code completion, hover, go to definition, etc.
```

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:162 on 2025-08-06 12:28_

I'd rephrase or extend this section. Its current framing contains a lot of details that aren't relevant for VS code and I think also Zed users. I think it only applies to neovim users. 


---

_@MichaReiser approved on 2025-08-06 12:28_

---

_@dhruvmanila reviewed on 2025-08-07 04:35_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:9 on 2025-08-07 04:35_

> I think I would remove _Runtime settings_ but keep _Initialization options_. I'm not a huge fan of the terminology because it isn't really clear for users what initialization options are. Given that the only supported options are logging related, should we rename the section to `Logging options` (and maybe move it to the very bottom?)

This is a good idea but I'm worried in the future we might have to rename it if there's anything other than logging related options that gets added. I think I'd keep it "initialization options" for now but I've updated the brief at the start to avoid using "language server" and change the language to be more towards using ty in an editor.

---

_@dhruvmanila reviewed on 2025-08-07 04:37_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:9 on 2025-08-07 04:37_

Another reason to avoid the "Logging options" section is that the `ty.trace.server` should also be part of that section but it's only specific to VS Code.

---

_@dhruvmanila reviewed on 2025-08-07 04:40_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:9 on 2025-08-07 04:40_

Yeah, good idea. I think something like "... language services that ty provides like ..." sounds reasonable (which I've updated to). What do you think?

---

_@dhruvmanila reviewed on 2025-08-07 04:42_

---

_Review comment by @dhruvmanila on `docs/reference/editor-settings.md`:162 on 2025-08-07 04:42_

They're relevant for Zed users. In general, these are relevant for all editors except for VS Code because the ty VS Code extension provides a different settings interface.

---

_Comment by @dhruvmanila on 2025-08-07 05:01_

(Going to merge this, but happy to do any follow-up if required.)

---

_Merged by @dhruvmanila on 2025-08-07 05:01_

---

_Closed by @dhruvmanila on 2025-08-07 05:01_

---

_Branch deleted on 2025-08-07 05:01_

---
