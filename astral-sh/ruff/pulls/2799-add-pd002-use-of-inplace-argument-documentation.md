```yaml
number: 2799
title: Add PD002 use-of-inplace-argument documentation
type: pull_request
state: merged
author: Zeddicus414
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-PD002-docs
created_at: 2023-02-12T04:20:34Z
updated_at: 2023-02-12T18:10:35Z
url: https://github.com/astral-sh/ruff/pull/2799
synced_at: 2026-01-12T04:52:01Z
```

# Add PD002 use-of-inplace-argument documentation

---

_Pull request opened by @Zeddicus414 on 2023-02-12 04:20_

https://github.com/charliermarsh/ruff/issues/2646

---

_@not-my-profile reviewed on 2023-02-12 04:26_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 04:26_

nit: "`pandas` code" could be misunderstood to refer to the code of the `pandas` library.

---

_@not-my-profile reviewed on 2023-02-12 04:27_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 04:27_

I think this section should mention which methods are checked for this parameter.

---

_@Zeddicus414 reviewed on 2023-02-12 04:37_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 04:37_

> I think this section should mention which methods are checked for this parameter.

I'm not sure how to do this exactly. I think it's a **bunch** of methods.

---

_@not-my-profile reviewed on 2023-02-12 04:43_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 04:43_

Ah ok ... in that case I think it's fine to just list a few of these methods as examples and say that there are more. Ideally the mentions of the example methods would be linked to their pandas documentation.

---

_@not-my-profile reviewed on 2023-02-12 04:47_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:15 on 2023-02-12 04:47_

I think the claim "Many people expect" should be removed ... we don't know what people expect, I think just saying that it often doesn't yield performance benefits should suffice. It would be interesting to note why it often doesn't improve performance.

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:17 on 2023-02-12 04:48_

nit: we may want to explicitly mention `method chaining` instead of just `chaining`

---

_@not-my-profile reviewed on 2023-02-12 04:48_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:17 on 2023-02-12 05:12_

done

---

_@Zeddicus414 reviewed on 2023-02-12 05:13_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:15 on 2023-02-12 05:13_

Removed the claim.

---

_@Zeddicus414 reviewed on 2023-02-12 05:13_

---

_@Zeddicus414 reviewed on 2023-02-12 05:21_

---

_Review comment by @Zeddicus414 on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 05:21_

I think I've addressed everything here except maybe for more detail about exactly which pandas methods this applies to and links to them maybe? I'm not sure how to do that in kind of a "principled" way, other than just linking to a few random pandas methods that happen to have this option. 

If you are okay with it, I'm inclined to leave it as is and see if anyone else has a clear path to making it clearer. I think this is considerably better than no documentation.

---

_@not-my-profile reviewed on 2023-02-12 05:39_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:20 on 2023-02-12 05:39_

I should have clarified ... I care in particular about the `ruff rule` output not being longer than 80 chars.

Since the `    /// ` is stripped from that the actual lines here can in fact be 88 chars long, so I think `code.` can be still on the previous line.

---

_@not-my-profile reviewed on 2023-02-12 05:40_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:16 on 2023-02-12 05:40_

I think Markdown lists look a bit better if the wrapped text is indented as much as the first line of the list item.

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:15 on 2023-02-12 05:42_

nit: In the other rule documentations we have been using `*` as a list marker rather than `-` ... it would be nice to be consistent.

---

_@not-my-profile reviewed on 2023-02-12 05:42_

---

_@not-my-profile reviewed on 2023-02-12 05:48_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:12 on 2023-02-12 05:48_

Fair enough linking several would indeed be quite random but I think linking just one as an example would be nice so that you can see how pandas documents the `inplace` parameter ... since the pandas documentation may change. How about the following?

```
Several methods from the pandas library accept an `inplace` parameter
(for example the [`pandas.DataFrame.replace`] method). This rule checks
for `inplace=True` in code using the `pandas` library.

// ... rest of documentation ...

[`pandas.DataFrame.replace`]: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.replace.html
```

---

_Label `documentation` added by @charliermarsh on 2023-02-12 16:54_

---

_Merged by @charliermarsh on 2023-02-12 18:10_

---

_Closed by @charliermarsh on 2023-02-12 18:10_

---
