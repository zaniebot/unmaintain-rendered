```yaml
number: 11338
title: Salsa based red-knot prototype
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: red-knot-salsa
created_at: 2024-05-08T13:50:06Z
updated_at: 2024-08-12T07:53:06Z
url: https://github.com/astral-sh/ruff/pull/11338
synced_at: 2026-01-12T15:55:37Z
```

# Salsa based red-knot prototype

---

_@MichaReiser_

This is a prototype that uses Salsa for our red-knot prototype :laughing: 

The PR implements cross-module type inference invalidation based on Salsa. What makes this hard is that 

1. Salsa query arguments need to be ingredients. That means we can no longer pass arbitrary arguments to queries. 
2. Salsa limits invalidation if the result of a query compares equal to the value it returned previously. We need to remodel our query return values to make best use of that and avoid that e.g. all types invalidate on a single whitespace change (the thing we return from types should be location-independent


I'll go through the important data models jar by jar. 


## Source

The source jar gives access to files, the text of a file, and a file's AST.

**File**

https://github.com/astral-sh/ruff/blob/946493cc301c6a5cee8389bcbdb141bcfbeaba4a/crates/red_knot/src/salsa_db/source.rs#L31-L44

The file stores the basic metadata about a file but doesn't store the file's content. This is mainly because of persistent caching. Restoring the database from disk requires that we restore all files. If the source is stored on the file, we would have to read the content of every file, and that would be very expensive (we want the source validation to happen lazily). That's why the file only stores basic metadata. 

Note: We may decide long-term to have a configuration option that allows users to select if they want to use `mtime` or the file's has for change detection. In that case, I think we would have a `source: Option<String>` on file so that the `source_text` query avoids re-reading the file from disk. 

Files are salsa inputs. Salsa doesn't know how to compute files. Instead, we need to tell salsa which files exist and when they change. That's why files are resolved using `db.file(path)` where we perform our own mapping from `Path -> File` (Salsa inputs have no identity other than their instance).


**`SourceText`**

The `source_text(file: File) -> SourceText` query allows retrieving a file's source text. The source text isn't very exciting. It just stores the file's content. 

https://github.com/astral-sh/ruff/blob/946493cc301c6a5cee8389bcbdb141bcfbeaba4a/crates/red_knot/src/salsa_db/source.rs#L163-L167

Some notes about the implementation:

* The query calls `file.revision()` (equal to the file's `mtime`) to inform Salsa that the query should rerun whenever the file is modified. It actually doesn't need the value. 
* It might happen that the file has been deleted between calling `db.file(path)` and `source_text(db, file)`. In that case, we just assume that the file is empty. That's the best we can do without dealing with awkward results in all caller paths. 


**`parse`**

https://github.com/astral-sh/ruff/blob/946493cc301c6a5cee8389bcbdb141bcfbeaba4a/crates/red_knot/src/salsa_db/source.rs#L175-L183

The `parse` query is almost boring. It retrieves the file text and calls the parser. We opt out of Salsa's `eq` optimization because the parse tree is guaranteed to change whenever the source text changes (and our AST doesn't implement `Eq` because of floats).


## Semantic

This is where it gets interesting. 

**`AstIds`**

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/ast_ids.rs#L18-L24

`AstIds` are a location-independent representation that allows mapping from `Id -> AstNode` and from `AstNode -> Id`. The implementation tries to assign stable IDs by first giving IDs to the module-level statements and expressions, and only then traversing into the function or class level. 

```python
a = 10 # statement-id: 0

def test(a): # statement-id: 1
	if a: # statement-id: 4
		pass # statement-id 5

print(a) # statement-id: 2

class Test: # statement-id: 3
	pass # statement-id: 6
```

This way, IDs of top level statements remain unchanged when only making changes to a function's body. Having stable top-level IDs is important because they are referred to from other modules.

### symbols, cfg

**`semantic_index`**

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic.rs#L103-L123

The `semantic_index` query computes a single file's symbol table and control flow graph. It shouldn't be used directly because the `semantic_index` changes every time the AST changes.

**`symbol_table`**

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/symbol_table.rs#L21-L25

The query itself just calls into `semantic_index`. The trick here is that the symbol table itself doesn't contain any data that references the AST. Instead, all data uses `AstIds`. What this query enables is that Salsa can avoid running queries that depend on the `symbol_table` if the constructed symbol table hasn't changed. For example, a comment only change doesn't invalidate the symbol table. 

**`flow_graph`**

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/flow_graph.rs#L12-L17

We apply the same trick for the control flow graph

### Typing


**`typing_scopes`**

Typing is where the code changes the most. The existing implementation does type inference per expression. I don't think that `type_inference` per expression will be fast in Salsa because storing a query result has some overhead. Salsa is also limited to at most `u32` results per query. I think large projects could reach that limit, especially when the server runs for a long time. 

That's why this PR changes inference to happen per `TypingScope` instead. For now, a typing scope is either a `Module`, `Function`, or `Class`. So this PR infers all types per module, class, or function (but the module doesn't traverse into function or class bodies).

