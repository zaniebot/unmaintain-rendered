```yaml
number: 3253
title: "Feature request: autofix specified errors only"
type: issue
state: closed
author: thisfred
labels: []
assignees: []
created_at: 2023-02-27T17:00:10Z
updated_at: 2023-02-27T17:08:03Z
url: https://github.com/astral-sh/ruff/issues/3253
synced_at: 2026-01-12T15:54:43Z
```

# Feature request: autofix specified errors only

---

_@thisfred_

There may be a way to do this already, but if so, I haven't found it.

What I would like to be able to do is something like `ruff --fix errors=Exxx,Exxx,Wxxx [files]` where only the specified issues are autofixed.

This would be helfpul for instance in pre-commit hooks, where there are very safe autofixes (like the isort ones) that we just want to apply and recommit, and then there are others that we want folks to manually inspect before committing, because they aren't necessarily always safe or appropriate.

I realize we could disallow all of the other autofixes in pyproject.toml, but even if we want to manually inspect the results, it's still a time saver to not have to manually make the changes.

---

_Comment by @charliermarsh on 2023-02-27 17:06_

We support this via the [`fixable`](https://beta.ruff.rs/docs/settings/#fixable) and [`unfixable`](https://beta.ruff.rs/docs/settings/#unfixable) settings, which work in `pyproject.toml` or on the command-line. By default, every rule that has an autofix implementation is considered fixable, but you should be able to do things like:

```
ruff --fixable E [files]  # Only autofix E rules
ruff --unfixable B [files]  # Avoid autofixing B rules
```

(Let me know if this isn't quite what you're asking and I can re-open!)

---

_Closed by @charliermarsh on 2023-02-27 17:06_

---

_Comment by @thisfred on 2023-02-27 17:08_

Oh thank you so much, I didn't know the command line option existed.

---
