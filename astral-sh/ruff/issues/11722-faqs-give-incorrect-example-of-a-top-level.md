```yaml
number: 11722
title: FAQs give incorrect example of a top-level settings 
type: issue
state: closed
author: keikoro
labels:
  - documentation
assignees: []
created_at: 2024-06-03T14:05:36Z
updated_at: 2024-06-10T20:33:15Z
url: https://github.com/astral-sh/ruff/issues/11722
synced_at: 2026-01-10T11:09:53Z
```

# FAQs give incorrect example of a top-level settings 

---

_Issue opened by @keikoro on 2024-06-03 14:05_

The FAQ section ["How does Ruff's import sorting compare to isort"](https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort) contains a `pyproject.toml` example config to demonstrate the `known-first-party` setting.

Included in this example config is the top-level `src` setting (+ comment noting it is a top-level setting), which is _not_ preceded by the correct/required heading, `[tool.ruff]`. Instead, it sits right below lint-specific settings, which makes the explanatory comment sound confusing and may mislead users with regard to where `src` can/needs to be set.

[Docs/FAQ source](https://github.com/astral-sh/ruff/blob/2b28889ca9d7935488875b5c944a159a2db20a23/docs/faq.md?plain=1#L276-L295)







---

_Label `documentation` added by @MichaReiser on 2024-06-03 14:48_

---

_Comment by @MichaReiser on 2024-06-03 14:48_

Thanks for raising this. Would you be interested in PRing a fix?

---

_Comment by @keikoro on 2024-06-03 15:03_

> Thanks for raising this. Would you be interested in PRing a fix?

Sure, can do! Probably not anymore today, but within the next several days.

---

_Comment by @keikoro on 2024-06-03 16:10_

@MichaReiser I just had another look at the FAQ and saw that in the next section, [How does Ruff determine which of my imports are first-party, third-party, etc.?](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc), `src` is actually explained in detail.

Is it possible the example for `known-first-party` was originally intended for that section? Or was the idea to provide a complete/working if minimal example for how to configure Ruff for isort?

My suggestion would be to move the (corrected) example away from where it's currently at and into the next FAQ section (minus the then redundant explanatory comment). If readers of the isort section should be made aware the next point is sort of a continuation of the wider topic, or that it includes a fuller example, I could briefly mention that.

---

_Comment by @keikoro on 2024-06-03 16:15_

Also, I believe the missing section header in the `pyproject.toml` example is a carry-over from the contents of the `ruff.toml` example (which doesn't need a top-level section) having been copied 1:1.

---

_Comment by @keikoro on 2024-06-10 15:56_

Ping, @MichaReiser. 

---

_Comment by @charliermarsh on 2024-06-10 20:26_

Thanks @keikoro -- I think that's the right diagnosis. Updated here to match your suggestion: https://github.com/astral-sh/ruff/pull/11829.

---

_Closed by @charliermarsh on 2024-06-10 20:33_

---