The reason why we don't perform type inference on a module scope is to get more fine-grained dependency tracking across files. The type checking of a dependency must only be rerun if the types of the scope where the symbol is defined depend on changes. If the types remain unchanged (for example because the public interface isn't changing), then type checking doesn't need to re-run.

The first step to make this possible is to create a `FunctionTypingScope` and `ClassTypingScope`s for every `Function` and `Class` in the file and store them in Salsa to use them as query arguments. 

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/types.rs#L26-L40


**`infer_*_body`**

The other important queries are `infer_module_body`, `infer_function_body`, and `infer_class_body`. They perform type inference for a single module, function or class, but without traversing into nested classes or functions. 

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/types/infer.rs#L39-L64


Doing type-checking per block introduces some complexity. Mainly that getting the type data for a `TypeId` not just requires knowing the file from which the data needs to be read, but also from which typing scope. There's even an extra complexity. There are cases where we want to to resolve the type for a `type_id`. But we may only just be building up that typing table. I solved this by introducing `TypingContext` and passing that to `TypeId::ty`. The `TypingContext` can have an override so that queries for a specific typing-scope are directly resolved without calling into the database. 

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/types.rs#L527-L556

**Public API**

The public API for types should be limited to:

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic/types/infer.rs#L24-L29

https://github.com/astral-sh/ruff/blob/176267f00b0162dee7545dea4ad14daf24f4897d/crates/red_knot/src/salsa_db/semantic.rs#L79-L101

### Module Resolver

The module resolver remains mostly unchanged, although I did some renaming. 

**`Module`**

I think the naming could be better. `Module` is mainly a `ModuleName` but interned into salsa so that it can be used as a query argument. 

https://github.com/astral-sh/ruff/blob/4c7033700dd3f7c954254de5f1c980a860c7e079/crates/red_knot/src/salsa_db/semantic/module.rs#L13-L17

I didn't want to intern `ModuleName` directly because I think there are places where we want to use it without the need for having it in Salsa. But maybe that's the wrong call and we should just intern `ModuleName` directly. 

**`resolve_module`**

The main query remains `resolve_module`

https://github.com/astral-sh/ruff/blob/4c7033700dd3f7c954254de5f1c980a860c7e079/crates/red_knot/src/salsa_db/semantic/module.rs#L193-L214

What changed is that it now accepts a `Module` and returns an `Option<ResolvedModule>`. Again, I'm open to suggestion for better naming. The idea is that a `ResolvedModule` represents to what a module name resolves. I'm consider renaming it to `ResolvedModulePath` because I think that's really what it is.

I think the implementation became much simpler because the module resolver now uses `File` and `File::exists` internally. This has the advantage that Salsa will automatically invalidate the `resolve_module` result if a relevant file gets added or removed. 

**`file_to_module`**

https://github.com/astral-sh/ruff/blob/4c7033700dd3f7c954254de5f1c980a860c7e079/crates/red_knot/src/salsa_db/semantic/module.rs#L228

Resolves a `file` to a `Option<ResolvedModule>` if it is a module and to `None` otherwise. This is mostly unchanged.

**`module_search_paths` and `set_module_search_paths`**

https://github.com/astral-sh/ruff/blob/4c7033700dd3f7c954254de5f1c980a860c7e079/crates/red_knot/src/salsa_db/semantic/module.rs#L178-L185

These queries shouldn't exist long term but it was a "quick" way to allow setting the module search paths without supporting settings. I'll adapt this to @AlexWaygood's most recent changes by having a `set_module_resolver_settings` short term (that has fields for the different lookup paths). The long term goal is that the module resolver queries the settings and constructs the search paths from the settings (it probably should remain a query)

---

_Comment by @github-actions[bot] on 2024-05-08 14:09_

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

_Closed by @MichaReiser on 2024-05-28 08:03_

---

_Reopened by @MichaReiser on 2024-05-28 08:27_

---

_Comment by @codspeed-hq[bot] on 2024-05-28 08:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-salsa)

### Merging #11338 will **not alter performance**

