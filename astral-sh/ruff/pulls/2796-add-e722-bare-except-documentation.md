```yaml
number: 2796
title: Add E722 bare-except documentation
type: pull_request
state: merged
author: Zeddicus414
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-E722-docs
created_at: 2023-02-12T03:53:58Z
updated_at: 2023-02-12T16:51:33Z
url: https://github.com/astral-sh/ruff/pull/2796
synced_at: 2026-01-12T04:52:01Z
```

# Add E722 bare-except documentation

---

_Pull request opened by @Zeddicus414 on 2023-02-12 03:53_

https://github.com/charliermarsh/ruff/issues/2646

---

_@not-my-profile reviewed on 2023-02-12 04:28_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:10 on 2023-02-12 04:28_

Doc comments should be indented with a space ... that is they should start with `/// `.

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:11 on 2023-02-12 04:29_

nit: I don't think `except` is a statement.

---

_@not-my-profile reviewed on 2023-02-12 04:29_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 04:30_

please wrap the text (the `ruff rule` command directly prints it)

---

_@not-my-profile reviewed on 2023-02-12 04:30_

---

_@not-my-profile reviewed on 2023-02-12 04:34_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 04:34_

> `BaseExceptions`

I think the plural s should be outside of the code tag.

---

_@Zeddicus414 reviewed on 2023-02-12 04:42_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 04:42_

> please wrap the text (the `ruff rule` command directly prints it)

I'm not sure what you are asking for here.

---

_@not-my-profile reviewed on 2023-02-12 04:49_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 04:49_

I meant that the line is very long and can be wrapped by inserting some line breaks. I don't think we want it to be longer than 80 chars.

---

_@Zeddicus414 reviewed on 2023-02-12 04:59_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:10 on 2023-02-12 04:59_

fixed.

---

_@Zeddicus414 reviewed on 2023-02-12 05:00_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:11 on 2023-02-12 05:00_

Good call. Tried to clarify.

---

_@Zeddicus414 reviewed on 2023-02-12 05:01_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 05:01_

> > `BaseExceptions`
> 
> I think the plural s should be outside of the code tag.
 
 Fixed this.


---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 05:07_

> the line is very long and can be wrapped by inserting some lin

Ahh, thanks. Are we okay with the links and code lines being longer than 80 characters?

---

_@Zeddicus414 reviewed on 2023-02-12 05:07_

---

_@not-my-profile reviewed on 2023-02-12 05:20_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 05:20_

Yeah links being longer is fine. I think it would be nice if the code would fit in 80 chars but it's less important.

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 05:22_

Sounds good. I'll try to keep that in mind next time. I've got a ruler in VScode reminding me now =)

---

_@Zeddicus414 reviewed on 2023-02-12 05:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/bare_except.rs`:14 on 2023-02-12 05:24_

Would love if we could automatically enforce the wrapping somehow for documentation specifically :sob:

---

_@charliermarsh reviewed on 2023-02-12 05:24_

---

_Label `documentation` added by @charliermarsh on 2023-02-12 16:48_

---

_Merged by @charliermarsh on 2023-02-12 16:51_

---

_Closed by @charliermarsh on 2023-02-12 16:51_

---
