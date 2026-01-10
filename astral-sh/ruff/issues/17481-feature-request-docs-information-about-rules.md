```yaml
number: 17481
title: "feature_request(docs): information about rules missing from original tools/plugins"
type: issue
state: closed
author: Kristinita
labels:
  - documentation
assignees: []
created_at: 2025-04-19T15:13:18Z
updated_at: 2025-04-22T07:53:57Z
url: https://github.com/astral-sh/ruff/issues/17481
synced_at: 2026-01-10T11:09:58Z
```

# feature_request(docs): information about rules missing from original tools/plugins

---

_Issue opened by @Kristinita on 2025-04-19 15:13_

### 1. Summary

It would be nice if Ruff documentation will contain information about rules missing from original tools/plugins. For example, it would be nice if Ruff will mark these rules.

### 2. Example of desired behavior

For example, at the moment of writing this issue Flake8 plugin [**flake8-use-pathlib has 24 `PL1XX` rules**](https://gitlab.com/RoPP/flake8-use-pathlib#rules). Ruff plugin flake8-use-pathlib [**has 10 additional rules `PL2XX`**](https://docs.astral.sh/ruff/rules/#flake8-use-pathlib-pth). It would be nice if Ruff documentation of the flake8-use-pathlib will look like this:

![Ruff additional rules example](https://i.imgur.com/jEPkCo3.png)

### 3. Justification of the need of the feature

Ruff users may not want to use some plugins because they use original plugins and don’t want to have duplicate settings and warnings/errors. It would be nice if users can quickly check does Ruff plugin have additional rules to decide whether to use it.

Thanks.


---

_Label `documentation` added by @ntBre on 2025-04-21 17:19_

---

_Closed by @MichaReiser on 2025-04-22 07:38_

---

_Comment by @Kristinita on 2025-04-22 07:51_

**Type: Oppose** 🙅

@MichaReiser, my issue isn’t a duplicate of **#13897**.

**#13897** requests “unsupported” rules — rules that original linters have, but Ruff doesn’t.

**#17481** requests “additional” rules — rules that Ruff have, but original linters doesn’t.

Thanks.


---

_Comment by @MichaReiser on 2025-04-22 07:53_

I see. Thanks for bringing this to my attention. I'll leave a comment in #13897 to consider this as well. 

Overall, it's unlikely that we'll implement either because we want to recategorize our rules long-term and decouple them from the upstream rules.

---