<sub>Comparing <code>red-knot-salsa</code> (06ee178) with <code>main</code> (2e0a975)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-05-31 13:28_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/module.rs`:20 on 2024-05-31 13:28_

@AlexWaygood this is where I'm currently landing on a Salsa design for a module resolver. I think it would simplify a lot for you because you no longer need to think about invalidation, Salsa will take care of that for you. The only thing necessary for this to work is that you use `db.file(path).exists()` to test if a file exists. 

But check out `resolve_module`, it's now almost empty!

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/module.rs`:221 on 2024-05-31 13:30_

It's a bit weird that `path_to_module` converts the path to a `file` as the very first thing only so that `file_to_module` then reads the path. However, for `file_to_module` to be a salsa query, it can only accept an ingredient as an argument and `file` is an ingredient but `path` isn't.

---

_@MichaReiser reviewed on 2024-05-31 13:30_

---

_@AlexWaygood reviewed on 2024-05-31 13:31_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/module.rs`:20 on 2024-05-31 13:31_

Ah, thanks for the ping. Yes, this indeed does make the code look a _lot_ cleaner! It was making my head hurt a little bit to see all the cache-checking stuff right alongside the search-path semantics in `resolve_module()`

---

_Renamed from "Restore Salsa DB for exploring Salsa further" to "Salsa based red-knot prototype" by @MichaReiser on 2024-06-07 15:48_

---

_Comment by @MichaReiser on 2024-06-07 19:17_

There's one limitation with the current model where the invalidation isn't as good as it could be and it is due to the fact that we build the entire symbol table at once (we don't have to and we could refactor that later). 

Let's say we start with

```python
# main.py
import foo;

x = foo.x

# foo.py
x = 10

def foo(): 
	pass
```

And we infer the type of `x` in `main`.  To do this, the implementation runs
* It parses `main` , builds its symbol table and cfg, calls into `infer_module_body`
* It resolves `foo` when reaching `import foo` 
* It calls `ModuleType.member` when reaching `foo.x`. This fans out to parse `foo`, build its symbol table and control flow graph, and then runs module-level type inference for `foo`
* ...

When we now change the content of `foo` to 

```py
x = 10

def foo(): 
	y = 10
