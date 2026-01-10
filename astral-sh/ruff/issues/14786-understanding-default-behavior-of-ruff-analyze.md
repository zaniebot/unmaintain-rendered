---
number: 14786
title: "Understanding default behavior of `ruff analyze graph` with different layouts"
type: issue
state: closed
author: purajit
labels:
  - question
  - analyze
assignees: []
created_at: 2024-12-05T10:05:11Z
updated_at: 2025-05-16T01:00:57Z
url: https://github.com/astral-sh/ruff/issues/14786
synced_at: 2026-01-10T01:22:55Z
---

# Understanding default behavior of `ruff analyze graph` with different layouts

---

_Issue opened by @purajit on 2024-12-05 10:05_

I'm working on a pretty anarchic codebase that I'm slowly wrestling into shape, but for now, the minimized structure is
```
my_project
├── pyproject.toml
└── src
    ├── __init__.py
    ├── foo.py
    └── ... other modules ...
```

and all imports are done via `PYTHONPATH=src` (aka, `from foo import ...`). We'e been running ruff 
for linting nearly since the start of the project with no issues, and once `analyze graph` was 
introduced I immediately started using it but couldn't get it to work; I finally got some time to look into it.

It works perfectly fine on the new src-layout uv projects I've been creating, but not on the main `src/`
codebase.

I created a minimal repro in [this repo](https://github.com/purajit/ruff-graph-layout-test) (see results of `ruff` [here](https://github.com/purajit/ruff-graph-layout-test/actions/runs/12177280068/job/33964766403)); it has a few different layouts.
* with the flat layout, notice how package-based imports from `flat_layout` are picked up in `a.py`, 
but not the `PYTHONPATH`-relative import in `c.py`
* with the semi-flat layout (aka, what I have), none of the imports are picked up
* with src-layout, everything works perfectly, as expected

From https://docs.astral.sh/ruff/settings/#src
> These defaults ensure that Ruff supports both flat layouts and src layouts out-of-the-box. (If a configuration file is explicitly provided (e.g., via the --config command-line flag), the current working directory will be considered the project root.)

### What I've tried

Hacking around, I was able to get it to work with this patch (that I understand is obviously incorrect):
```diff
git diff -U1
diff --git a/crates/ruff_graph/src/db.rs b/crates/ruff_graph/src/db.rs
index 39f74081b..ae9403e18 100644
--- a/crates/ruff_graph/src/db.rs
+++ b/crates/ruff_graph/src/db.rs
@@ -31,6 +31,6 @@ impl ModuleDb {
             // Use the first source root.
-            let src_root = src_roots
+            let mut src_root = src_roots
                 .next()
                 .ok_or_else(|| anyhow::anyhow!("No source roots provided"))?;
-
+            src_root.push("src");
             let mut search_paths = SearchPathSettings::new(src_root);
```

Also, I don't really understand this (I'm rather new to Rust) - but without the patch, the value of 
`src_roots.next()` doesn't contain the required trailing `src/`, but the value here:
https://github.com/astral-sh/ruff/blob/fda8b1f884c8598960b2e3f0c729e17ac62bfe35/crates/ruff/src/commands/analyze_graph.rs#L63-L68
does contain `src/` - every single value, so choosing `.next()` shouldn't matter - so I don't understand
where that's getting stripped out. If it weren't getting stripped out, this would work out-of-the-box.

---

_Comment by @MichaReiser on 2024-12-05 10:41_

Thanks for the nice reproduction and writeup! 

I'm not quiet sure what's happening. Maybe @charliermarsh has a better idea. I observe that `package_roots` always pick up the parent directory instead of the file containing the `project. tool`. I'd expect that the `source_root` always maps to the `lint.src` directory instead of just mapping to the package roots.

---

_Label `analyze` added by @MichaReiser on 2024-12-05 10:41_

---

_Comment by @purajit on 2024-12-25 20:13_

Ah yeah - I spent some time going through, and yeah, that's pretty much what's happening - instead
of just taking the parent here: https://github.com/astral-sh/ruff/blob/8b9c843c727de463efccafc7e012414b93458dc7/crates/ruff/src/commands/analyze_graph.rs#L62-L68
should it instead run through the list of `linter.src`s and check path prefixes, similar to what's done 
here for namespace packages here? https://github.com/astral-sh/ruff/blob/8b9c843c727de463efccafc7e012414b93458dc7/crates/ruff_linter/src/packaging.rs#L29-L32

Otherwise, it seems like flat packages are _explicitly_ not supported for `ruff analyze graph`.

I'm down to PR! Want to make sure this makes sense first.

---

_Comment by @MichaReiser on 2024-12-26 09:24_

@charliermarsh what do you think?

---

_Comment by @charliermarsh on 2024-12-26 14:49_

I haven't thought deeply about this, but isn't the import in `flat_layout/c.py` incorrect? Having the `__init__.py` in `semi_flat_src/src` also seems incorrect... The imports would then need to be `from src.b import B`, etc. I don't know if we should support these layouts, since they seem to be incorrectly structured?

---

_Comment by @purajit on 2024-12-29 03:04_

@charliermarsh yes it is, sadly, that's how our primary codebase is laid out, with imports being
done based on `PYTHONPATH=src`, and the application deployed with a plain source code copy (I 
promise I'm actively working on this mess, haha). I guess I was hoping `ruff` would/could(/should?)
be able to honor `PYTHONPATH`, but maybe that's a much larger discussion?

We can close this issue, since I understand the behavior now, having resolved some misunderstandings
I had and reading the code, but still curious about your thoughts.

---

_Label `question` added by @dhruvmanila on 2024-12-30 05:48_

---

_Comment by @purajit on 2024-12-31 19:43_

Ah ok, I think I was able to narrow down the pathological case we have, so I'm now able to 
boil down this issue to a minimal question: 

Should ruff's package discovery stop trying to detect packages once it discovers anything 
specified in the `src` config?

Essentially, because we deploy via code-copy and setting `PYTHONPATH=src`, meaning that
`src/pkg` is imported as `pkg` rather than `src.pkg`. However, we have a vestigial `src/__init__.py` 
(again, I apologize on behalf of the codebase), which caused ruff analyze's package discovery 
to still resolve everything to `src` rather than the individual modules inside it, and preventing 
any imports from resolving. Deleting the `src/__init__.py` fixes the issue.

I'm totally fine if the answer is "no, make sure your Python packaging and organization is 
correctly set up", but just thought I'd follow up to resolve the confusion.

---

_Comment by @purajit on 2025-05-16 01:00_

Having fixed our entire packaging and making everything PEPpy, I definitely don't think `ruff analyze graph` should care about `PYTHONPATH`. However, a warning might be helpful, consider bad packaging is pretty commonplace.

---

_Closed by @purajit on 2025-05-16 01:00_

---
