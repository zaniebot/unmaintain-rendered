```yaml
number: 2412
title: Enhance terraform default filter
type: pull_request
state: merged
author: vincentbockaert
labels: []
assignees: []
merged: true
base: master
head: feature/enhance-terraform-default-filter-types
created_at: 2023-02-08T16:31:34Z
updated_at: 2023-02-09T17:57:02Z
url: https://github.com/BurntSushi/ripgrep/pull/2412
synced_at: 2026-01-12T18:23:14Z
```

# Enhance terraform default filter

---

_@vincentbockaert_

The default filter for terraform only checks for *.tf files, but there are quite few other terraform filetypes.

The explanation for all of them can be found below:
_(including a link to the documentation from Hashicorp at the time of writing)_
- [*.tf.json & *.tfvars.json is to capture the files written in JSON-based variant of the Terraform language](https://developer.hashicorp.com/terraform/language/files)
- [*.tfvars is used to supply variables](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/variables#6-auto-tfvars-variable-files)
- [.terraform.lock.hcl is used as a Dependency lock file ](https://developer.hashicorp.com/terraform/language/files/dependency-lock
)
- [terraform.rc & .terraformrc, *.tfrc](https://developer.hashicorp.com/terraform/cli/config/config-file)


---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:254 on 2023-02-08 16:36_

Could you please follow the style of the surrounding code and make sure lines are wrapped to 79 columns inclusive?

---

_@BurntSushi requested changes on 2023-02-08 16:36_

Thanks! Just a style nit.

---

_@vincentbockaert reviewed on 2023-02-09 08:19_

---

_Review comment by @vincentbockaert on `crates/ignore/src/default_types.rs`:254 on 2023-02-09 08:19_

Hi, should be fine now, I think.

_& thanks for the quick response :smiley:, really like the tool_

---

_@BurntSushi approved on 2023-02-09 17:56_

---

_Merged by @BurntSushi on 2023-02-09 17:57_

---

_Closed by @BurntSushi on 2023-02-09 17:57_

---
