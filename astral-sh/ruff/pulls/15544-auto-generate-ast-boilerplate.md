```yaml
number: 15544
title: Auto-generate AST boilerplate
type: pull_request
state: merged
author: dcreager
labels:
  - internal
assignees: []
merged: true
base: main
head: dcreager/generate-ast
created_at: 2025-01-17T01:38:16Z
updated_at: 2025-01-17T19:23:05Z
url: https://github.com/astral-sh/ruff/pull/15544
synced_at: 2026-01-10T20:34:00Z
```

# Auto-generate AST boilerplate

---

_Pull request opened by @dcreager on 2025-01-17 01:38_

This PR replaces most of the hard-coded AST definitions with a generation script, similar to what happens in `rust_python_formatter`.  I've replaced every "rote" definition that I could find, where the content is entirely boilerplate and only depends on what syntax nodes there are and which groups they belong to.

This is a pretty massive diff, but it's entirely a refactoring.  It should make absolutely no changes to the API or implementation.  In particular, this required adding some configuration knobs that let us override default auto-generated names where they don't line up with types that we created previously by hand.

## Test plan

There should be no changes outside of the `rust_python_ast` crate, which verifies that there were no API changes as a result of the auto-generation.  Aggressive `cargo clippy` and `uvx pre-commit` runs after each commit in the branch.

---

_Label `internal` added by @dcreager on 2025-01-17 01:38_

---

_Comment by @github-actions[bot] on 2025-01-17 01:46_

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
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
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
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/ast.toml`:31 on 2025-01-17 03:44_

Should this be called "rustdoc" or "doc" instead to indicate that this is the documentation for the generated enum?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 03:49_

Maybe we could make this a standalone script that can be run by uv?

```py
# /// script
# requires-python = ">=3.11"
# dependencies = []
# ///
```

This requires 3.11 because of `tomllib` library usage.

---

_@dhruvmanila approved on 2025-01-17 03:51_

This is great, thanks for doing this!

Unrelated to this PR, Biome does something similar (https://github.com/biomejs/biome/tree/main/xtask/codegen).

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-01-17 03:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:4 on 2025-01-17 08:15_

```suggestion
# these nodes belong to groups. For instance, there is a `Stmt` group
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:14 on 2025-01-17 08:15_

Is it intentional that you insert two spaces after each `.`?
```suggestion
# Each group is defined by two sections below. The `[GROUP]` section defines
# options that control the auto-generation for that group. The `[GROUP.nodes]`
# section defines which syntax nodes belong to that group. The name of each
# entry in the nodes section must match the name of the corresponding Rust
# struct. The value of each entry defines options that control the
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:28 on 2025-01-17 08:19_

I find the code a bit hard to read because it mixes top level statement with class and declarations. I'd prefer to instead have a `main` function that's called in `if __name__ == "__main__"`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:77 on 2025-01-17 08:23_

I'd use one of:

```py
out = [
    textwrap.dedent("""
        // This is a generated file. Don't modify it by hand!
        // Run `crates/ruff_python_ast/generate.py` to re-generate the file.
    """)
]


out = [
    textwrap.dedent(
        """
        // This is a generated file. Don't modify it by hand!
        // Run `crates/ruff_python_ast/generate.py` to re-generate the file.
        """
    )
]
```

Or use the auto formatted code. IMO, it's roughly equally esthetic as the manually formatted array

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:83 on 2025-01-17 08:24_

I think each of those blocks could be functions instead where the inline comment becomes a docstring. 

It would also be nice if the docstring could contain. code snippet of what it generates:

Generates the owned enum for each group, e.g `Stmt`, `Expr` etc. For each, generate:

* The enum
* Implement `From<Variant>`
* Implement `Ranged`
* 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/generated.rs`:35 on 2025-01-17 08:28_

Not for this PR but I think it would be nice if we generated the `is` implementations as well. The `is` macro doesn't always work well with ide's and macros are also more expensive to compile.

---

_@MichaReiser approved on 2025-01-17 08:31_

Nice, this is great. 

I think my long term preference would be to use something like ungrammar that would also allow us to e.g. generate the source order visitor but I think this is a great start.


---

_@AlexWaygood reviewed on 2025-01-17 10:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/ast.toml`:1 on 2025-01-17 10:42_

Is it worth adding a small CI job that checks that `generated.rs` is up to date with what this script would output? Or would we just quickly get alerted by compiler errors if we forgot to run the script to update the file?

I guess maybe the more likely way that things could go wrong would be if a contributor tried to edit `generated.rs` directly and we didn't notice they were updating a generated file, and merged it. Then their edits would just get overwritten the next time somebody ran `generate.py`, and it might be pretty confusing when the tests they added started failing

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:1 on 2025-01-17 10:50_

Oh yeah, I forgot to mention that. We could integrate it into https://github.com/astral-sh/ruff/blob/f319531632ecb683051891637be56ec58d470993/.github/workflows/ci.yaml#L377-L396

---

_@MichaReiser reviewed on 2025-01-17 10:50_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:9 on 2025-01-17 10:53_

nit: could you run Ruff on this file with its isort rules enabled? (Sorry! 😄)

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:45 on 2025-01-17 10:54_

```suggestion
    def __init__(self, group_name: str, group: dict[str, Any]) -> None:
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:43 on 2025-01-17 10:54_

