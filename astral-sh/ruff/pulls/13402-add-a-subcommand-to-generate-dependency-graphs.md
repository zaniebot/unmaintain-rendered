```yaml
number: 13402
title: Add a subcommand to generate dependency graphs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/import-map
created_at: 2024-09-18T23:46:42Z
updated_at: 2024-09-20T01:20:35Z
url: https://github.com/astral-sh/ruff/pull/13402
synced_at: 2026-01-10T21:30:32Z
```

# Add a subcommand to generate dependency graphs

---

_Pull request opened by @charliermarsh on 2024-09-18 23:46_

## Summary

This PR adds an experimental Ruff subcommand to generate dependency graphs based on module resolution.

A few highlights:

- You can generate either dependency or dependent graphs via the `--direction` command-line argument.
- Like Pants, we also provide an option to identify imports from string literals (`--detect-string-imports`).
- Users can also provide additional dependency data via the `include-dependencies` key under `[tool.ruff.import-map]`. This map uses file paths as keys, and lists of strings as values. Those strings can be file paths or globs.

The dependency resolution uses the red-knot module resolver which is intended to be fully spec compliant, so it's also a chance to expose the module resolver in a real-world setting.

The CLI is, e.g., `ruff graph build ../autobot`, which will output a JSON map from file to files it depends on for the `autobot` project.


---

_Review requested from @carljm by @charliermarsh on 2024-09-18 23:46_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-09-18 23:46_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-18 23:46_

---

_Comment by @github-actions[bot] on 2024-09-19 00:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @AlexWaygood on `crates/ruff_graph/src/collector.rs`:67 on 2024-09-19 02:24_

An empty string is also not a valid module name. You could fix that by doing something like this?

```diff
diff --git a/crates/red_knot_python_semantic/src/module_name.rs b/crates/red_knot_python_semantic/src/module_name.rs
index 885c6adf9..3bce5b2ec 100644
--- a/crates/red_knot_python_semantic/src/module_name.rs
+++ b/crates/red_knot_python_semantic/src/module_name.rs
@@ -59,7 +59,7 @@ impl ModuleName {
     }
 
     #[must_use]
-    fn is_valid_name(name: &str) -> bool {
+    pub fn is_valid_name(name: &str) -> bool {
         !name.is_empty() && name.split('.').all(is_identifier)
     }
 
diff --git a/crates/ruff_graph/src/collector.rs b/crates/ruff_graph/src/collector.rs
index 9b8c6108a..687fd7c20 100644
--- a/crates/ruff_graph/src/collector.rs
+++ b/crates/ruff_graph/src/collector.rs
@@ -1,9 +1,9 @@
+use red_knot_python_semantic::ModuleName;
 use ruff_python_ast::helpers::collect_import_from_member;
 use ruff_python_ast::name::QualifiedName;
 use ruff_python_ast::visitor::source_order::walk_module;
 use ruff_python_ast::visitor::source_order::{walk_expr, walk_stmt, SourceOrderVisitor};
 use ruff_python_ast::{self as ast, Expr, Mod, Stmt};
-use ruff_python_stdlib::identifiers::is_identifier;
 
 /// Collect all imports for a given Python file.
 #[derive(Default, Debug)]
@@ -64,7 +64,7 @@ impl<'ast> SourceOrderVisitor<'ast> for Collector<'ast> {
                 // Determine whether the string literal "looks like" an import statement: contains
                 // a dot, and consists solely of valid Python identifiers.
                 let value = value.to_str();
-                if value.split('.').all(is_identifier) {
+                if ModuleName::is_valid_name(value) {
                     let qualified_name = QualifiedName::from_dotted_name(value);
                     self.imports.push(CollectedImport::Import(qualified_name));
```

---

_Review comment by @AlexWaygood on `crates/ruff_graph/src/resolver.rs`:38 on 2024-09-19 02:26_

You can just reuse the logic we have in `ModuleName::new()` here as well, I think

---

_@AlexWaygood reviewed on 2024-09-19 02:27_

Couple of comments from skimming!

---

_Comment by @charliermarsh on 2024-09-19 02:48_

This needs tests prior to merging but I would like feedback. 

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:137 on 2024-09-19 06:17_

