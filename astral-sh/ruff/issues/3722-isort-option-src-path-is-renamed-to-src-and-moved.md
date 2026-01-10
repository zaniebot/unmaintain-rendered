```yaml
number: 3722
title: "Isort option `src_path` is renamed to `src` and moved to tool.ruff section"
type: issue
state: closed
author: Cjkjvfnby
labels: []
assignees: []
created_at: 2023-03-24T20:45:58Z
updated_at: 2023-03-25T16:18:35Z
url: https://github.com/astral-sh/ruff/issues/3722
synced_at: 2026-01-10T11:09:46Z
```

# Isort option `src_path` is renamed to `src` and moved to tool.ruff section

---

_Issue opened by @Cjkjvfnby on 2023-03-24 20:45_

Isort option `src_path` is renamed to `src` and moved to `[tool.ruff]` section.  It took me some time to find it. 

It would be great to mention it in the corresponding documentation section https://beta.ruff.rs/docs/settings/#isort docs

Maybe do a little trick, and add this option to the list of options in `[tool.ruff.isort]` but when it is set it will raise a deprecation warning, with the text like: `use [tool.ruff] src=`. 

Although it bit hack, for me this way would be the easiest to find the right place to check. 
 I just renamed the `[tools.isort]` to `[tools.ruff.isort]` in the pyproject.toml and just fix erroneous entries, choosing things from the list.

```
unknown field `src`, expected one of `force-wrap-aliases`, `force-single-line`, `single-line-exclusions`, `combine-as-imports`, `split-on-trailing-comma`, `order-by-type`, `force-sort-within-sections`, `force-to-top`, `known-first-party`, `known-third-party`, `known-local-folder`, `extra-standard-libra
ry`, `relative-imports-order`, `required-imports`, `classes`, `constants`, `variables`, `no-lines-before`, `lines-after-imports`, `lines-between-types`, `forced-separate`
```

---

_Comment by @charliermarsh on 2023-03-24 21:02_

Ah yeah, it's because `src` is used for more than just `isort`, so it sits as a top-level setting rather than an `isort`-specific setting. I'll try to find a way to incorporate it into the docs.

---

_Closed by @charliermarsh on 2023-03-25 16:18_

---
