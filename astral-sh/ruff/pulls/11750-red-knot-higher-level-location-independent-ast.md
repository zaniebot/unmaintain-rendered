```yaml
number: 11750
title: "red-knot: Higher level, location independent AST"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: red-knot-salsa
head: red-knot-hir
created_at: 2024-06-05T11:42:40Z
updated_at: 2024-08-12T07:53:09Z
url: https://github.com/astral-sh/ruff/pull/11750
synced_at: 2026-01-10T21:38:31Z
```

# red-knot: Higher level, location independent AST

---

_Pull request opened by @MichaReiser on 2024-06-05 11:42_

## Summary

This PR is mainly to explore a location independent, higher level AST for red knot. 

The motivation for a location independent higher level AST is to have sub-level invalidation granularity. 
What I mean by that is that e.g. whitespace only changes should not invalidate any type inference results
but they do because the AST stores location informations that change when adding or removing characters from a document. 

A higher level AST (HAST) also allow to avoid infering the types when making local changes in a function. 

Let's say we have 

```python
def add(x, y):
	x + y

def multiply(x, y):
	x * y

```

It shouldn't be necessary to do type inference for `add` when making changes to `multiply` because `add`'s body doesn't change nor does it depend on `multiply` in anyway (it doesn't call the function). 

An HAST enables function level garnulariy, but it doesn't come for free:

* It requires building an additional AST that is very familiar to the AST created by the parser with the exception that it doesn't include TextRange's and that we build it per "function" rather than building the entire AST for the entire module
* It requires rebuilding a lot of the infrastructure like visitors 
* We end up with two AST representations and need a way to map between the representations 
* The HAST, at least what I built so far, is awkward to use
* While the HAST optimizes the time it takes to reanalyze when making local changes, the cost for reanalyzing in the worst case remains unchanged (or is now higher because of the extra intermediate representations). For example, adding a new symbol to the module scope requires type-checking the entire file because each sub scope depends on the root scope. 
  That means, while the HIR helps to improve the average response time in the LSP, it doesn't help improving the time of all requests. This is not a reason why we should not do it. But maybe it's more beneficial to reduce the cost of running the entire analysis so that an HIR isn't needed.  



## Test Plan

<!-- How was it tested? -->


---

_Closed by @MichaReiser on 2024-06-11 13:38_

---

_Branch deleted on 2024-08-12 07:53_

---
