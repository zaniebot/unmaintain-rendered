```yaml
number: 2827
title: "Add `*.tfvars` to list of Terraform filetypes"
type: pull_request
state: closed
author: sorliem
labels: []
assignees: []
base: master
head: patch-1
created_at: 2024-05-28T21:35:21Z
updated_at: 2024-06-05T20:34:23Z
url: https://github.com/BurntSushi/ripgrep/pull/2827
synced_at: 2026-01-12T18:23:14Z
```

# Add `*.tfvars` to list of Terraform filetypes

---

_@sorliem_

Hello,
I'd like to propose adding `*.tfvars` to the list of terraform filetypes. In setups with multiple workspaces, we have different configuration for say `staging.tfvars` and `production.tfvars`. This might override `terraform.tfvars`, so not sure if that should be removed.

Thank you 🙂 

---

_Comment by @sorliem on 2024-06-05 20:34_

Actually, nevermind 😄 I discovered the arg: `--type-add='tf:*.tfvars'` which fixes my issue.

---

_Closed by @sorliem on 2024-06-05 20:34_

---

_Branch deleted on 2024-06-05 20:34_

---
