```yaml
number: 3308
title: "Is there a `noqa` equivalent for do not fix"
type: issue
state: closed
author: paddyroddy
labels:
  - question
assignees: []
created_at: 2023-03-02T14:50:40Z
updated_at: 2023-03-02T22:19:22Z
url: https://github.com/astral-sh/ruff/issues/3308
synced_at: 2026-01-12T15:54:43Z
```

# Is there a `noqa` equivalent for do not fix

---

_@paddyroddy_

Unfortunately for `pydantic` you can't use the new union syntax `X | Y` for models and so you have to change it to i.e. `Union[X, Y]` or `Optional[X]` (depending on use case) if using `python` < `3.10` pydantic/pydantic#2609.

For now I've got around it by doing, i.e.
```toml
[tool.ruff]
fix = true
select = [
    "UP",
]
per-file-ignores = {"btrack/models.py" = [
    "UP006",
    "UP007",
], "btrack/config.py" = [
    "UP006",
    "UP007",
]}
```

However, this would mean if there are cases of `X | Y` or `list[Z]` for example outside of `pydantic` then they won't be fixed. Is there a way of doing, i.e. `# nofix: UP006,UP007`?

---

_Comment by @calumy on 2023-03-02 14:53_

Setting a target version below 3.10 might be what you are looking for: https://beta.ruff.rs/docs/settings/#target-version.

---

_Comment by @paddyroddy on 2023-03-02 14:58_

But we are also supporting `3.10`, i.e. `3.8<= x <= 3.10` at the moment.

---

_Comment by @charliermarsh on 2023-03-02 14:58_

@paddyroddy - Hmm, you can turn off autofix for select rules like:

```py
unfixable = [
  "UP006",
  "UP007",
]
```

But that won't activate on a per-file or per-case basis -- it'll tell Ruff to still flag those errors, but not fix them.


---

_Comment by @paddyroddy on 2023-03-02 15:00_

I guess `target-version` may be the easiest but not the most satisfying IMO as another thing to remember when, for example, dropping `3.8` support. But thank you.

---

_Comment by @paddyroddy on 2023-03-02 15:08_

I'm not sure `target-version` works this way? I changed it to the following, but my code is still full of, e.g. `|` typings.
```
[tool.ruff]
fix = true
select = [
    "UP",
]
target-version = "py38"
```

---

_Comment by @paddyroddy on 2023-03-02 15:19_

Seems like `target-version` may have worked, but the problem is I've already upgraded without that flag. So the solution doesn't work well for me, as I'd have to manually downgrade throughout.

---

_Comment by @charliermarsh on 2023-03-02 15:21_

Can you revert your change, then re-run Ruff? Ruff won't actively "downgrade" the code if those changes have already been applied.

---

_Comment by @paddyroddy on 2023-03-02 15:25_

Yes but I've made a fair few manual changes since so a bit hesitant

---

_Comment by @charliermarsh on 2023-03-02 22:06_

What's the advantage of `# nofix: UP006,UP007` over `# noqa: UP006,UP007`? Would the former _not_ ignore the violation?

---

_Label `question` added by @charliermarsh on 2023-03-02 22:06_

---

_Comment by @paddyroddy on 2023-03-02 22:15_

Not sure what happened there. `noqa` actually does the right thing - i.e. to not format it. I'm sure when playing around, it was doing something else, or I messed up.

---

_Closed by @paddyroddy on 2023-03-02 22:15_

---

_Comment by @charliermarsh on 2023-03-02 22:19_

Oh no prob! Maybe it expected it on a different line than you tried initially or something.

---
