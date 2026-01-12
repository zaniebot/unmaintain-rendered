```yaml
number: 4297
title: "Implement `--extend-fixable` option"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extend-fixable
created_at: 2023-05-09T01:34:33Z
updated_at: 2023-05-19T02:20:20Z
url: https://github.com/astral-sh/ruff/pull/4297
synced_at: 2026-01-12T03:50:02Z
```

# Implement `--extend-fixable` option

---

_Pull request opened by @charliermarsh on 2023-05-09 01:34_

# Summary

This PR adds an `--extend-fixable` command-line and configuration file option, and changes the semantics to match those of `--select` (i.e., providing a new `--select` wipes out previous ignores, and so on).

Closes #4289.

---

_Comment by @github-actions[bot] on 2023-05-09 01:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-05-09 06:05_

My concern with adding `--extend-fixable`, or any other new CLI argument that directly match to a configuration option is that we ultimately end up with a whole lot of CLI arguments: One to override every option in the configuration. What do you think of adding a `--config [key1=value1,key2=value2,...]` option that allows specifying ANY option in the configuration similar to `rustfmt` or `clippy`? 

---

_Comment by @charliermarsh on 2023-05-09 18:44_

I'm generally open to something like that, though I think in this case, it does make sense to add `--extend-fixable`, since it's an inconsistency in the API:

```
Rule selection:
      --select <RULE_CODE>
          Comma-separated list of rule codes to enable (or ALL, to enable all rules)
      --ignore <RULE_CODE>
          Comma-separated list of rule codes to disable
      --extend-select <RULE_CODE>
          Like --select, but adds additional rule codes on top of the selected ones
      --per-file-ignores <PER_FILE_IGNORES>
          List of mappings from file pattern to code to exclude
      --fixable <RULE_CODE>
          List of rule codes to treat as eligible for autofix. Only applicable when autofix itself is enabled (e.g., via `--fix`)
      --unfixable <RULE_CODE>
          List of rule codes to treat as ineligible for autofix. Only applicable when autofix itself is enabled (e.g., via `--fix`)
```

(Notice that `--extend-select` exists, but not `--extend-fixable`.)

---

_@MichaReiser approved on 2023-05-10 07:23_

---

_Comment by @MichaReiser on 2023-05-10 07:26_

> I'm generally open to something like that, though I think in this case, it does make sense to add --extend-fixable, since it's an inconsistency in the API:

What is your suggestion to decide on when we should add new explicit options? Should we also add `extend-ignore`, `extend-per-file-ignores`, and `extend-unfixable`? That's why I think I would prefer to potentially decide on adding support for `--config` (and rename the existing `--config` option to `--config-path`?) because I fear that it will always be possible to reason that some option should be supported because we already support some other option. 


---

_Comment by @charliermarsh on 2023-05-19 02:20_

I think we _should_ add these for all of the rule selection APIs, but not for the rule _configuration_ APIs. That's my general stance right now.

---

_Merged by @charliermarsh on 2023-05-19 02:20_

---

_Closed by @charliermarsh on 2023-05-19 02:20_

---

_Branch deleted on 2023-05-19 02:20_

---