```

What I expected is that the type inference for `main` wouldn't re-run because the module-level types of `foo` remain unchanged. However, that's not the case. The reason is that `foo.x` resolved the symbol table of `foo.py` and the symbol table has changed because we introduced `y` in the scope of `foo`.  We have the same problem when a flag of an enclosing symbol changes. For example if the body of `foo.foo` is changed to `y = x`. The symbol table of that module changes because `x` now has the flag `used`.

We can avoid this by also building the symbol table per scope rather than once globally. Or have a query that reduces the global symbol table to just the global symbols. I do think something like that would be nice to have more fine granular invalidations.

---

_Label `red-knot` added by @MichaReiser on 2024-06-07 19:37_

---

_Review comment by @carljm on `Cargo.toml`:7 on 2024-06-07 19:44_

Why are we dropping our rust version in this PR? Did you add a dependency here that doesn't work with 1.74?

---

_Review comment by @carljm on `crates/red_knot/Cargo.toml`:34 on 2024-06-07 19:45_

Does this incorporate any of Niko's newest work on "v3" yet? Or are those changes we'll have to adapt to yet in the future?

---

_Comment by @carljm on 2024-06-07 21:58_

This is a great write-up, thanks for taking the time! A few thoughts from the write-up, before I dive into the code:

1. One caveat with building symbol tables per-scope is that the contents of nested functions can affect the symbol table of the enclosing function. (If a nested function uses `nonlocal x` and assigns to `x`, the symbol kind for `x` may change based on the knowledge that a nested function might assign to it.) So we can build symbol tables separately for module scope, class scope, and function scope -- but building a function symbol table needs to build all enclosed symbol tables (which may involve N nested class and function scopes, to arbitrary depth.) I think in practice this is still fine, though, and worth doing the split. Nesting isn't that common, and by definition the symbols inside a function scope aren't ones that another module can depend on, anyway.
2. My initial feeling is that what you are calling `Module` maybe should be called `ModuleName`, and what you are calling `ResolvedModule` maybe should be called `Module`. But I'm not totally sure until I look closer at the code.
3. I think if we are doing type inference per-scope rather than per-expression that will probably also recommend changes to how type inference works in the first place. We can probably just walk the AST for the scope, assigning types to expressions as we go, and similarly tracking narrowed local types for local symbols as we go. This will mean we don't even need `FlowGraph` anymore (a lot of the concepts developed in building it will still carry over, it will just be resolved eagerly instead.)

---

_Comment by @MichaReiser on 2024-06-08 06:07_

> One caveat with building symbol tables per-scope is that the contents of nested functions can affect the symbol table of the enclosing function. 

The part that's unclear to me is how we would compute flags like `USED` when building the symbol table lazily per scope, because a symbol might be used in a child scope. But we also have the option to build the entire symbol table at once and then split it per scope (similar to the trick with `SymbolIndex`, build it once, then have sub-queries that only return a slice of that. 

> My initial feeling is that what you are calling Module maybe should be called ModuleName, and what you are calling ResolvedModule maybe should be called Module. But I'm not totally sure until I look closer at the code.

Yeah, but that would require that `ModuleName` becomes a sala ingredient. It might be fine but it makes `ModuleName` a bit more awkward to use. But if we keep `ModuleName` a regular struct, than what would you call the module thing that we pass to `resolve_module`?  I might be overthinking this because we can make the argument to `resolve_module` private and only expose a `resolve_module(db: &dyn Db, name: &str)` function that internally converts the `&str` to that `Module` thing and we can then keep our existing terminology. Maybe that's for the best (it also hides complexity). 

 My thinking why I called the `Module { name: ModuleName }` a module is that I don't think that the existence of the file on the disk makes a module. When we have `import foo`, then there's an import of the `foo` module, regardless if that module exists or not. That's why I think that a module's identity is really defined by its name.

> We can probably just walk the AST for the scope, assigning types to expressions as we go, and similarly tracking narrowed local types for local symbols as we go. This will mean we don't even need FlowGraph anymore (a lot of the concepts developed in building it will still carry over, it will just be resolved eagerly instead.)

This is what the new implementation does. But I must say, it would make me sad to see your CFG go away.  I think it could be useful for other things than just typing, like an unreachable rule. 



---

_Comment by @carljm on 2024-06-08 16:21_

> The part that's unclear to me is how we would compute flags like USED when building the symbol table lazily per scope, because a symbol might be used in a child scope.

The `USED` flag only means "used in current scope", so it's not an issue. The only analysis that crosses scopes is the one Patrick was working on, and that's the case I discussed in my comment; but it only applies to nested scopes within function scopes, so handling a function scope and all it's nested scopes together is one way to handle it; module scopes and class scopes not nested in functions can be fully independent.

> build it once, then have sub-queries that only return a slice of that.

This could work, too.

> When we have import foo, then there's an import of the foo module, regardless if that module exists or not. That's why I think that a module's identity is really defined by its name.

This is a good point. I think it's a solid enough reason for the current naming. We will need to be able to track dependencies on nonexistent modules.

> This is what the new implementation does.

"tracking narrowed local types for local symbols as we go" is the part that would specifically replace the CFG; I don't think the implementation here does that yet.

> it would make me sad to see your CFG go away. I think it could be useful for other things than just typing, like an unreachable rule.

The eager version of the same logic would also have the ability to discover unreachable branches.




---

_Review comment by @AlexWaygood on `crates/red_knot/Cargo.toml`:43 on 2024-06-10 10:47_

nit ;)

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic.rs`:43 on 2024-06-10 10:52_

An ID for what, exactly? A file, a module, a type, a definition...? Could maybe do with a docstring here

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic.rs`:71 on 2024-06-10 10:56_

Would be nice to have a docstring for this type as well

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic.rs`:117 on 2024-06-10 11:04_

Maybe this should go into a `new` associated method for `SemanticIndexer` (or an implementation of the `Default` trait)?

```rs
impl SemanticIndexer {
    fn new(db: &dyn Db, file: File) -> Self {
        Self {
            db,
            file,
            symbol_table_builder: SymbolTableBuilder::new(),
            flow_graph_builder: FlowGraphBuilder::new(),
            scopes: vec![ScopeState {
                scope_id: SymbolTable::root_scope_id(),
                current_flow_node_id: FlowGraph::start(),
            }],
            current_definition: None,
        }
    }
}

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:22 on 2024-06-10 11:52_

The second variant of this enum is so that we can also detect when the "revision" of a vendored source file changes?

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:29 on 2024-06-10 12:26_

I'm not sure whether this deserves to be a custom enum, or whether we should just use a `bool`. An argument in favour of it being a `bool` is that really the only thing we care about is whether the file exists or not, and I think a `exists: bool` field in the `File` struct would be more expressive of that. ("Status" could mean a lot of other things apart from whether it exists or not.) An argument in favour of it being a custom enum is that `self.set_status(db).to(FileStatus::Exists)` is probably more readable than `self.set_exists(db).to(true)`

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:36 on 2024-06-10 12:28_

What do we need to know the permissions of the file for? If we don't have permissions to read it, maybe we should just mark it as not existing -- for our purposes, they probably come to the same thing?

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:43 on 2024-06-10 12:41_

You probably already know this, but in most of the `countme` docs, they seem to use fields prefixed with `_` for these `Count`s in their examples, which would mean (I think) we could avoid the `#[allow(unused)]` attributes

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:146 on 2024-06-10 12:47_

