```yaml
number: 9464
title: "`--show-settings` displays active settings in a far more readable format"
type: pull_request
state: merged
author: snowsignal
labels:
  - cli
assignees: []
merged: true
base: main
head: jane/settings/display-improvements
created_at: 2024-01-11T09:20:59Z
updated_at: 2024-01-12T19:36:12Z
url: https://github.com/astral-sh/ruff/pull/9464
synced_at: 2026-01-10T22:57:09Z
```

# `--show-settings` displays active settings in a far more readable format

---

_Pull request opened by @snowsignal on 2024-01-11 09:20_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #8334.

`Display` has been implemented for `ruff_workspace::Settings`, which gives a much nicer and more readable output to `--show-settings`. 

Internally, a `display_settings` utility macro has been implemented to reduce the boilerplate of the display code.

### Work to be done

- [x] A lot of formatting for `Vec<_>` and `HashSet<_>` types have been stubbed out, using `Debug` as a fallback. There should be a way to add generic formatting support for these types as a modifier in `display_settings`.
- [x] Several complex types were also stubbed out and need proper `Display` implementations rather than falling back on `Debug`.
- [x] An open question needs to be answered: how important is it that the output be valid TOML? Some types in settings, such as a hash-map from a glob pattern to a multi-variant enum, will be hard to rework into valid _and_ readable TOML.
- [x] Tests need to be implemented.

## Test Plan

Tests consist of a snapshot test for the default `--show-settings` output and a doctest for `display_settings!`.


---

_Label `cli` added by @MichaReiser on 2024-01-11 09:36_

---

_Comment by @github-actions[bot] on 2024-01-11 09:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L666'>src/bokeh/core/serialization.py:666:34:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/serialization.py#L666'>src/bokeh/core/serialization.py:666:34:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/registry/rule_set.rs`:286 on 2024-01-11 13:06_

Would `f.debug_list` be useful here? 

Not that this part is performance sensitive but we can avoid allocating a string for each rule by using a for loop

```rust
write!(f, "[\n\t")?;

for rule in self.iter() {
	write!(f, "\"{rule:?}\"")?;
}

write!(f, "\n]")
```

Joking: We could use our formatter infrastructure to print the rules on a single line if they fit into an 80-character line length and split them when they do not :laughing: 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/categorize.rs`:405 on 2024-01-11 13:09_

Nit: Same as above. A traditional for loop can help to avoid the inner `format!` call (which allocates a string, if Rust isn't clever enough to optimise it away)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/mod.rs`:106 on 2024-01-11 13:11_

I think some documentation would help to demystify the macro :) (what are the supported options)

---

_@MichaReiser reviewed on 2024-01-11 13:18_

Nice work! 

I've left a few nit comments that are up to you if you want to address them. 

My main question is what convention we want to use for the field names. 

The option names in the YAML configuration and the CLI use kebab case: `extend-ignore`. 

I see reasons for using the same as well as for using different names than what we use in the configuration:

* Same names: More familiar to users
* Different names: Makes it clear that these are the resolved settings which doesn't map 1:1 to the YAML or CLI configuration. For example, the resolved settings can have fewer or more fields than the options, the values are merged from different settings (or sections) etc. 

 
Before you publish your PR. Could you expand your test plan with an example output? 

---

_Review comment by @BurntSushi on `crates/ruff_formatter/src/lib.rs`:119 on 2024-01-11 13:28_

For calls to `write!` that just print out the `Display` of a single argument like this, you can just do `Display::fmt(&self.0, f)` directly. (Technically `self.0.fmt(f)` can also work, but IIRC there are cases where that call is ambiguous with `Debug::fmt`. In which case, you'd get a compile error. Not sure if that's the case here though, so I tend to just write it out as `Display::fmt(...)`.)

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/mod.rs`:158 on 2024-01-11 13:33_

Oh this isn't nearly as bad as I thought it was going to be based on your comments in Discord. :P I'm totally cool with this.

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/mod.rs`:107 on 2024-01-11 13:36_

I might be tempted to rename `namespace` to `prefix`, since it seems to usually end with a `.`. (This is a small thing.)

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/mod.rs`:107 on 2024-01-11 13:37_

Or I wonder, could the `.` be inserted unconditionally in this macro?

I would guess maybe not, for cases where the namespace is empty?

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_import_conventions/settings.rs`:54 on 2024-01-11 13:38_

Should this have a `.` at the end?

