```yaml
number: 2906
title: Implement shell autocompletion for rule codes
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: rule-code-autocompletion
created_at: 2023-02-14T23:37:39Z
updated_at: 2023-02-15T13:09:54Z
url: https://github.com/astral-sh/ruff/pull/2906
synced_at: 2026-01-12T04:52:01Z
```

# Implement shell autocompletion for rule codes

---

_Pull request opened by @not-my-profile on 2023-02-14 23:37_

For example:

    $ ruff check --select=EM<Tab>
    EM          -- flake8-errmsg
    EM10   EM1  --
    EM101       -- raw-string-in-exception
    EM102       -- f-string-in-exception
    EM103       -- dot-format-in-exception

(You will need to enable autocompletion as described
in the Autocompletion section in the README.)

Fixes #2808.

---

_Comment by @charliermarsh on 2023-02-14 23:39_

Wow it includes the human-readable name? That is so nice.

---

_@not-my-profile reviewed on 2023-02-14 23:42_

---

_Review comment by @not-my-profile on `crates/ruff/src/rule_selector.rs`:237 on 2023-02-14 23:42_

I put it in a submodule so that we can easily feature gate it in the future (since we'll probably want to turn `clap` into an optional dependency for the `ruff` library).

---

_Comment by @not-my-profile on 2023-02-14 23:43_

Yeah I agree ... it's pretty neat.

---

_@not-my-profile reviewed on 2023-02-14 23:50_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/args.rs`:113 on 2023-02-14 23:50_

Added `hide_possible_values` because we don't want `ruff help check` to list all 4,000 codes.^^

---

_Review comment by @charliermarsh on `README.md`:471 on 2023-02-15 00:26_

I assume this is related to `hide_possible_values`, but I'm trying to wrap my head around what it's saying, when it would show up, and what would change by adding `--help` -- isn't this message printed when the user _runs_ `--help`?

---

_@charliermarsh reviewed on 2023-02-15 00:26_

---

_Review comment by @not-my-profile on `README.md`:471 on 2023-02-15 05:40_

Oh yeah ... this also confused me quite a bit. As it turns out this is a Clap bug. I submitted a fix to upstream with https://github.com/clap-rs/clap/pull/4710.

---

_@not-my-profile reviewed on 2023-02-15 05:40_

---

_Merged by @charliermarsh on 2023-02-15 13:09_

---

_Closed by @charliermarsh on 2023-02-15 13:09_

---

_@charliermarsh reviewed on 2023-02-15 13:09_

---

_Review comment by @charliermarsh on `README.md`:471 on 2023-02-15 13:09_

Thanks for doing that!

---