```suggestion
                let (last_modified, permissions, file_status) = if let Ok(metadata) = metadata {
                    if metadata.is_file() {
                        let last_modified = FileTime::from_last_modification_time(&metadata);
                        #[cfg(unix)]
                        let permissions = if cfg!(unix) {
                            use std::os::unix::fs::PermissionsExt;
                            metadata.permissions().mode()
                        } else {
                            0
                        };

                        (last_modified, permissions, FileStatus::Exists)
                    } else {
                        (FileTime::zero(), 0, FileStatus::Deleted)
                    }
                } else {
                    (FileTime::zero(), 0, FileStatus::Deleted)
                };

                let file = File::new(
                    db,
                    path,
                    permissions,
                    FileRevision::LastModified(last_modified),
                    file_status,
                    Count::default(),
                );
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/types/infer.rs`:121 on 2024-06-10 12:49_

```suggestion
        TypeInferenceBuilder {
            db,
            typing_scope: scope,
            file,
            enclosing_scope: symbol_scope,

            symbol_table,
            control_flow_graph,
            typing_scopes: typing_scopes(db, file),

            result: TypeInference::default(),
        }
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/types.rs`:269 on 2024-06-10 13:51_

Some docstrings for these `*Id` types would be really helpful

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/symbol_table.rs`:395 on 2024-06-10 14:02_

I think if we derived `Default` on the `SymbolTable` struct, this could just be

```suggestion
        let mut table = SymbolTable::default();
```

right?

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/flow_graph.rs`:150 on 2024-06-10 14:11_

I _think_ this shouldn't be necessary since the function's `pub`?

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:64 on 2024-06-10 14:17_

I think these `#[allow(unused)]` shouldn't be needed because they're `pub`

```suggestion
    pub fn functions(&self) -> impl Iterator<Item = (FunctionId, &StmtFunctionDef)> {
        self.statements
            .iter_enumerated()
            .filter_map(|(index, stmt)| Some((FunctionId(index), stmt.as_function_def_stmt()?)))
    }

```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:35 on 2024-06-10 14:23_

Did you consider using something like https://docs.rs/bimap/latest/bimap/ here, instead of having one mapping for ID-to-expression, and another mapping for expression-to-ID (IIUC)?

---

_@AlexWaygood reviewed on 2024-06-10 14:32_

Thanks for the great PR writeup. Overall this looks good to me, though it looks like it's missing some of my recent changes to `module.rs`.

I left a bunch of comments below, mostly pretty minor. I think many of them may also apply to the existing red-knot codebase -- I wouldn't yet consider myself an expert in the crate overall -- so please feel free to ignore any that you don't feel are useful. I think @carljm will probably be a much better reviewer for this in general :/

---

_Comment by @AlexWaygood on 2024-06-10 14:40_

> ### Module Resolver
> 
> The module resolver remains mostly unchanged, although I did some renaming.
> 
> **`Module`**
> 
> > My initial feeling is that what you are calling Module maybe should be called ModuleName, and what you are calling ResolvedModule maybe should be called Module. But I'm not totally sure until I look closer at the code.
> 
> Yeah, but that would require that `ModuleName` becomes a sala ingredient. It might be fine but it makes `ModuleName` a bit more awkward to use. But if we keep `ModuleName` a regular struct, than what would you call the module thing that we pass to `resolve_module`? I might be overthinking this because we can make the argument to `resolve_module` private and only expose a `resolve_module(db: &dyn Db, name: &str)` function that internally converts the `&str` to that `Module` thing and we can then keep our existing terminology. Maybe that's for the best (it also hides complexity).

I wonder if what's currently called `Module` could be renamed to `ModuleRequest`. The user "requests" a module (and the request is represented with a `ModuleRequest` instance) by importing a module with a certain `ModuleName`, but they might not actually get a module back, because the module might not actually exist. Unlike the `Module` type on the `main` branch, your `Module` type in `crates/red_knot/src/salsa_db/semantic/module.rs` doesn't really feel like a _Module_ to me, as you can't query any information about the module directly from the type -- you have to resolve it first, and it feels like the module object is the thing you get given at the end of the resolution process.

---

_@MichaReiser reviewed on 2024-06-10 14:48_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:43 on 2024-06-10 14:48_

