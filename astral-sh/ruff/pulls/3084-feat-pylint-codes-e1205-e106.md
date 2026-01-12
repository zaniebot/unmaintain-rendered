```yaml
number: 3084
title: "feat: pylint codes E1205-E106"
type: pull_request
state: merged
author: md384
labels:
  - rule
assignees: []
merged: true
base: main
head: m.pylint-E1205-E1206
created_at: 2023-02-21T05:02:06Z
updated_at: 2023-02-22T03:53:11Z
url: https://github.com/astral-sh/ruff/pull/3084
synced_at: 2026-01-12T04:39:44Z
```

# feat: pylint codes E1205-E106

---

_Pull request opened by @md384 on 2023-02-21 05:02_

Adds support for pylint [E1205](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/logging-too-many-args.html) and [E1206](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/logging-too-few-args.html) for https://github.com/charliermarsh/ruff/issues/970.

A couple of things that I think are wrong here (I am new to this project and to Rust)
- the test snapshots are almost empty. I expected them to be generated with more code.
- running ruff locally with `cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pylint/logging_E1205.py --no-cache --select PLE1205` shows no message but `cargo run -p ruff_cli -- check crates/ruff/resources/test/fixtures/pylint/logging_E1205.py --no-cache --select PLE` does - similarly with `PLE1206`. Have I set up the codes incorrectly?

For reference, https://github.com/PyCQA/pylint/blob/main/pylint/checkers/logging.py#L313-L367 is the pylint implementation of these rules.

---

_@md384 reviewed on 2023-02-21 05:02_

---

_Review comment by @md384 on `crates/ruff/src/rules/flake8_logging_format/rules.rs`:13 on 2023-02-21 05:02_

Perhaps this should be moved to a different file now

---

_@md384 reviewed on 2023-02-21 05:05_

---

_Review comment by @md384 on `crates/ruff/src/rules/pylint/rules/logging.rs`:113 on 2023-02-21 05:05_

This was taken from the pylint implementation but I think we could allow the presence of `exc_info`, `stack_info` and `extra`. Not sure if we're going to run into complications with that but doesn't seem like it.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:2878 on 2023-02-21 17:08_

I think one of these should be `LoggingTooManyArgs`.

---

_@charliermarsh reviewed on 2023-02-21 17:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/logging.rs`:63 on 2023-02-21 17:08_

I think this should be `LoggingTooManyArgs`.

---

_@charliermarsh reviewed on 2023-02-21 17:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/logging.rs`:72 on 2023-02-21 17:08_

I think this should be `LoggingTooFewArgs `.

---

_@charliermarsh reviewed on 2023-02-21 17:08_

---

_Comment by @charliermarsh on 2023-02-21 17:09_

Awesome! Hopefully my comments help with the fixture emptiness?

---

_Label `rule` added by @charliermarsh on 2023-02-21 17:09_

---

_@charliermarsh requested changes on 2023-02-21 19:51_

(Just "requesting changes" to help me manage my queue.)

---

_Comment by @md384 on 2023-02-21 20:58_

> Awesome! Hopefully my comments help with the fixture emptiness?

🤦 yep, I should have been paying more attention

---

_@md384 reviewed on 2023-02-21 21:02_

---

_Review comment by @md384 on `crates/ruff/src/rules/pylint/rules/logging.rs`:113 on 2023-02-21 21:02_

py3.8 added `stacklevel` too. Looking into this more, I still think this is doable (although perhaps in a new PR) since those 4 kwargs are the only allowed ones and AFAIK there is no support for named args in %-style log formatting.

---

_Review requested from @charliermarsh by @md384 on 2023-02-21 21:05_

---

_@md384 reviewed on 2023-02-21 21:10_

---

_Review comment by @md384 on `crates/ruff/src/rules/pylint/rules/logging.rs`:113 on 2023-02-21 21:10_

Perhaps for the moment, we can just ignore keywords all together (i.e. not even check them)

---

_@charliermarsh reviewed on 2023-02-21 23:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_logging_format/rules.rs`:13 on 2023-02-21 23:23_

Let's move this to a new `ast/logging.rs`? (`ast` is a bit of a misnomer, but I think it's right to be alongside moduiles like `ast/whitespace.rs`.)

---

_@charliermarsh reviewed on 2023-02-21 23:25_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pylint/logging_E1206.py`:1 on 2023-02-21 23:25_

Nit: can we name these to match the rule? I think that's the convention we've been using for Pylint (e.g., `logging_too_few_args.py`)

---

_@charliermarsh reviewed on 2023-02-21 23:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/logging.rs`:40 on 2023-02-21 23:26_

Could this be `.is_some()` rather than a match with an unused name?

---

_@charliermarsh reviewed on 2023-02-21 23:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/logging.rs`:19 on 2023-02-21 23:26_

Nit: should we wrap `logging` in backticks here? Pylint may not do that, but we tend to.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/logging.rs`:41 on 2023-02-21 23:27_

Are you open to writing documentation for these rules? We're trying to include documentation for all new rules. You can grep for `/// ## What it does` to see some examples of the expected format.

---

_@charliermarsh reviewed on 2023-02-21 23:27_

---

_Review comment by @md384 on `crates/ruff/src/rules/pylint/rules/logging.rs`:113 on 2023-02-22 01:05_

Going to leave this as is to match pylint

---

_@md384 reviewed on 2023-02-22 01:05_

---

_Comment by @charliermarsh on 2023-02-22 03:48_

Looking good! Thanks for turning this around.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-02-22 03:48_

---

_@charliermarsh approved on 2023-02-22 03:50_

---

_Merged by @charliermarsh on 2023-02-22 03:53_

---

_Closed by @charliermarsh on 2023-02-22 03:53_

---
