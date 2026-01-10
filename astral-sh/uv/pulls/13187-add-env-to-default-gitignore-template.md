```yaml
number: 13187
title: Add .env to default .gitignore template  
type: pull_request
state: closed
author: AsharibAli
labels: []
assignees: []
base: main
head: add-env-to-gitignore
created_at: 2025-04-29T01:06:15Z
updated_at: 2025-05-18T21:30:22Z
url: https://github.com/astral-sh/uv/pull/13187
synced_at: 2026-01-10T11:10:41Z
```

# Add .env to default .gitignore template  

---

_Pull request opened by @AsharibAli on 2025-04-29 01:06_

This PR adds `.env` files to the default `.gitignore` template that's generated when initializing a new uv project.
  
## Motivation  
`.env` files typically contain environment-specific configuration and secrets that shouldn't be committed to version control. The uv project already has support for `.env` files in the `uv run` command, so adding this to the default `.gitignore` aligns with best practices and the existing functionality.  
  
## Changes  
- Added `.env` to the `GITIGNORE` constant in `crates/uv-configuration/src/vcs.rs`

---

_Comment by @HenriBlacksmith on 2025-04-29 09:37_

I do not think this is always valid.

There are many situations where you might want to commit that file.

Many items could be added to the .gitignore but I assume that the goal here is to only consider the files which we are sure will always be ignored 

---

_Comment by @AsharibAli on 2025-04-29 09:56_

> I do not think this is always valid.
> 
> There are many situations where you might want to commit that file.
> 
> Many items could be added to the .gitignore but I assume that the goal here is to only consider the files which we are sure will always be ignored

Bro! Who wants to commit a `.env` file? I always have to add `.env` to the `.gitignore` file. I'm facing this problem, which is why I think it's necessary to include it.

---

_Comment by @notatallshaw on 2025-04-29 14:15_

FWIW developers can set up their own global core exclude file for patterns they never want to commit but the some projects don't exclude: https://sebastiandedeyne.com/setting-up-a-global-gitignore-file/

---

_Comment by @AsharibAli on 2025-04-29 15:58_

> FWIW developers can set up their own global core exclude file for patterns they never want to commit but the some projects don't exclude: https://sebastiandedeyne.com/setting-up-a-global-gitignore-file/

Not everyone is aware of global `.gitignore` setup, around 80% of developers might not know about it. Adding `.env` to the project's default `.gitignore` ensures better security and aligns with common practices.

---

_Comment by @charliermarsh on 2025-05-18 21:30_

I'm going to close for now since we've kept this minimal.

---

_Closed by @charliermarsh on 2025-05-18 21:30_

---
