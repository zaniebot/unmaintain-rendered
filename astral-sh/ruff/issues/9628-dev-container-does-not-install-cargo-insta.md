```yaml
number: 9628
title: "Dev-container does not install `cargo insta`"
type: issue
state: closed
author: MartinBernstorff
labels: []
assignees: []
created_at: 2024-01-23T18:35:52Z
updated_at: 2024-01-23T18:37:06Z
url: https://github.com/astral-sh/ruff/issues/9628
synced_at: 2026-01-10T11:09:51Z
```

# Dev-container does not install `cargo insta`

---

_Issue opened by @MartinBernstorff on 2024-01-23 18:35_

Just booted up the dev container, and get:

```bash
vscode ➜ /workspaces/ruff (mbern/8061/td003-support-vscode-github-syntax) $ cargo insta review
error: no such command: `insta`

        Did you mean `init`?

        View all installed commands with `cargo --list`
        Find a package to install `insta` with `cargo search cargo-insta`
```

Did I do something wrong? Not experienced in the Rust ecosystem :-)

---

_Comment by @MartinBernstorff on 2024-01-23 18:37_

Huh, seems my `post-create` command failed for some reason. Rebuilding fixed it, apologies.

---

_Closed by @MartinBernstorff on 2024-01-23 18:37_

---
