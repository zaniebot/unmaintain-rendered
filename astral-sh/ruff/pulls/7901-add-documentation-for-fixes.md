```yaml
number: 7901
title: Add documentation for fixes
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zanie/doc-fix
created_at: 2023-10-10T18:37:18Z
updated_at: 2023-10-11T16:48:13Z
url: https://github.com/astral-sh/ruff/pull/7901
synced_at: 2026-01-12T15:55:25Z
```

# Add documentation for fixes

---

_@zanieb_

Adds documentation for using `ruff check . --fix`

Uses the draft of the "Automatic fixes" section from https://github.com/astral-sh/ruff/pull/7732 and adds documentation for unsafe fixes, applicability levels, and https://github.com/astral-sh/ruff/pull/7841

I enabled admonitions because they're nice. We should use them more.

---

_Label `documentation` added by @zanieb on 2023-10-10 18:39_

---

_Comment by @zanieb on 2023-10-10 19:04_

Ah mdformat puts code fences on admonitions... will need to investigate. Maybe relevant https://github.com/cljdoc/cljdoc/issues/95

edit: resolved, there's a plugin for this at https://github.com/KyleKing/mdformat-admon which is recommended by the mdformat-mkdocs plugin

---

_Marked ready for review by @zanieb on 2023-10-10 19:50_

---

_Review comment by @charliermarsh on `docs/configuration.md`:416 on 2023-10-10 20:12_

Apparently this is unsettled grammar but (I'm really sorry) can you add a comma after the `e.g.`? We also stylize as `Pyflakes` per their README.

---

_Review comment by @charliermarsh on `docs/configuration.md`:394 on 2023-10-10 20:13_

Perhaps "will be retained by safe fixes"? We could also say that unsafe fixes _shouldn't_ change the meaning or intent in the majority of cases, but _could_. 

---

_@charliermarsh approved on 2023-10-10 20:13_

Looks great! Very clear.

---

_@zanieb reviewed on 2023-10-10 23:47_

---

_Review comment by @zanieb on `docs/configuration.md`:416 on 2023-10-10 23:47_

```suggestion
You may use prefixes to select rules as well, e.g., `F` can be used to promote fixes for all rules in Pyflakes to safe.
```


---

_Review comment by @zanieb on `docs/configuration.md`:394 on 2023-10-10 23:50_

```suggestion
Ruff labels fixes as "safe" and "unsafe". The meaning and intent of your code will be retained when applying safe fixes, but the meaning could be changed when applying unsafe fixes.
```

---

_@zanieb reviewed on 2023-10-10 23:50_

---

_Review comment by @zanieb on `docs/configuration.md`:394 on 2023-10-10 23:51_

I don't see an elegant way to insert the later part. Maybe to be included in an admonition later if we want.

---

_@zanieb reviewed on 2023-10-10 23:51_

---

_@dhruvmanila approved on 2023-10-11 04:41_

---

_Review comment by @konstin on `docs/configuration.md`:381 on 2023-10-11 12:38_

```suggestion
imports, reformat docstrings, rewrite type annotations to use the newer Python syntax, and more.
```

I wouldn't require users to know what PEPs do 

---

_Review comment by @konstin on `docs/configuration.md`:394 on 2023-10-11 12:39_

I think we should show an example here of what we consider safe and unsafe so users get a rough idea what "change meaning" entails

---

_@konstin approved on 2023-10-11 12:43_

---

_@zanieb reviewed on 2023-10-11 16:30_

---

_Review comment by @zanieb on `docs/configuration.md`:381 on 2023-10-11 16:30_

```suggestion
imports, reformat docstrings, rewrite type annotations to use newer Python syntax, and more.
```

---

_@zanieb reviewed on 2023-10-11 16:32_

---

_Review comment by @zanieb on `docs/configuration.md`:394 on 2023-10-11 16:32_

Good idea I was considering that but didn't know a good example off the top of my head. Maybe Charlie does? Let's address this in a follow-up.

---

_Merged by @zanieb on 2023-10-11 16:41_

---

_Closed by @zanieb on 2023-10-11 16:41_

---

_Branch deleted on 2023-10-11 16:41_

---

_Comment by @codspeed-hq[bot] on 2023-10-11 16:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/doc-fix)

### Merging #7901 will **improve performances by 9.59%**

<sub>Comparing <code>zanie/doc-fix</code> (cfd84f1) with <code>main</code> (090c1a4)</sub>



### Summary

`⚡ 4` improvements
`✅ 21` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/doc-fix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 2.2 ms | 2 ms | +9.59% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38.8 ms | 36.9 ms | +5.34% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 18.6 ms | 17.2 ms | +8.5% |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 6.3 ms | 6.1 ms | +4.59% |


---
