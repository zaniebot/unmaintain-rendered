```yaml
number: 12338
title: "[red-knot] Add notebook support"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: notebook-support
created_at: 2024-07-15T15:45:18Z
updated_at: 2024-07-17T08:40:51Z
url: https://github.com/astral-sh/ruff/pull/12338
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Add notebook support

---

_Pull request opened by @MichaReiser on 2024-07-15 15:45_

## Summary

This PR adds notebook support to red knot. 

The way notebooks are handled in Ruff is that it parses the notebook and concatenates the content of all cells to a single string. The concatenated string is then passed to the linter or formatter. Reported diagnostics are mapped back to their origin cells by using a source map. 

This PR doesn't implement any of the fancy re-mapping logic but adds support for reading notebooks into a structured representation. This requires two additions:

* A new `System::read_to_notebook` method that reads a system path to a `Notebook`. The motivation for this method is that the LSP uses a structured notebook representation (slightly different from the representation used by `ruff_notebook`) that's cheaper to convert to a `ruff_notebook::Notebook` then reparsing the notebook from source.
* I renamed `SourceCode` to `SourceFile` and changed it to use an internal enum that is either `Text` or `Notebook` to account for the fact that a file can either be a text or notebook.


## Open questions

I'm not too happy with the name `SourceFile`. I'm open for better suggestions but I also don't want to block the PR on finding the perfect name (the query is used very sparsely and a rename is quickly done)


## Cell and embedded file support

I intentionally excluded support for cell-based queries or a design for supporting embedded files (e.g. a doctest block in a python file). I think we could support both and a way of doing it could be to:

* Make `SourceFile` a salsa tracked struct
* Add additional `Cell` and `Embedded` variants that either store another salsa tracked struct, or at least store the origin file id and an index to identify the cell or embed in the file. 

Methods that can operate on any source-file would be changed to accept a `SourceFile` instead of a `File` argument. However, it's unclear if we want this:

* The only use case for a cell-level granular query today is to format a single cell. Formatting a single cell is so fast that it doesn't justifies creating a query for it. 
* It's unclear if we have many queries that can operate on any file content or if this is more a one-off, in which case `Cell` and `Embed` could be salsa tracked structs and we define `format_cell` and `parse_cell` etc. queries rather than having one generic `parse` query.
* An alternative is to introduce a new salsa tracked on top of `SourceFile` with the variants `File`, `Embed`, and `Cell`. 

## Test Plan

I added a notebook file to my test folder and verified that Red Knot runs the lint rules on its content.

---

_Label `red-knot` added by @MichaReiser on 2024-07-15 15:45_

---

_Comment by @github-actions[bot] on 2024-07-15 16:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2024-07-16 11:59_

---

_Review requested from @carljm by @MichaReiser on 2024-07-16 11:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-16 11:59_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-16 11:59_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/source.rs`:22 on 2024-07-16 13:18_

Couldn't you do this more simply with the existing API, than with the `try_from_extension()` API you're adding in this PR? If you apply the below suggestion, you won't need to make any changes to the `ruff_python_ast` crate as part of this PR.

```suggestion
        if path.extension().is_some_and(|extension| {
            PySourceType::from_extension(extension).is_ipynb()
        }) {
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/source.rs`:48 on 2024-07-16 13:33_

I'm not sure this is a particularly helpful comment. It feels slightly tautological (of course a `SourceFile` represents a "source file" -- but that's not so helpful if you're not sure what's meant by the phrase "source file" in the first place). I'm also not sure it's entirely accurate: to me, "source file" would imply "a file that is known to contain valid Python source code". But it seems like it's possible to create a `ruff_db::files::File` instance that points to a file that doesn't contain valid Python source code, and I don't think there's anything preventing me from calling `source_file()` on that file, in which case `source_file` would return a `SourceFile` instance that doesn't contain valid Python source code. There's also no documentation anywhere indicating that this would be a misuse of the API.

I'd find something like this more helpful:

```suggestion
/// Representation of the contents of the file.
/// Internally, this may be stored either as raw text or as a notebook.
```

---

_@AlexWaygood approved on 2024-07-16 13:34_

With the caveats that I've never used notebooks much when working with Python, and that I'm also not particularly familiar with Ruff's existing notebook support, this overall LGTM. Thanks for the detailed PR summary, it was really helpful!

