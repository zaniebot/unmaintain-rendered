```yaml
number: 11183
title: "Write `ruff server` setup guide for Helix"
type: pull_request
state: merged
author: snowsignal
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: jane/server/editors/helix/setup
created_at: 2024-04-28T03:22:35Z
updated_at: 2024-04-30T17:15:30Z
url: https://github.com/astral-sh/ruff/pull/11183
synced_at: 2026-01-12T15:55:36Z
```

# Write `ruff server` setup guide for Helix

---

_@snowsignal_

## Summary

Closes #11027.


---

_Label `documentation` added by @snowsignal on 2024-04-28 03:22_

---

_Label `server` added by @snowsignal on 2024-04-28 03:22_

---

_Review comment by @charliermarsh on `crates/ruff_server/docs/setup/HELIX.md`:4 on 2024-04-28 12:55_

Nit: "be at", like the last fragment?

---

_Review comment by @charliermarsh on `crates/ruff_server/docs/setup/HELIX.md`:11 on 2024-04-28 12:56_

Nit: formatting here is different (no space after and before hard brackets) unlike the below. I'd probably just remove the spacing in both cases.

---

_@charliermarsh reviewed on 2024-04-28 12:57_

I think there are some nice features in the existing Helix setup guide that we should replicate here (https://github.com/astral-sh/ruff-lsp?tab=readme-ov-file#example-helix):

- `auto-format = true`?
- Demonstrating how to configure it (`[language-server.ruff.config.settings]`)
- Including a screenshot of the editor once you've reached the success state

---

_Review requested from @charliermarsh by @snowsignal on 2024-04-29 16:32_

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/HELIX.md`:51 on 2024-04-29 16:35_

Do we have a list of allowed settings here somewhere which we can link it to? If not, it might be a useful reference.

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/HELIX.md`:24 on 2024-04-29 16:42_

We could also provide an example configuration to disable hover (similar to Neovim) if Helix has the same problem as Neovim which is that the client will ask for hover to all the configured servers but Ruff only provides for `# noqa` comments. So, Neovim complains that there's no information via Ruff.

<img width="864" alt="Screenshot 2024-04-29 at 22 09 43" src="https://github.com/astral-sh/ruff/assets/67177269/fc5e8f7b-3833-4762-9b20-c11726323d93">

You can confirm if it's the same problem in Helix and if it is, we can recommend a similar config:

```toml
[[language]]
name = "python"
language-servers = [ { name = "ruff", except-features = [ "hover" ] }, "pyright" ]
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/HELIX.md`:30 on 2024-04-29 16:44_

nit: Just a FYI that the latest version to support configuring multiple language servers is 23.10 which was released 6 months ago so it might be ok to not mention it here (https://github.com/helix-editor/helix/blob/master/CHANGELOG.md#2310-2023-10-24)

---

_Comment by @github-actions[bot] on 2024-04-29 16:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-04-29 16:44_

---

_@snowsignal reviewed on 2024-04-29 16:46_

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/HELIX.md`:51 on 2024-04-29 16:46_

We don't - I'll open an issue for that.

---

_@snowsignal reviewed on 2024-04-30 14:52_

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/HELIX.md`:24 on 2024-04-30 14:52_

Hover information is somewhat hidden by default in Helix (you have to press `Space-k` to access it, and it opens a new popup window). So I think we're good as-is.

---

_@dhruvmanila reviewed on 2024-04-30 16:18_

---

_Review comment by @dhruvmanila on `crates/ruff_server/docs/setup/HELIX.md`:24 on 2024-04-30 16:18_

Yes, it's similar in Neovim. It's fine to not add it, I was just wondering if Helix also shows a log message like the one in Neovim's case as shown above with "No information available". 

---

_@snowsignal reviewed on 2024-04-30 16:20_

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/HELIX.md`:24 on 2024-04-30 16:20_

It just doesn't show anything if hover information isn't available.

---

_Review comment by @snowsignal on `crates/ruff_server/docs/setup/HELIX.md`:51 on 2024-04-30 17:15_

Issue opened: https://github.com/astral-sh/ruff/issues/11217

---

_@snowsignal reviewed on 2024-04-30 17:15_

---

_Merged by @snowsignal on 2024-04-30 17:15_

---

_Closed by @snowsignal on 2024-04-30 17:15_

---

_Branch deleted on 2024-04-30 17:15_

---