I'm leaning toward introducing a more general `analyze` subcommand and make `graph` or `module-graph` a subcommand of that so that we can introduce more analysis in the future without "polluting" the root commands.

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/graph.rs`:23 on 2024-09-19 06:18_

Too bad that red knot doesn't support exclude/include configurations yet because you would then get all of this for free (lazy file discovery)

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 06:24_

What's the motivation for creating a new db for each source root? 

The caching is local to each database, and creating a new database for each package means that we trade less congestion with reduced cache efficiency. This might be fine if the resolved modules in each package root are distinct but I suspect that the files contain cross-package imports and many stdlib imports that now need to resolved multiple times. 


The recommended way to parallelism in Salsa is to implement a `snapshot` method for your `Database` that creates a shallow clone:

```rust
    #[must_use]
    pub fn snapshot(&self) -> Self {
        Self {
            workspace: self.workspace,
            storage: self.storage.clone(),
            files: self.files.snapshot(),
            system: Arc::clone(&self.system),
        }
    }
```

https://github.com/astral-sh/ruff/blob/ecab04e33851b26cf2e8376be46ca01ce40185e6/crates/red_knot_workspace/src/db.rs#L82-L90


And you can then create a unique database handle for each task:

```rust
        let files = WorkspaceFiles::new(db, self);
        let result = Arc::new(std::sync::Mutex::new(Vec::new()));
        let inner_result = Arc::clone(&result);

        let db = db.snapshot();
        let workspace_span = workspace_span.clone();

        rayon::scope(move |scope| {
            for file in &files {
                let result = inner_result.clone();
                let db = db.snapshot();
                let workspace_span = workspace_span.clone();

                scope.spawn(move |_| {
                    let check_file_span = tracing::debug_span!(parent: &workspace_span, "check_file", file=%file.path(&db));
                    let _entered = check_file_span.entered();

                    let file_diagnostics = check_file(&db, file);
                    result.lock().unwrap().extend(file_diagnostics);
                });
            }
        });

        Arc::into_inner(result).unwrap().into_inner().unwrap()
```

https://github.com/astral-sh/ruff/blob/599103c933e129c6ec9be533c2c563721a58f3bb/crates/red_knot_workspace/src/workspace.rs#L206-L220

Edit: I see below that you already did this. I'm still curious for why we need multiple Dbs. I think what we do here is too granular. At least if I remember how package roots works correctly. I think it doesn't just generate a db for the top-level packages but also for all sub packages. That means that we have two database for `a.b.c`: one for package `a` and another for `b` if `c` is a module and `a` and `a.b` are packages. The result is that we double-resolve modules for `a.b`, depending on whether they come from the `a` or `a.b.` database. 

I also worry that relative imports don't work due to that because I think we might baile out at the `src_root`. 

Can't we generate a single database or a database for each common ancestor of the packages (which ideally is the src directory)?  

If not, can we add some package paths to `SearchPathSettings::extra_paths`?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/os.rs`:24 on 2024-09-19 06:26_

This shouldn't require clone

```suggestion
#[derive(Default, Debug)]
```

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/collector.rs`:67 on 2024-09-19 06:28_

My preference here is to simply call `ModuleName::new` and return the module name instead of the `QualifiedName`. It's not a valid module name if `ModuleName::new` returns `None`, otherwise it's a legal python module name.

This also avoids exposing any internal `ModuleName` methods.

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/lib.rs`:88 on 2024-09-19 06:31_

Since you're already using Salsa:

```rust
let file = system_path_to_file(db, path)?;
let parsed = parsed_module(db, file);
```

It could help to reduce some IO operations (at the cost that the source code is now cached)

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/lib.rs`:80 on 2024-09-19 06:32_

I would recommend you to use `SystemPath` in more places and do the conversation from `Path` at the boundary rather than ad-hoc in here.

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/resolver.rs`:14 on 2024-09-19 06:34_

Nit: The convention is that `db` comes first
```suggestion
    pub(crate) fn new(db: &'ast ModuleDb, module_path: Option<&'ast [String]>) -> Self {
        Self { module_path, db }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/resolver.rs`:9 on 2024-09-19 06:34_

Nit: the use of the `ast` lifetime for `db` is somewhat confusing. Maybe just use `'a`.

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/resolver.rs`:100 on 2024-09-19 06:35_

Nit: I would use `ModuleName` throught all your code instead of switching between `QualifiedName` and `ModuleName`. 

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/settings.rs`:75 on 2024-09-19 06:36_

The indent here seems off

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/settings.rs`:20 on 2024-09-19 06:36_

What's the reason for making `Settings` clone?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:3318 on 2024-09-19 06:37_

Should we remove the `OptionsMetadata` trait for now to exclude the settings from the schema?

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/resolver.rs`:94 on 2024-09-19 06:46_

You may want to use something closer to 

https://github.com/astral-sh/ruff/blob/d988204b1b3935a9c449495efe5cdf21e4b82887/crates/red_knot_python_semantic/src/types/infer.rs#L1225-L1257