since the script can only run with Python 3.11+ (due to `tomllib` usage), let's use the new-style syntax for unions with `None`

```suggestion
    comment: str | None
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:63 on 2025-01-17 10:55_

```suggestion
    def __init__(self, group: Group, node_name: str, node: dict[str, Any]) -> None:
```

---

_@AlexWaygood reviewed on 2025-01-17 10:55_

Some Python nitpicks ;)

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:1 on 2025-01-17 13:29_

Yes good idea!  Done

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:14 on 2025-01-17 13:30_

Old habits die hard, I guess :sweat_smile: 

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:9 on 2025-01-17 13:32_

Sure!  I was relying on however `ruff` is run via `uvx pre-commit run --all-files`.  Is this an option we want to turn on globally for the pre-commit hook?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 13:35_

Could you add the file to .gitattributes and mark it as generated so that it is by default?

https://github.com/astral-sh/ruff/blob/47c9ed07f2c00ac0d600294a0cb418d91c13b4ea/.gitattributes#L17

---

_@MichaReiser reviewed on 2025-01-17 13:35_

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 13:50_

It would be `generated.rs` that gets marked as generated, not the Python script, right?

I had considered this, but thought that it might be useful (in future PRs at least) to see changes to the generated code in diffs.  But maybe that will be too verbose, given the number of syntax node types, and (per your other comment) if the docstring comments include descriptions of what changes?

---

_Review comment by @dcreager on `crates/ruff_python_ast/ast.toml`:31 on 2025-01-17 13:53_

Done

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 13:55_

Done

---

_@dcreager reviewed on 2025-01-17 13:59_

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 14:00_

Done.  I also added `rust_python_formatter`'s generated file, and added the `-diff` attribute so that these files don't show up in local `git diff` output either.

---

_@dcreager reviewed on 2025-01-17 14:00_

---

_@AlexWaygood reviewed on 2025-01-17 14:00_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:9 on 2025-01-17 14:00_

> Is this an option we want to turn on globally for the pre-commit hook?

That makes sense to me! I think import sorting is pretty uncontroversial

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:1 on 2025-01-17 14:06_

>  But maybe that will be too verbose, given the number of syntax node types, and (per your other comment) if the docstring comments include descriptions of what changes?

You can still see the changes, it's just that the files are collapsed by default.

---

_@MichaReiser reviewed on 2025-01-17 14:06_

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:9 on 2025-01-17 14:20_

Done.  I marked the `red_knot_vendored` crate as being skipped, since that is code we bring in from elsewhere and not under our control

---

_@dcreager reviewed on 2025-01-17 14:20_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:85 on 2025-01-17 14:21_

Would it be better to put this config in the `tool.ruff` section of our `pyproject.toml` file at the directory root?

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:87 on 2025-01-17 14:21_

These files should already be excluded from all pre-commit hooks

https://github.com/astral-sh/ruff/blob/4fdf8af7473a1e55f0bc38cd762da6e4a958df70/.pre-commit-config.yaml#L3-L17

---

_@AlexWaygood reviewed on 2025-01-17 14:22_

---

_Review comment by @dcreager on `.pre-commit-config.yaml`:85 on 2025-01-17 14:25_

Yes it would!  Done

---

_Review comment by @dcreager on `.pre-commit-config.yaml`:87 on 2025-01-17 14:25_

Ah okay, now I see what I was seeing — the actually vendored part of that crate is already being excluded.  But `knot_extensions` isn't vendored, we wrote that.  So the new lint regression is something I should accept and add to this PR.

---

_Review requested from @carljm by @dcreager on 2025-01-17 14:26_

---

_Review requested from @sharkdp by @dcreager on 2025-01-17 14:26_

---

_@dcreager reviewed on 2025-01-17 14:26_

---

_@calumy reviewed on 2025-01-17 14:29_

---

_Review comment by @calumy on `.pre-commit-config.yaml`:85 on 2025-01-17 14:29_

It might be worth considering moving the [ruff config in the scripts dir](https://github.com/astral-sh/ruff/blob/main/scripts/pyproject.toml) into the root dir `pyproject.toml`. Isort and some other common rules are already enabled there, so it may be useful to enable them for the whole project if scripts are being added outwith the scripts dir.

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:28 on 2025-01-17 18:28_

Done

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:77 on 2025-01-17 18:28_

This is moot as part of reorganizing into functions.

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:83 on 2025-01-17 18:29_

Done

---

_@dcreager reviewed on 2025-01-17 18:30_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/generate.py`:81 on 2025-01-17 18:46_

nit: I would personally add return annotations to all functions, even those which just return `None`

```suggestion
def write_preamble(out: list[str]) -> None:
```

but I know some find this just needless boilerplate. No strong feeling! :-)

---

_@AlexWaygood approved on 2025-01-17 18:50_

LGTM. I agree with @calumy's comment at https://github.com/astral-sh/ruff/pull/15544#discussion_r1920276383, but that could be done as a followup!

---

_@dcreager reviewed on 2025-01-17 19:03_

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:81 on 2025-01-17 19:03_

Done!  (I must be too used to Rust, where that's the default :sweat_smile:)

---

_Merged by @dcreager on 2025-01-17 19:23_

---

_Closed by @dcreager on 2025-01-17 19:23_

---

_Branch deleted on 2025-01-17 19:23_

---