---

_@BurntSushi reviewed on 2024-01-11 13:45_

Wow, that's a lot of `Display` impls! I agree that your `display_settings!` macro here is pretty effective.

Very nice work. :)

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/mod.rs`:107 on 2024-01-11 15:07_

Alternatively, we could make the namespace/prefix an optional parameter and then append `.` if a namespace is specified.

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/mod.rs`:158 on 2024-01-11 15:09_

That's probably because you're seeing the revised version :P

Thanks though, I'm glad this is workable and understandable as a solution!

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/flake8_import_conventions/settings.rs`:54 on 2024-01-11 15:10_

Good catch!

---

_Review comment by @snowsignal on `crates/ruff_linter/src/registry/rule_set.rs`:286 on 2024-01-11 15:19_

> Would `f.debug_list` be useful here?

Not really since it formats everything on one line 😞 

Thanks for the suggestion! I was using an allocating `format!` + `.join` to avoid a trailing comma, but apparently trailing commas are valid toml anyway, so my bad 🤷‍♀️ 

---

_@snowsignal reviewed on 2024-01-11 15:20_

---

_@BurntSushi reviewed on 2024-01-11 15:53_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/settings/mod.rs`:107 on 2024-01-11 15:53_

I defer to you on what to do here. :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/registry/rule_set.rs`:282 on 2024-01-11 15:55_

Any world in which this should be using the display representation rather than the debug representation for `Rule`? (I actually don't know what each of them prints :))

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_copyright/settings.rs`:38 on 2024-01-11 15:56_

Maybe a dumb question, but to confirm my understanding, if `| debug` is required-but-omitted for a type, do you just get a compile error (e.g., that the type doesn't implement `Display`)?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:603 on 2024-01-11 15:59_

Should this have a TODO? Or is it intentionally empty?

---

_@charliermarsh reviewed on 2024-01-11 16:03_

This is great work! Big improvement.

>  An open question needs to be answered: how important is it that the output be valid TOML? Some types in settings, such as a hash-map from a glob pattern to a multi-variant enum, will be hard to rework into valid and readable TOML.

In my opinion, this is a nice-to-have but not critical, since the representation isn't going to match the representation that users have in their own `pyproject.toml` or `ruff.toml` file (since `Settings` is an internal representation, rather than the user-facing representation).


---

_@snowsignal reviewed on 2024-01-11 16:07_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/flake8_copyright/settings.rs`:38 on 2024-01-11 16:07_

Right, it would fail to compile with the exact error you mentioned.

---

_@snowsignal reviewed on 2024-01-11 16:10_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/registry/rule_set.rs`:282 on 2024-01-11 16:10_

`Rule` doesn't have a `Display` implementation, and I thought the `Debug` implementation looked good as it was! (though I did add some quotes)

---

_Review comment by @snowsignal on `crates/ruff_linter/src/settings/types.rs`:603 on 2024-01-11 16:10_

My bad, that should have a TODO.

---

_@snowsignal reviewed on 2024-01-11 16:10_

---

_Comment by @codspeed-hq[bot] on 2024-01-11 19:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InquisitivePenguin:jane/settings/display-improvements)

### Merging #9464 will **degrade performances by 4.35%**

<sub>Comparing <code>InquisitivePenguin:jane/settings/display-improvements</code> (a24a50a) with <code>main</code> (fee64b5)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/InquisitivePenguin:jane/settings/display-improvements)._

### Benchmarks breakdown

|     | Benchmark | `main` | `InquisitivePenguin:jane/settings/display-improvements` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @charliermarsh on 2024-01-11 20:00_

(Feel free to ignore the perf regression - you may need to merge / rebase on main, or otherwise we may be seeing some flakiness (in other PRs too).)

---

_Comment by @snowsignal on 2024-01-11 20:10_

I'll merge from `main` just in case 🙂 

---

_Marked ready for review by @snowsignal on 2024-01-11 22:09_

---

_Review comment by @charliermarsh on `crates/ruff_cli/tests/snapshots/show_settings__display_default_settings.snap`:17 on 2024-01-12 00:20_

Nit: might be nice to visually separate the two lines above (which are meta-information about the settings) from the settings themselves. Maybe adding a comment above the `cache_dir` line, like `Top-level Settings` or similar? Just an idea, open to better solutions.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/mod.rs`:40 on 2024-01-12 00:24_

Very nice.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/mod.rs`:393 on 2024-01-12 00:24_