Oh yeah, this is a prototype. I'll write documentation before pulling this into the "real" version.

---

_@MichaReiser reviewed on 2024-06-10 14:49_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/source.rs`:22 on 2024-06-10 14:49_

No, this was mainly to explore how and if we could support file revisions e.g. based on a file's hash rather than the last modified timestamp. But this isn't used right now.

---

_@MichaReiser reviewed on 2024-06-10 14:50_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/source.rs`:43 on 2024-06-10 14:50_

Yeah, I used `_count` but the salsa generated macro code than uses `_count` and genrates a `__count` field name that generates another clippy warning. So it's the allow unused or that other warning :D

---

_@AlexWaygood reviewed on 2024-06-10 14:51_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/source.rs`:43 on 2024-06-10 14:51_

Aha!

---

_@MichaReiser reviewed on 2024-06-10 14:52_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:35 on 2024-06-10 14:52_

I haven't, and i wasn't aware of that data structure. I prefer our implementation because we use an `IndexVec` for statements and `expressions` where a lookup is just an array offset whereas `BiMap` would require a hash map lookup.

---

_Comment by @MichaReiser on 2024-06-10 14:54_

Thanks @AlexWaygood for the feedback. I don't plan to incorporate any of the code changes into this PR because I don't plan on merging. I'll incorporate your changes when working on the specific areas before pulling them into ruff.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db.rs`:60 on 2024-06-10 15:24_

What does this event mean?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/lint.rs`:59 on 2024-06-10 15:32_

I'm not sure why we would bother with a simplified semantic model (unless it's extremely simple). It seems better to just give the rules that need semantic information access to the full semantic model, and avoid inconsistency.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:92 on 2024-06-10 15:35_

Should we be consistent about using `_ty` vs `_type` in APIs?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:105 on 2024-06-10 15:45_

This returns an owned SemanticIndex. How does this work in Salsa, where the index returned might be cached in the salsa db?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:503 on 2024-06-10 15:46_

How is this better than implementing `visit_expr` and just not calling walk on anything from there?

I'm asking because a) I think maybe dependencies should be collected in semantic indexing, and b) I don't think semantic indexing should use SourceOrderVisitor.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:487 on 2024-06-10 15:47_

We already build dependencies in the symbol table builder as well -- is there a reason for these to be separate AST visits? Maybe dependencies should also be part of the semantic index?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:577 on 2024-06-10 15:50_

Curious to know your thinking about source order here; I find it a bit confusing to have these defined at the bottom of the module, when they are frequently referred to throughout the module.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic.rs`:574 on 2024-06-10 15:50_

What are all these things? It seems to include queries, but also some types.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:346 on 2024-06-10 16:53_

Not clear on the meaning of the initial underscore here. Doesn't the lack of `pub` sufficiently indicate the field is private?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:406 on 2024-06-10 16:54_

Why do these impls have to be `unsafe`?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/flow_graph.rs`:63 on 2024-06-10 16:59_

Does this need to be `pub`? We're clearly using it below in `reachable_definitions`, not sure we'll need it anywhere else.

Is the unused lint about the pub?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/symbol_table.rs`:395 on 2024-06-10 17:08_

I intentionally avoided that, IIRC, because I don't want it to be possible (and especially not easy!) to create a SymbolTable without the root scope.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:45 on 2024-06-10 17:11_

Remind me what this does?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:112 on 2024-06-10 17:15_

nit: inconsistent use of `Self` vs `TypingScope`

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:125 on 2024-06-10 17:26_

I'm not sure about this logic; feels more convoluted than it should be (and without spending more time on it, I'm having trouble evaluating whether it's correct or not). On a shallow level I think the cause is that we should be able to trivially build a 1:1 mapping direct from ScopeId to TypingScope; maybe if we created TypingScope in semantic indexing?

But I think the root cause is that it's not really clear why we have both Scope/ScopeId and TypingScope at all. They represent the same thing and should have a 1:1 relationship with each other: a scope is a scope. Scopes in the symbol table already know if they are module or function or class scopes. So why should `TypingScope` even exist separately from a symbol table scope?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:112 on 2024-06-10 21:22_

The awkward naming here is kind of another indication that we have too many things named "scope" -- "symbol scope" isn't really a sensible name, but it's needed to distinguish from typing scope. Why are they two separate things that we have to map between?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:589 on 2024-06-10 21:25_

Not sure what a "location definition" is?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:579 on 2024-06-10 21:26_

We do need a place to add this deduplication (as well as the flattening/simplification that I already added in PRs since you translated this to Salsa); it's not clear to me where in this structure that should happen.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types.rs`:647 on 2024-06-10 21:31_

