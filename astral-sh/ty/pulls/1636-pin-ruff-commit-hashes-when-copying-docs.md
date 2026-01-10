```yaml
number: 1636
title: pin ruff commit hashes when copying docs
type: pull_request
state: open
author: oconnor663
labels: []
assignees: []
base: main
head: ruff_commit_in_links
created_at: 2025-11-25T21:18:16Z
updated_at: 2025-11-26T09:32:07Z
url: https://github.com/astral-sh/ty/pull/1636
synced_at: 2026-01-10T02:34:10Z
```

# pin ruff commit hashes when copying docs

---

_Pull request opened by @oconnor663 on 2025-11-25 21:18_

This is a drive-by fix/idea. I noticed that we're publishing links with line numbers that tend to go stale as the upstream Rust code is edited over time (like most of the "View source" links on [this page](https://docs.astral.sh/ty/reference/rules)). Might be nice to snapshot these when we vendor the `.md` files?

---

_Review requested from @carljm by @oconnor663 on 2025-11-25 21:18_

---

_@chatgpt-codex-connector[bot] reviewed on 2025-11-25 21:20_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `scripts/autogenerate_files.sh`:23 on 2025-11-25 21:20_

**<sub><sub>![P1 Badge](https://img.shields.io/badge/P1-orange?style=flat)</sub></sub>  POSIX sh script now uses bash-specific conditional**

The new `copy_docs` uses `[[ -d "$dest" ]]` under a `/usr/bin/env sh` shebang. On systems where `sh` is dash (Debian/Ubuntu default), `[[` is unsupported and the script now aborts with a syntax error before copying any docs. Use POSIX `[ -d "$dest" ]` or switch the shebang to bash to keep the tool runnable.

Useful? React with 👍 / 👎.

---

_Comment by @oconnor663 on 2025-11-25 21:25_

I guess if we did want to do this, we'd need to remove the CI check that specifically tests we didn't change anything...

---

_Review requested from @zanieb by @oconnor663 on 2025-11-25 23:39_

---

_Comment by @oconnor663 on 2025-11-25 23:41_

Adding @zanieb as a reviewer. Low priority, no rush, etc. I'm curious to get your take as the CEO-at-large of Rooster :)

---

_Review requested from @MichaReiser by @carljm on 2025-11-25 23:42_

---

_Review comment by @MichaReiser on `scripts/autogenerate_files.sh`:30 on 2025-11-26 09:29_

I think we only need this for `rules.md`, no other markdown file contains line-references

---

_@MichaReiser approved on 2025-11-26 09:31_

---

_Comment by @MichaReiser on 2025-11-26 09:32_

Hmm, you now run into issues with our check-generated files unedited. That script will require the same pre-processing 

---
