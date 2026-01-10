```yaml
number: 22457
title: "[ty] Fix goto definition for relative imports in third-party files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/remove-default-database
created_at: 2026-01-08T12:58:57Z
updated_at: 2026-01-09T08:30:33Z
url: https://github.com/astral-sh/ruff/pull/22457
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Fix goto definition for relative imports in third-party files

---

_Pull request opened by @MichaReiser on 2026-01-08 12:58_

## Summary

This PR removes our default database handling from ty's LSP. 

We used the default database for files that don't belong to any project, 
because we want to treat them differently:

* We shouldn't show any diagnsotics (funnily enough, this doesn't work today)
* In the future, we want to disable formatting 

This PR removes the default database and:

* relies on the project inclusion/exlusion to disable diagnostics for non-project files, the same as we do it in the CLI 
* We can disable formatting and other features that should only run on project files the very same way


This fixes an issue where goto definition didn't work in files in the project's virtual environment
for relative imports because the default database uses different search paths than the project.


The only downside that I see using this approach is that ty doesn't warn
about missing imports in non-project files if those modules are available in the project's virtual environment. 
If this is a concern, than I think we'll want to use a similar approach to scripts (TBD). Either way, I don't think
we want to use a default database.

Fixes https://github.com/astral-sh/ty/issues/2290

## Test Plan

Tested that goto definition now works for projects using `venv` as their virtual environment (the example in https://github.com/astral-sh/ty/issues/2290)


---

_Label `server` added by @MichaReiser on 2026-01-08 12:58_

---

_Label `ty` added by @MichaReiser on 2026-01-08 12:58_

---

_Review requested from @carljm by @MichaReiser on 2026-01-08 12:58_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-08 12:58_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-08 12:58_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-08 12:58_

---

_Label `server` added by @MichaReiser on 2026-01-08 12:58_

---

_Label `ty` added by @MichaReiser on 2026-01-08 12:58_

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-08 12:59_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-08 12:59_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-08 12:59_

---

_Review requested from @dhruvmanila by @MichaReiser on 2026-01-08 12:59_

---

_Comment by @codspeed-hq[bot] on 2026-01-08 13:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **not alter performance**




### Summary

`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



---

<sub>Comparing <code>micha/remove-default-database</code> (511478e) with <code>main</code> (f9f7a69)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/micha%2Fremove-default-database?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Fremove-default-database?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-08 13:09_

Uhm what?

---

_Comment by @Gankra on 2026-01-08 15:42_

Uhh wait but we still have the default database for when config handling fails on init right..?

---

_Comment by @MichaReiser on 2026-01-08 15:52_

> Uhh wait but we still have the default database for when config handling fails on init right..?

That's different and, confusingly, we used the same term for them. We use a fallback db if the initialization of the project's database fails during workspace initialization. But we associate that db with the project's path. 

The default database was different. We only used it for paths that have no project with which they share a prefix. 

---

_@Gankra approved on 2026-01-08 17:33_

As long as we don't produce diagnostics for things-that-would-otherwise-be-default-database this seems relatively reasonable..?

---

_Comment by @Gankra on 2026-01-08 17:34_

Mostly this just leaves me feeling like "god we have to stop tiptoeing around actually handling multiple workspaces in a session".

---

_Comment by @MichaReiser on 2026-01-09 08:11_

> Mostly this just leaves me feeling like "god we have to stop tiptoeing around actually handling multiple workspaces in a session".

Yeah, I'm also getting more and more to the conclusion that not having to do that would be great

> As long as we don't produce diagnostics for things-that-would-otherwise-be-default-database this seems relatively reasonable..?


I'm not aware of a situationb where this would be the case. 

---

_Merged by @MichaReiser on 2026-01-09 08:30_

---

_Closed by @MichaReiser on 2026-01-09 08:30_

---

_Branch deleted on 2026-01-09 08:30_

---