I'm not sure what a `DefinitionType` is supposed to represent that is different from a `Type`, or why it needs to exist at all. It seems like all it does is intern unions? (And with narrowing it will probably have to intern intersections, too.) But `infer_definitions` already has a `TypeInference` -- why can't it do the interning itself?

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types/infer.rs`:89 on 2024-06-10 21:57_

The location of this comment seems odd; it's not clear what code it is referring to.

---

_Review comment by @carljm on `crates/red_knot/src/salsa_db/semantic/types/infer.rs`:161 on 2024-06-10 23:56_

It's strange to me that we build definitions in SemanticIndexer, but now we're rebuilding definitions from scratch here as well. This seems like duplication we probably don't want.

---

_@carljm reviewed on 2024-06-11 00:08_

I looked over all the code. This was a lot of work to translate all this, thanks for doing this! I don't see anything here that I think can't work in the new approach. I think overall on the semantic side this PR now has kind of a mish-mash of the old approach  (per expression laziness) and the new approach (per scope typing) that is probably more complex and less efficient than we could achieve, so I expect that over the next few weeks we'll want to re-work and simplify a fair bit of it. But it makes sense to land something _working_ with Salsa and iterate from there.

---

_@MichaReiser reviewed on 2024-06-11 05:40_

---

_Review comment by @MichaReiser on `crates/red_knot/Cargo.toml`:34 on 2024-06-11 05:40_

Not yet, v3 is only a PR at this point. I scanned through the code and v3 is fairly close to v2022, so we're using that for now. But yes, we'll probably have to adapt some code.

---

_@MichaReiser reviewed on 2024-06-11 05:43_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db.rs`:60 on 2024-06-11 05:43_

It's possible to create multiple snapshots of the database that then each can run in isolation (they still share the underlying caches). This is useful when using salsa in a multithreaded context. 

Now, Salsa cancels any pending snapshots (other threads) when you want to make changes to it. The way this works is that each query tests if cancellation was requested and if so, it panics with a specific error. The `WillCheckCancellation` indicates that Salsa now tests cancellation. 

I removed the log because it is very noisy. I think I often saw 2-3 of these logs per query. Maybe something that can be optimized later to reduce it to just one. Removing it made the log a bit more dense and easier to read thorough

---

_@MichaReiser reviewed on 2024-06-11 05:45_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/lint.rs`:59 on 2024-06-11 05:45_

Neither do I but it probably also depends on what we refer to as the semantic model. Is it any information that isn't part of the AST? If so, maybe exposing the parent expression or statement is something that we can support even for syntax rules. But yeah, I don't know if it's worth it. I think this is a comment copied from the existing  implementation.

---

_@MichaReiser reviewed on 2024-06-11 05:45_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:92 on 2024-06-11 05:45_

Probably. `ty` is somewhat common in the Rust ecosystem and has the advantage that it isn't a keyword (it also works for variables).

---

_@MichaReiser reviewed on 2024-06-11 05:46_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:105 on 2024-06-11 05:46_

The trick here is the `return_ref` in the attribute. The actual query then returns a `&'db SemanticIndex`. Salsa uses some unsafe code to achieve this. 

---

_@MichaReiser reviewed on 2024-06-11 05:49_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:503 on 2024-06-11 05:49_

`visit_expr` is probably easier. 

I think I just mainly restored this from the old salsa branch. I haven't thought much about how to handle dependencies. I should probably have marked this with a TODO. We can build it as part of the symbol table. My only concern is that it's important that extracting the dependencies is as cheap as possible, because we need the information early on to schedule checking of dependent files

An other option would be to build it as part of the `ast_ids` pass

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:487 on 2024-06-11 05:52_

I resolve this in favor of https://github.com/astral-sh/ruff/pull/11338#discussion_r1633473881

---

_@MichaReiser reviewed on 2024-06-11 05:52_

---

_@MichaReiser reviewed on 2024-06-11 05:54_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:577 on 2024-06-11 05:54_

To me these are kind of the least important types because it's mainly just boilerplate to make Salsa happy. However, I also expect that we'll probably have a `db` module in every Salsa-enabled crate rather than smashing all of this together into a single file. The way it is in this PR is just for fast prototyping.

---

