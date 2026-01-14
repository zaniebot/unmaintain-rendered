```yaml
number: 22572
title: "feat: added isort options for completeness (WIP)"
type: pull_request
state: open
author: tonycoco
labels:
  - isort
  - configuration
assignees: []
base: main
head: isort-options
created_at: 2026-01-14T15:34:41Z
updated_at: 2026-01-14T20:43:04Z
url: https://github.com/astral-sh/ruff/pull/22572
synced_at: 2026-01-14T21:43:07Z
```

# feat: added isort options for completeness (WIP)

---

_@tonycoco_

WIP to add completeness for isort options like:

```toml
[tool.ruff.lint.isort]
lexicographical = true
line-length = 1000
group-by-package = true
force-single-line = true
force-sort-within-sections = true
single-line-exclusions = [
  "collections.abc",
  "six.moves",
  "typing",
  "typing-extensions"
]
order-by-type = false
```


---

_Renamed from "feat: added isort options for completeness" to "feat: added isort options for completeness (WIP)" by @tonycoco on 2026-01-14 15:35_

---

_Comment by @ntBre on 2026-01-14 15:47_

Hi, thanks for working on this! I know it's still a work in progress, but I wanted to check if there had been some previous discussion of this in an issue or elsewhere? My understanding is that we mostly intentionally don't implement all of the same configuration options as isort, so I'm not sure we will want to add several new options, especially all at once.

---

_Label `isort` added by @ntBre on 2026-01-14 15:48_

---

_Label `configuration` added by @ntBre on 2026-01-14 15:48_

---

_Comment by @tonycoco on 2026-01-14 19:47_

@ntBre talked about in reference to issue #13389. Google's styleguide requires some of these options and I just wanted to sketch out a commit so I can further Google's open-source projects to use the ruff formatter/linter. Is there any reason that these options wouldn't be included? The overhead is extremely minimal to support them fully as the committed changes are, in my opinion, very small.

---

_Comment by @ntBre on 2026-01-14 20:23_

> @ntBre talked about in reference to issue #13389. Google's styleguide requires some of these options and I just wanted to sketch out a commit so I can further Google's open-source projects to use the ruff formatter/linter. 

Ah I see, thanks!

> Is there any reason that these options wouldn't be included? The overhead is extremely minimal to support them fully as the committed changes are, in my opinion, very small.

There are a couple of general reasons mentioned in #13389 and its parent issue, #6190, related to the maintenance burden and potential conflicts with every new option. 

I think the `line-length` setting here could be problematic, in particular, because it could conflict with our existing top-level [line-length](https://docs.astral.sh/ruff/settings/#line-length) setting. I'm not sure about the other two settings yet. Maybe @amyreese would have some insight.

---

_Comment by @MichaReiser on 2026-01-14 20:43_

> I think the line-length setting here could be problematic, in particular, because it could conflict with our existing top-level [line-length](https://docs.astral.sh/ruff/settings/#line-length) setting. I'm not sure about the other two settings yet. Maybe @amyreese would have some insight.

Agree, I don't think we should add another `line-length` setting. It's also not clear to me why that would be necessary


`group-by-package` is probably fine. Although I'm not sure how it interacts with all other settings. 


`lexicographical` also seems mostly fine


---
