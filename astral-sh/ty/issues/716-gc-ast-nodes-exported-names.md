```yaml
number: 716
title: "GC AST nodes & `exported_names`"
type: issue
state: open
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-06-27T15:30:52Z
updated_at: 2025-06-30T07:34:36Z
url: https://github.com/astral-sh/ty/issues/716
synced_at: 2026-01-12T15:54:23Z
```

# GC AST nodes & `exported_names`

---

_@MichaReiser_

The `exported_names(file)` query is only called when another module imports `file` with a star import. Otherwise, the query never runs. 

This introduces a problem with how we garbage collected AST nodes because the basic assumption is that it's fine to collect AST nodes when `check_file` completes because all queries for that file are now "pre-warmed". 

There are a few options here:

1. We accept that we might need to re-parse because it isn't that common
2. We use salsa's specify feature (or something else) to set the `exported_names` result after building the semantic index (and deriving the information from the semantic index). 
3. We always call `exported_names` as part of `check_files` (that's probably undecired because it is unnecessary in most cases
4. ... something else?

The downside of setting the `exported_names` result for each module is that we have to store extra information for many modules that never get star-imported, which seems wasteful. 





---

_Label `performance` added by @MichaReiser on 2025-06-27 15:30_

---

_Comment by @AlexWaygood on 2025-06-28 13:43_

I think there might be a similar issue with the `dunder_all_names` query, which is also called by functions that are called cross-module, possibly after type checking of the external module has been completed

---

_Comment by @AlexWaygood on 2025-06-28 14:17_

Is there a way of asking Salsa whether a query has already been run and had its result cached? Maybe `exported_names()`, when called, could check whether semantic indexing for the file has already been completed. If it has, `exported_names` could just obtain the necessary information from the semantic index if so; if not, `exported_names` would fall back to an AST walk of the global scope, like it does now.

---

_Comment by @MichaReiser on 2025-06-28 14:20_

No, this isn't currently possible. The closest is using `specify` where one query can "set" the result of another query. The downside is that it's less lazy than what you're suggesting. That doesn't mean that we can't extend salsa to possibly support this

---

_Comment by @carljm on 2025-06-28 15:50_

My proposal on this issue is that we don't prioritize it until/unless we see a practical problem on real codebases.

I think even without `exported_names` and `dunder_all_names`, that our current GC AST strategy is not foolproof, and there is nothing in principle preventing re-parsing -- it should be possible if a class is defined and one of its attributes is never accessed in the current module but is accessed in another module.

---

_Comment by @sharkdp on 2025-06-30 07:34_

> My proposal on this issue is that we don't prioritize it until/unless we see a practical problem on real codebases.

When I looked at the traces of a dd-trace ecosystem run of ty recently, I noticed that 300/2000 = 15% of files were being reparsed. That may only be due to diagnostic printing, but could probably also refer to the issue here(?).

---
