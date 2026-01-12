```yaml
number: 2197
title: "[`flake8-bandit`] Add Rule S110 (try/except/pass)"
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: s110
created_at: 2023-01-26T16:22:47Z
updated_at: 2023-11-22T20:21:27Z
url: https://github.com/astral-sh/ruff/pull/2197
synced_at: 2026-01-12T15:55:07Z
```

# [`flake8-bandit`] Add Rule S110 (try/except/pass)

---

_@sciyoshi_

Check for [flake8-bandit B110: blank `pass` in exception handler](https://bandit.readthedocs.io/en/latest/plugins/b110_try_except_pass.html). Just getting my feet wet with implementing checkers in Ruff, so let me know if anything is missing. I used the `add_rule.py` script which has a slightly different convention than the other flake8-bandit checkers.

ref: #1646

---

_Comment by @edgarrmondragon on 2023-01-26 17:12_

I think there's some overlap between this rule and `SIM105`: `Use contextlib.suppress({exception}) instead of try-except-pass`.


---

_Comment by @andersk on 2023-01-26 22:20_

SIM105 is intentionally [restricted](https://github.com/charliermarsh/ruff/pull/1948) because `contextlib.suppress` is confusing in all but the simplest cases.

---

_@charliermarsh reviewed on 2023-01-27 00:51_

---

_Review comment by @charliermarsh on `src/rules/flake8_bandit/settings.rs`:42 on 2023-01-27 00:51_

This looks like it might be incorrect for this option. Maybe copied over from elsewhere?

---

_@charliermarsh reviewed on 2023-01-27 00:52_

---

_Review comment by @charliermarsh on `src/rules/flake8_bandit/rules/try_except_pass.rs`:31 on 2023-01-27 00:52_

We can do a bit better here. Try: `checker.resolve_call_path(&type).map_or(false, |call_path| call_path.as_slice() == ["", "Exception"])`.

That will verify that `&type` is a reference to the built-in `Exception` (so it also does the right thing if `Exception` gets shadowed and assigned to some other variable -- which is weird, but correct).


---

_@charliermarsh reviewed on 2023-01-27 00:54_

---

_Review comment by @charliermarsh on `src/rules/flake8_bandit/rules/try_except_pass.rs`:16 on 2023-01-27 00:54_

I know this is exactly what Bandit uses, but our conventions would probably be more like:

```
format!("`try`-`except`-`pass` detected")
```

Or commas in lieu of dashes. But wrapping except keyword in backticks, and removing the period.

(Sorry for the nit!)

---

_@sciyoshi reviewed on 2023-01-27 01:01_

---

_Review comment by @sciyoshi on `src/rules/flake8_bandit/rules/try_except_pass.rs`:16 on 2023-01-27 01:01_

Thanks - fixed 👍 

---

_@sciyoshi reviewed on 2023-01-27 01:02_

---

_Review comment by @sciyoshi on `src/rules/flake8_bandit/rules/try_except_pass.rs`:31 on 2023-01-27 01:02_

Fixed in my latest commit, although I didn't add a test for the case of `Exception` being shadowed.

---

_Comment by @sciyoshi on 2023-01-27 01:16_

@andersk's link doesn't seem to resolve so I'm not sure what SIM105 being restricted means in this context, but the point about overlapping rules is a good one - I'm not sure what the best approach would be for that.

(Also - it's a small world, I remember Anders from my USAMO days!)

---

_Comment by @andersk on 2023-01-27 01:31_

(Oops, fixed the link.)

---

_Merged by @charliermarsh on 2023-01-27 23:52_

---

_Closed by @charliermarsh on 2023-01-27 23:52_

---

_Comment by @charliermarsh on 2023-01-27 23:53_

Thank you!

---

_Branch deleted on 2023-11-22 20:21_

---
