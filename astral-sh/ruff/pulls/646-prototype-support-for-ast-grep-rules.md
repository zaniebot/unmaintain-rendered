```yaml
number: 646
title: Prototype support for ast-grep rules
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/ast-grep
created_at: 2022-11-07T19:51:45Z
updated_at: 2022-11-22T18:08:58Z
url: https://github.com/astral-sh/ruff/pull/646
synced_at: 2026-01-12T05:48:45Z
```

# Prototype support for ast-grep rules

---

_Pull request opened by @charliermarsh on 2022-11-07 19:51_

Going to use this branch to explore an ast-grep integration (i.e., allow users to define custom rules in YAML that can be executed via `ast-grep`, then automatically pick them up in Ruff).

See: #516.


---

_Comment by @charliermarsh on 2022-11-07 19:53_

One thing to note is that this basic rule takes ~1.8s, and it seems to scale ~linearly with the number of rules (very crude analysis so may well be wrong). So, there would be a conscious perf hit here vs. first-party rules.


---

_Comment by @charliermarsh on 2022-11-07 19:55_

This rule takes about ~650ms:

```yaml
id: C001
message: Don't import bbbbbbbb
severity: warning
language: Python
rule:
  any:
    - pattern: import bbbbbbbb
```

So it does depend on the number and complexity of patterns _and_ the number of occurrences.


---

_Comment by @squiddy on 2022-11-10 07:47_

I like the idea. Having an accessible way of writing custom lints was the reason I introduced semgrep at work, next to flake8.

The performance impact is quite huge though. If this were to be introduced, a flag to measure/report rule execution time would be helpful. I just looked at our repository at work, and running ruff cold took 200ms. flake8 needed 10s. If adding even just one of these rules can take up to 2s, it would be good to know, otherwise we're quickly in the ballpark of flake8 again. :) 

---

_Comment by @charliermarsh on 2022-11-10 13:37_

@squiddy - Yeah I feel similarly on perf. It's the same reason that I didn't base the entire linter on LibCST. I need to think on what the right approach is here. It's possible that we will want to end up with code-based plugins.


---

_Closed by @charliermarsh on 2022-11-22 18:08_

---