> I'm not too happy with the name `SourceFile`. I'm open for better suggestions but I also don't want to block the PR on finding the perfect name (the query is used very sparsely and a rename is quickly done)

This is definitely the biggest issue I have with the PR; I also don't much like `SourceFile`. Perhaps `FileContents`? I agree it doesn't need to block this PR, but I do think naming is important, or it will become harder to understand these abstractions when we come back to them later

---

_Comment by @MichaReiser on 2024-07-16 13:41_

> This is definitely the biggest issue I have with the PR; I also don't much like SourceFile. Perhaps FileContents?

I strongly prefer a less generic name because I think this API shouldn't be used to read any non-Python files (or at least any non-source files). For example, if we consider using the' File' API for this in the future, we should use `File.read_to_string` to read `pth` files. 

Edit: I'm also open to keeping the name `SourceCode`. It just made some documentation rather awkward. Source code that's a notebook? 

---

_Comment by @AlexWaygood on 2024-07-16 14:14_

> I strongly prefer a less generic name because I think this API shouldn't be used to read any non-Python files (or at least any non-source files). For example, if we consider using the' File' API for this in the future, we should use `File.read_to_string` to read `pth` files.

I see. This changes my understanding of that function, since:
- My understanding of the `File` struct from the name of the structs and the comments in the source code is that it is an abstract representation of an arbitrary file: not necessarily a Python file.
- The `source_text()` function accepts an arbitrary `File` object
- The `source_text()` function was previously documented as simply being a function that simply "reads the contents of a file".

If `source_text()`/`source_file()` is only meant to be used to read files that we expect to contain Python source code, I think that should be documented better in the docstring for that function.

If your concern is that `FileContents` would be too generic as a name, maybe `SourceFileContents`? I also don't mind `SourceCode` too much, even with this change! Agree it's slightly awkard, but it feels more expressive to me than `SourceFile`. Fundamentally it feels to me like this struct represents what the file _contains_ rather than the file itself. You cannot retrieve the file's metadata, its path, etc., from this struct: only the contents of the file.

---

_Comment by @AlexWaygood on 2024-07-16 19:26_

Note that we've already been using (for a few weeks) `source_text()` to read the source of a file that doesn't contain Python source code: typeshed's `VERSIONS` file: https://github.com/astral-sh/ruff/blob/9a2dafb43d36b62d45a604dea58224a0884cd6e5/crates/red_knot_module_resolver/src/typeshed/versions.rs#L72-L79

---

_@carljm approved on 2024-07-16 23:17_

This looks reasonable to me!

I don't have strong feelings about the naming.

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/source.rs`:119 on 2024-07-17 06:08_

Ignore if this isn't relevant but I was wondering if there's any reason for not using the `SourceKind` or borrowing the name:

https://github.com/astral-sh/ruff/blob/9a2dafb43d36b62d45a604dea58224a0884cd6e5/crates/ruff_linter/src/source_kind.rs#L19-L25

---

_@dhruvmanila reviewed on 2024-07-17 06:08_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:119 on 2024-07-17 06:13_

I wasn't aware of it. There are too many `PySourceType` and kind types haha. 

The only reason right now is that it's in `ruff_linter` and we can't provide all it's API (e.g. we don't want `from_path`)

---

_@MichaReiser reviewed on 2024-07-17 06:13_

---

_@dhruvmanila reviewed on 2024-07-17 06:17_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:228 on 2024-07-17 06:17_

Technically, an empty notebook would contain at least a single empty cell. I verified that by creating an empty notebook in VS Code and Jupyter Lab. The use-case for this seems to be only when there's some error when creating a Ruff notebook in Red Knot. I think it make sense to use this representation (one empty code cell) for an empty notebook.

---

_@dhruvmanila approved on 2024-07-17 06:18_

---

_@MichaReiser reviewed on 2024-07-17 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:22 on 2024-07-17 08:01_

Yes, that would be possible. But I'm going to introduce the API anyway and I don't like the `PySourceType` default fallback. 

---

_Comment by @MichaReiser on 2024-07-17 08:19_

I reverted the name back to `SourceCode`, changed `versions` to use `file.read_to_string`, and ensured that `Notebook::empty` contains an empty code cell. 

---

_Merged by @MichaReiser on 2024-07-17 08:26_

---

_Closed by @MichaReiser on 2024-07-17 08:26_

---

_Branch deleted on 2024-07-17 08:26_

---