Perhaps makes sense as an associated method on `PerFileIgnores`, now that the struct exists? But not blocking feedback.

---

_@charliermarsh approved on 2024-01-12 00:24_

This is great work!

---

_@snowsignal reviewed on 2024-01-12 00:30_

---

_Review comment by @snowsignal on `crates/ruff_cli/tests/snapshots/show_settings__display_default_settings.snap`:17 on 2024-01-12 00:30_

Good idea! I might do something like `# General Settings` if that works.

---

_Review comment by @MichaReiser on `crates/ruff_cli/tests/snapshots/show_settings__display_default_settings.snap`:214 on 2024-01-12 07:03_

Nit: I think some empty lines could help readability by making the comments stand out more and clearly indicating where sections end.
```suggestion
Settings path: "[BASEPATH]/pyproject.toml"

# General Settings
cache_dir = "[BASEPATH]/.ruff_cache"
fix = false
fix_only = false
output_format = text
show_fixes = false
show_source = false
unsafe_fixes = hint

# File Resolver Settings
file_resolver.exclude = [
	".bzr",
	".direnv",
	".eggs",
	".git",
	".git-rewrite",
	".hg",
	".ipynb_checkpoints",
	".mypy_cache",
	".nox",
	".pants.d",
	".pyenv",
	".pytest_cache",
	".pytype",
	".ruff_cache",
	".svn",
	".tox",
	".venv",
	".vscode",
	"__pypackages__",
	"_build",
	"buck-out",
	"build",
	"dist",
	"node_modules",
	"site-packages",
	"venv",
]
file_resolver.extend_exclude = [
	"crates/ruff_linter/resources/",
	"crates/ruff_python_formatter/resources/",
]
file_resolver.force_exclude = false
file_resolver.include = [
	"*.py",
	"*.pyi",
	"**/pyproject.toml",
]
file_resolver.extend_include = []
file_resolver.respect_gitignore = true
file_resolver.project_root = "[BASEPATH]"

# Linter Settings
linter.exclude = []
linter.project_root = "[BASEPATH]"
linter.rules.enabled = [
	MultipleImportsOnOneLine,
	ModuleImportNotAtTopOfFile,
	MultipleStatementsOnOneLineColon,
	MultipleStatementsOnOneLineSemicolon,
	UselessSemicolon,
	NoneComparison,
	TrueFalseComparison,
	NotInTest,
	NotIsTest,
	TypeComparison,
	BareExcept,
	LambdaAssignment,
	AmbiguousVariableName,
	AmbiguousClassName,
	AmbiguousFunctionName,
	IOError,
	SyntaxError,
	UnusedImport,
	ImportShadowedByLoopVar,
	UndefinedLocalWithImportStar,
	LateFutureImport,
	UndefinedLocalWithImportStarUsage,
	UndefinedLocalWithNestedImportStarUsage,
	FutureFeatureNotDefined,
	PercentFormatInvalidFormat,
	PercentFormatExpectedMapping,
	PercentFormatExpectedSequence,
	PercentFormatExtraNamedArguments,
	PercentFormatMissingArgument,
	PercentFormatMixedPositionalAndNamed,
	PercentFormatPositionalCountMismatch,
	PercentFormatStarRequiresSequence,
	PercentFormatUnsupportedFormatCharacter,
	StringDotFormatInvalidFormat,
	StringDotFormatExtraNamedArguments,
	StringDotFormatExtraPositionalArguments,
	StringDotFormatMissingArguments,
	StringDotFormatMixingAutomatic,
	FStringMissingPlaceholders,
	MultiValueRepeatedKeyLiteral,
	MultiValueRepeatedKeyVariable,
	ExpressionsInStarAssignment,
	MultipleStarredExpressions,
	AssertTuple,
	IsLiteral,
	InvalidPrintSyntax,
	IfTuple,
	BreakOutsideLoop,
	ContinueOutsideLoop,
	YieldOutsideFunction,
	ReturnOutsideFunction,
	DefaultExceptNotLast,
	ForwardAnnotationSyntaxError,
	RedefinedWhileUnused,
	UndefinedName,
	UndefinedExport,
	UndefinedLocal,
	UnusedVariable,
	UnusedAnnotation,
	RaiseNotImplemented,
]
linter.rules.should_fix = [
	MultipleImportsOnOneLine,
	ModuleImportNotAtTopOfFile,
	MultipleStatementsOnOneLineColon,
	MultipleStatementsOnOneLineSemicolon,
	UselessSemicolon,
	NoneComparison,
	TrueFalseComparison,
	NotInTest,
	NotIsTest,
	TypeComparison,
	BareExcept,
	LambdaAssignment,
	AmbiguousVariableName,
	AmbiguousClassName,
	AmbiguousFunctionName,
	IOError,
	SyntaxError,
	UnusedImport,
	ImportShadowedByLoopVar,
	UndefinedLocalWithImportStar,
	LateFutureImport,
	UndefinedLocalWithImportStarUsage,
	UndefinedLocalWithNestedImportStarUsage,
	FutureFeatureNotDefined,
	PercentFormatInvalidFormat,
	PercentFormatExpectedMapping,
	PercentFormatExpectedSequence,
	PercentFormatExtraNamedArguments,
	PercentFormatMissingArgument,
	PercentFormatMixedPositionalAndNamed,
	PercentFormatPositionalCountMismatch,
	PercentFormatStarRequiresSequence,
	PercentFormatUnsupportedFormatCharacter,
	StringDotFormatInvalidFormat,
	StringDotFormatExtraNamedArguments,
	StringDotFormatExtraPositionalArguments,
	StringDotFormatMissingArguments,
	StringDotFormatMixingAutomatic,
	FStringMissingPlaceholders,
	MultiValueRepeatedKeyLiteral,
	MultiValueRepeatedKeyVariable,
	ExpressionsInStarAssignment,
	MultipleStarredExpressions,
	AssertTuple,
	IsLiteral,
	InvalidPrintSyntax,
	IfTuple,
	BreakOutsideLoop,
	ContinueOutsideLoop,
	YieldOutsideFunction,
	ReturnOutsideFunction,
	DefaultExceptNotLast,
	ForwardAnnotationSyntaxError,
	RedefinedWhileUnused,
	UndefinedName,
	UndefinedExport,
	UndefinedLocal,
	UnusedVariable,
	UnusedAnnotation,
	RaiseNotImplemented,
]
linter.per_file_ignores = {}
linter.safety_table.forced_safe = []
linter.safety_table.forced_unsafe = []
linter.target_version = Py37
linter.preview = disabled
linter.explicit_preview_rules = false
linter.extension.mapping = {}
linter.allowed_confusables = []
linter.builtins = []
linter.dummy_variable_rgx = ^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$
linter.external = []
linter.ignore_init_module_imports = false
linter.logger_objects = []
linter.namespace_packages = []
linter.src = ["[BASEPATH]"]
linter.tab_size = 4
linter.line_length = 88
linter.task_tags = [
	TODO,
	FIXME,
	XXX,
]
linter.typing_modules = []
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/tests/snapshots/show_settings__display_default_settings.snap`:221 on 2024-01-12 07:05_

