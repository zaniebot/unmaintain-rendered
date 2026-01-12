```yaml
number: 9018
title: Fix documentation snafu that recommended invalid settings
type: pull_request
state: merged
author: eli-schwartz
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc-error
created_at: 2023-12-06T02:16:11Z
updated_at: 2023-12-07T05:01:56Z
url: https://github.com/astral-sh/ruff/pull/9018
synced_at: 2026-01-12T15:55:27Z
```

# Fix documentation snafu that recommended invalid settings

---

_@eli-schwartz_

The include/exclude settings return a configuration parsing error when placed under the "lint" section. They have always been expected at the top level (no `.ruff.toml` section).

---

_Review comment by @MichaReiser on `docs/configuration.md`:397 on 2023-12-06 02:18_

Using `exclude` under `lint` is correct here. We only exclude `.ipynb` files from linting, but keep them as part of formatting (same as the example aboth).

---

_Review comment by @MichaReiser on `docs/configuration.md`:406 on 2023-12-06 02:19_

Same here. Using `lint` here is correct.

---

_@MichaReiser reviewed on 2023-12-06 02:20_

Thanks. Looks correct to me, except the example configuration for formatting notebooks but not linting them.

---

_Label `documentation` added by @charliermarsh on 2023-12-06 02:33_

---

_@eli-schwartz reviewed on 2023-12-06 02:35_

---

_Review comment by @eli-schwartz on `docs/configuration.md`:397 on 2023-12-06 02:35_

`include` fails even with the latest version, so the documentation is wrong and needs fixing.

`exclude` fails only if you happen to have ruff 0.1.0 (or earlier) installed, but I do not know what the backwards compatibility story is here. If it is okay to assume users have a very recent ruff installed, then using it here is fine.

My stake in this is as someone who doesn't use the formatter at all, but found this page as the first and most obvious place discussing how to configure the linter.

---

_@MichaReiser reviewed on 2023-12-06 02:49_

---

_Review comment by @MichaReiser on `docs/configuration.md`:397 on 2023-12-06 02:49_

We don't support versioned documentation yet.

> include fails even with the latest version, so the documentation is wrong and needs fixing.

But this line isn't using `include`? The changes above are okay. It's just this example that should be left unchanged.

---

_@eli-schwartz reviewed on 2023-12-06 02:52_

---

_Review comment by @eli-schwartz on `docs/configuration.md`:397 on 2023-12-06 02:52_

Sorry, I should rephrase that.

> `include` fails even with the latest version, so the documentation ***in general*** is wrong and needs fixing.
> 
> `exclude` fails only if you happen to have ruff 0.1.0 (or earlier) installed, but I do not know what the backwards compatibility story is here. If it is okay to assume users have a very recent ruff installed, then using it here ***on this specific line*** is fine.

---

_Comment by @eli-schwartz on 2023-12-06 13:04_

If you can confirm that you'd prefer the (unversioned) documentation to use syntax that requires a recent version of ruff, then I'll update the PR to do so.

---

_@dhruvmanila reviewed on 2023-12-06 16:21_

---

_Review comment by @dhruvmanila on `docs/configuration.md`:397 on 2023-12-06 16:21_

I'm sorry but I'm unable to follow your reasoning in changing this specific example.

The example is correct here, I just tested it out on `v0.1.7`. To give some context on this specific example, it's to allow users to include notebook files for formatting but exclude them from linting. We don't support versioned documentation so the examples should always reflect as per the latest version.

The reason it fails on `v0.1.0` or earlier is because the `exclude` key didn't exist in those versions.

---

_Comment by @MichaReiser on 2023-12-07 02:19_

> If you can confirm that you'd prefer the (unversioned) documentation to use syntax that requires a recent version of ruff, then I'll update the PR to do so.

Yes, our documentation assumes a recent version. We would otherwise not be able to document new functionality. We should start versioning our documentation but haven't gotten around to do it yet.

---

_@eli-schwartz reviewed on 2023-12-07 02:59_

---

_Review comment by @eli-schwartz on `docs/configuration.md`:397 on 2023-12-07 02:59_

Pretty sure I already explained it, so I'm not sure why you're circling back around to the beginning.

---

_@MichaReiser approved on 2023-12-07 04:59_

Thank you for flagging and fixing the incorrect documentation. 

---

_Merged by @MichaReiser on 2023-12-07 05:01_

---

_Closed by @MichaReiser on 2023-12-07 05:01_

---