---

_Review comment by @MichaReiser on `crates/ruff_graph/src/resolver.rs`:9 on 2024-09-19 06:48_

I should have brought this up before. Using the salsa queries directly is something we want to red-knot's core (ideally the fact that we use salsa should be mostly hidden). Could you have a look if you can use `SemanticModel` model here which we consider to be public api

---

_@MichaReiser reviewed on 2024-09-19 06:49_

Overall looks good. 

I think we're creating too many Db instances which will hurt performance and I would recommend to use `ModuleName` and `SystemPath` over `QualifiedName` and `Path`  to avoid conversions at the leaf. 

---

_@MichaReiser reviewed on 2024-09-19 06:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/graph.rs`:82 on 2024-09-19 06:50_

It's a bit unfortunate that we need to clone this for every path.

---

_@AlexWaygood reviewed on 2024-09-19 10:16_

---

_Review comment by @AlexWaygood on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 10:16_

> If not, can we add some package paths to `SearchPathSettings::extra_paths`?

This isn't really "correct" since `extra_paths` take priority over even the stdlib search path in module resolution. But maybe it'll produce the right answer in most cases.

In the real world, if you have multiple independent first-party packages in a workspace (which may or may not depend on each other), you're likely to have them all editably installed into a venv, and red-knot would statically discover all the first-party packages by reading the `pth` files in the `site-packages` directory and add them as search paths that way. But maybe that approach doesn't work here.

---

_@charliermarsh reviewed on 2024-09-19 12:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:137 on 2024-09-19 12:55_

Sounds good, I like that.

---

_@charliermarsh reviewed on 2024-09-19 15:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 15:28_

The source roots here are actually just the root folders. So, like, in Django, it's just a small set of top-level folders:

```python
["/Users/crmarsh/workspace/django", "/Users/crmarsh/workspace/django/tests", "/Users/crmarsh/workspace/django/tests/admin_scripts/custom_templates", "/Users/crmarsh/workspace/django/tests/admin_scripts/custom_templates/project_template", "/Users/crmarsh/workspace/django/tests/i18n/sampleproject", "/Users/crmarsh/workspace/django/tests/migrations/faulty_migrations/namespace", "/Users/crmarsh/workspace/django/tests/migrations/test_fake_initial_case_insensitive"]
```

---

_@charliermarsh reviewed on 2024-09-19 15:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 15:29_

We could use a single database with extra paths -- that's actually the way I did it initially, though Alex said it's not the intended use-case. The downside, I think, is that it would allow those source roots to import files across one another. Maybe that's correct though! I can change it.

---

_@charliermarsh reviewed on 2024-09-19 15:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/graph.rs`:82 on 2024-09-19 15:29_

We're only cloning the return value though, which should be unique for every path.

---

_@AlexWaygood reviewed on 2024-09-19 15:40_

---

_Review comment by @AlexWaygood on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 15:40_

> The source roots here are actually just the root folders.

Possibly I'm still misunderstanding, but if everything's importable from the workspace root, doesn't that imply you can resolve all modules and submodules relative to a single search path, `src_root`, which would point to the workspace root?

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 15:59_

Alex is obviously the authority when it comes to search paths and I'm probably wrong haha :laughing: 

The intended setup is to use a single database for an application (except VS code where you can have multiple workspaces). That gives the best cache reuse. If that's possible, great. If we aren't hitting any perf issues, that's great too.

---

_@MichaReiser reviewed on 2024-09-19 15:59_

---

_@carljm reviewed on 2024-09-19 22:27_

---

_Review comment by @carljm on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 22:27_