Nit: Maybe start a new section here 
```suggestion

# Linter Plugins
linter.flake8_annotations.mypy_init_return = false
linter.flake8_annotations.suppress_dummy_args = false
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/tests/snapshots/show_settings__display_default_settings.snap`:348 on 2024-01-12 07:05_

Nit
```suggestion
linter.pyupgrade.keep_runtime_typing = false

# Formatter Settings
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:565 on 2024-01-12 07:07_

Nice, I love new type wrappers :) 

---

_@MichaReiser approved on 2024-01-12 07:09_

So many display implementations :laughing: 

This is excellent. I prefer to have some empty lines between the sections, but I'll leave the decision up to you. Let me (or anyone else around at the time) know when it is ready to merge.

---

_Review comment by @konstin on `crates/ruff_linter/src/settings/mod.rs`:137 on 2024-01-12 09:49_

I'd go for `none` or `None` here, it's what rust (`Option::None`) and python (`None`) use.

```suggestion
                None        => writeln!($fmt, "none")?
```

---

_Review comment by @konstin on `crates/ruff_linter/src/settings/types.rs`:566 on 2024-01-12 09:54_

This could use a comment what the two glob matchers are

---

_@konstin approved on 2024-01-12 09:54_

Great work!

---

_Comment by @charliermarsh on 2024-01-12 19:30_

Rock on

---

_Merged by @charliermarsh on 2024-01-12 19:30_

---

_Closed by @charliermarsh on 2024-01-12 19:30_

---

_Branch deleted on 2024-01-12 19:31_

---