_@MichaReiser reviewed on 2024-06-11 05:56_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic.rs`:574 on 2024-06-11 05:56_

The `Jar` definition lists all Salsa queries and ingredients (Salsa inputs, interned structs, or tracked structs). The way I think about it. Everything that's stored in Salsa, whether it is the cached results of a query, or some data structure that gets interned / stored in salsa.

---

_@MichaReiser reviewed on 2024-06-11 05:58_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:346 on 2024-06-11 05:58_

The `_` isn't about visibility as it is in Python (and pre-Typescript JS), but that the field is never read. Without the `_`, clippy complains that the field is only written to but never read. Removing the field would be incorrect because the whole trick relies on the fact that `Parsed` outlives the node. Adding the `_` makes clippy shut up, because it indicates that this is an intentional "unused"

---

_@MichaReiser reviewed on 2024-06-11 06:00_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/ast_ids.rs`:406 on 2024-06-11 06:00_

Because `Send` and `Sync` are `unsafe` traits. Rust normally implements these traits automatically for you if all the fields of your struct are `Send` or `Sync`. However, Rust can't do that for raw pointers, because it don't know what the guarantees are about the pointer. 

In our case, `Send` and `Sync` is safe because we never mutate the pointer nor the data they're referencing (our data structure is similar to an `Arc`)

---

_@MichaReiser reviewed on 2024-06-11 06:01_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/flow_graph.rs`:63 on 2024-06-11 06:01_

I don't know. I didn't spend much time thinking about visibility. I plan to do this when working on the "proper" Salsa migration.

---

_@MichaReiser reviewed on 2024-06-11 06:02_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types.rs`:45 on 2024-06-11 06:02_

Niko probably does a better job at it than I ever could https://salsa-rs.github.io/salsa/overview.html#id-fields

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types.rs`:125 on 2024-06-11 06:07_

What I understood is that we don't have a 1:1 mapping in case of lambda expressions and comprehensions. That's one reason why they're different.

The other reason is that salsa queries can only take Salsa ingredients (something stored in Salsa) as arguments, our `ScopeId`s aren't ingredient because they're created by pushing a value in an `IndexVec` which is more efficient than using a salsa interned struct which is always backed by a hash map.

I think it would make sense to use the same scope structure if we decide to build the symbol table per scope as well. Before then, I think it makes sense to keep them separate. 

---

_@MichaReiser reviewed on 2024-06-11 06:07_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types.rs`:112 on 2024-06-11 06:07_

I resolve this in favor of https://github.com/astral-sh/ruff/pull/11338#discussion_r1633601373

---

_@MichaReiser reviewed on 2024-06-11 06:07_

---

_@MichaReiser reviewed on 2024-06-11 06:08_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types.rs`:579 on 2024-06-11 06:08_

Yeah, agree. I think we probably want to have methods on the `TypeInferenceBuilder` because we only need to e.g. track the reverse map of already created unions back to their type ids during construction but we won't need it once type inference is complete (and we won't create any new types)

---

_@MichaReiser reviewed on 2024-06-11 06:12_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types.rs`:647 on 2024-06-11 06:12_

The enum is a lifetime hack. `infer_definition` takes `&self` as argument, so it can't intern a new union type. 

The fact that it is a readonly reference is important in `finish` where we iterate over `self.symbol_table`. 

```rust
        for symbol in self.symbol_table.symbol_ids_for_scope(self.enclosing_scope) {
            let definition_type = self.typing_context().infer_definitions(
                symbol_table
                    .definitions(symbol)
                    .iter()
                    .map(|definition| ReachableDefinition::Definition(*definition)),
                GlobalId::new(self.file, self.enclosing_scope),
            );

            public_symbol_types.insert(symbol, definition_type.into_type(&mut self.result));
        }
```

Taking a &mut wouldn't compile because Rust couldn't prove that the `symbol_table` doesn't get mutated (a method taking `&mut self` can mutate any field). By explicitly passing `&mut self.result` in `into_type` Rust can prove that `self.symbol_table` is never borrowed mutably 

---

_@MichaReiser reviewed on 2024-06-11 06:15_

---

_Review comment by @MichaReiser on `crates/red_knot/src/salsa_db/semantic/types/infer.rs`:161 on 2024-06-11 06:15_

Is your concern just about the `ImportDefinition` creation that is used as key? Because there's a difference. We associate a definition with its type. 

I don't think we can avoid this much without having a way to iterate over the AST and definitions at the same time. It may be nice to have a helper that, given a `StmtImport` generates the `ImportDefinition`s with their metadata that could be reused across the two implementations.

---

_@AlexWaygood reviewed on 2024-06-11 07:05_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/salsa_db/semantic/symbol_table.rs`:395 on 2024-06-11 07:05_

I see! That wasn't obvious to me here; maybe we could think about how to make that clearer so we don't have contributors coming along and proposing the "obvious" refactor ;)

---

_Closed by @MichaReiser on 2024-06-18 12:04_

---

_Branch deleted on 2024-08-12 07:53_

---