Looking at the roots for Django that were listed, I think those are all independent roots (as in, they aren't all importable from a single source root), but they can import each other. So I think it would be more efficient to use a single db, by using multiple you are likely repeating work. It's true that the red-knot resolver (because it was built around the more restrictive assumptions of PEP 561 for Python type checkers, rather than a generic "ordered `sys.path`" abstraction) doesn't obviously accommodate this use case, but I think extra-paths is a totally fine way to handle it, and better than multiple databases. (In fact I think this is pretty much the sort of use-case extra-paths exists to handle.)

> This isn't really "correct" since `extra_paths` take priority over even the stdlib search path in module resolution.

I don't understand this comment: all first-party paths (both the main source root and the extra paths) should always take priority over the stdlib in module resolution. According to PEP 561 order, the first two items in the resolution order are extra paths and then the first-party root. So if you have multiple "first party roots", given that they have to go in _some_ resolution order, I don't see how any incorrectness is introduced by putting N-1 of them as extra paths, and whichever one you want last as the main first-party root.

---

_@AlexWaygood reviewed on 2024-09-19 22:31_

---

_Review comment by @AlexWaygood on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 22:31_

> > This isn't really "correct" since `extra_paths` take priority over even the stdlib search path in module resolution.
> 
> I don't understand this comment: all first-party paths (both the main source root and the extra paths) should always take priority over the stdlib in module resolution.

Ugh, you're quite right. Blame it on the holiday haze... yeah, using `extra_paths` should be fine then, in that case. Sorry for causing unnecessary work @charliermarsh :(

---

_@carljm reviewed on 2024-09-19 22:32_

---

_Review comment by @carljm on `crates/ruff/src/commands/graph.rs`:50 on 2024-09-19 22:32_

> In the real world, if you have multiple independent first-party packages in a workspace (which may or may not depend on each other), you're likely to have them all editably installed into a venv, and red-knot would statically discover all the first-party packages by reading the `pth` files in the `site-packages` directory and add them as search paths that way. But maybe that approach doesn't work here.

In many real-world cases (including Django), you'll have little sub-trees of Python code that aren't packaged at all or installed via Python packaging tools, but still end up on the runtime `sys.path` in some more bespoke way. That's the sort of scenario `extra-paths` is supposed to be for.

---

_Review comment by @carljm on `crates/ruff/src/commands/analyze_graph.rs`:43 on 2024-09-19 22:46_

This logic (determining a package root for an arbitrary file by walking up the tree looking for `__init__.py`) is part of Ruff so it must be fairly battle-tested, but I'm a little surprised if it doesn't often fail on big internal codebases. In my experience, "accidental namespace packages" (because someone just forgot an `__init__.py`, and imports worked anyway) are pretty common. Maybe a lot of people use linters (like Ruff, perhaps!) that prevent this? 

Because of that, I tend to be more on the side of, if you're going to be doing multi-file analysis, you should explicitly configure the package root(s), rather than trying to discover files and packages automatically, which is easily broken by namespace packages and therefore requires explicitly configuring namespace packages.

---

_Review comment by @carljm on `crates/ruff/src/commands/analyze_graph.rs`:89 on 2024-09-19 22:53_

How do we get TOML files here? I thought we used `python_files_in_path` to discover only Python files?

---

_Review comment by @carljm on `crates/ruff_graph/src/collector.rs`:56 on 2024-09-19 22:57_

Seems like all of this could go after the `if/else` instead of being duplicated in both branches -- the only thing that differs is establishing the initial components vec?

---

_Review comment by @carljm on `crates/ruff_graph/src/collector.rs`:61 on 2024-09-19 23:01_

If I'm reading `to_module_path` correctly, I think for an `__init__.py` module, `self.module_path` will be e.g. `['foo', 'bar', '__init__']` -- that is, it will include `__init__` as a final component?

That's necessary for this logic to be correct, since the meaning of a relative import is one level different in `foo/bar/__init__.py` vs in `foo/bar.py`.

---

_Review comment by @carljm on `crates/ruff_graph/src/collector.rs`:92 on 2024-09-19 23:04_

This is more aggressive than I realized -- we don't even look for calls to `__import__` or `importlib.import_module`, we just look for any string that might be a module path.

It seems like this could easily be fooled by cases where an import-path constant is defined in one file (like a Django settings file) but the string is imported and then used in a dynamic import in a different module.

But if it's good enough to match existing tools on the codebases we care about, great!

---

_@carljm approved on 2024-09-19 23:05_

Very cool!

---

_@charliermarsh reviewed on 2024-09-20 00:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/commands/analyze_graph.rs`:89 on 2024-09-20 00:05_

I think it will pick up anything in the default `include`:

```rust
pub(crate) static INCLUDE: &[FilePattern] = &[
    FilePattern::Builtin("*.py"),
    FilePattern::Builtin("*.pyi"),
    FilePattern::Builtin("*.ipynb"),
    FilePattern::Builtin("**/pyproject.toml"),
];
```

---

_@charliermarsh reviewed on 2024-09-20 00:06_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/collector.rs`:61 on 2024-09-20 00:06_

Correct!

---

_@charliermarsh reviewed on 2024-09-20 00:08_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/collector.rs`:56 on 2024-09-20 00:08_

Good call.

---

_Merged by @charliermarsh on 2024-09-20 01:06_

---

_Closed by @charliermarsh on 2024-09-20 01:06_

---

_Branch deleted on 2024-09-20 01:06_

---
