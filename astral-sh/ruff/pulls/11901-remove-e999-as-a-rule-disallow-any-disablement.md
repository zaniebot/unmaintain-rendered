```yaml
number: 11901
title: "Remove `E999` as a rule, disallow any disablement methods for syntax error"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
  - breaking
assignees: []
merged: true
base: ruff-0.5
head: dhruv/syntax-error-2
created_at: 2024-06-17T11:57:20Z
updated_at: 2024-06-27T02:21:34Z
url: https://github.com/astral-sh/ruff/pull/11901
synced_at: 2026-01-12T15:55:39Z
```

# Remove `E999` as a rule, disallow any disablement methods for syntax error

---

_@dhruvmanila_

## Summary

This PR updates the way syntax errors are handled throughout the linter.

The main change is that it's now not considered as a rule which involves the following changes:
* Update `Message` to be an enum with two variants - one for diagnostic message and the other for syntax error message
* Provide methods on the new message enum to query information required by downstream usages

This means that the syntax errors cannot be hidden / disabled via any disablement methods. These are:
1. Configuration via `select`, `ignore`, `per-file-ignores`, and their `extend-*` variants
	```console
	$ cargo run -- check ~/playground/ruff/src/lsp.py --extend-select=E999 --no-preview --no-cache 
	    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
	     Running `target/debug/ruff check /Users/dhruv/playground/ruff/src/lsp.py --extend-select=E999 --no-preview --no-cache`
	warning: Rule `E999` is deprecated and will be removed in a future release. Syntax errors will always be shown regardless of whether this rule is selected or not.
	/Users/dhruv/playground/ruff/src/lsp.py:1:8: F401 [*] `abc` imported but unused
	  |
	1 | import abc
	  |        ^^^ F401
	2 | from pathlib import Path
	3 | import os
	  |
	  = help: Remove unused import: `abc`
	```
3. Command-line flags via `--select`, `--ignore`, `--per-file-ignores`, and their `--extend-*` variants
	```console
	$ cargo run -- check ~/playground/ruff/src/lsp.py --no-cache --config=~/playground/ruff/pyproject.toml 
	    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
	     Running `target/debug/ruff check /Users/dhruv/playground/ruff/src/lsp.py --no-cache --config=/Users/dhruv/playground/ruff/pyproject.toml`
	warning: Rule `E999` is deprecated and will be removed in a future release. Syntax errors will always be shown regardless of whether this rule is selected or not.
	/Users/dhruv/playground/ruff/src/lsp.py:1:8: F401 [*] `abc` imported but unused
	  |
	1 | import abc
	  |        ^^^ F401
	2 | from pathlib import Path
	3 | import os
	  |
	  = help: Remove unused import: `abc`
	```

This also means that the **output format** needs to be updated:
1. The `code`, `noqa_row`, `url` fields in the JSON output is optional (`null` for syntax errors)
2. Other formats are changed accordingly
For each format, a new test case specific to syntax errors have been added. Please refer to the snapshot output for the exact format for syntax error message.

The output of the `--statistics` flag will have a blank entry for syntax errors:
```
315     F821    [ ] undefined-name
119             [ ] syntax-error
103     F811    [ ] redefined-while-unused
```

The **language server** is updated to consider the syntax errors by convert them into LSP diagnostic format separately.

### Preview

There are no quick fixes provided to disable syntax errors. This will automatically work for `ruff-lsp` because the `noqa_row` field will be `null` in that case.
<img width="772" alt="Screenshot 2024-06-26 at 14 57 08" src="https://github.com/astral-sh/ruff/assets/67177269/aaac827e-4777-4ac8-8c68-eaf9f2c36774">

Even with `noqa` comment, the syntax error is displayed:
<img width="763" alt="Screenshot 2024-06-26 at 14 59 51" src="https://github.com/astral-sh/ruff/assets/67177269/ba1afb68-7eaf-4b44-91af-6d93246475e2">

Rule documentation page:
<img width="1371" alt="Screenshot 2024-06-26 at 16 48 07" src="https://github.com/astral-sh/ruff/assets/67177269/524f01df-d91f-4ac0-86cc-40e76b318b24">


## Test Plan

- [x] Disablement methods via config shows a warning
	- [x] `select`, `extend-select`
	- [ ] ~`ignore`~ _doesn't show any message_
	- [ ] ~`per-file-ignores`, `extend-per-file-ignores`~ _doesn't show any message_
- [x] Disablement methods via command-line flag shows a warning
	- [x] `--select`, `--extend-select`
	- [ ] ~`--ignore`~ _doesn't show any message_
	- [ ] ~`--per-file-ignores`, `--extend-per-file-ignores`~ _doesn't show any message_
- [x] File with syntax errors should exit with code 1
- [x] Language server
	- [x] Should show diagnostics for syntax errors
	- [x] Should not recommend a quick fix edit for adding `noqa` comment
	- [x] Same for `ruff-lsp`

resolves: #8447


---

_Renamed from "Avoid any disablement methods for syntax error diagnostics" to "Disable all disablement methods for syntax error diagnostics" by @dhruvmanila on 2024-06-18 09:51_

---

_Comment by @github-actions[bot] on 2024-06-18 12:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/3eee441edbd722831fb2a3bd041f85a9e4c893c5/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L106'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:106:12:</a> E999 SyntaxError: Multiple exception types must be parenthesized
- <a href='https://github.com/demisto/content/blob/3eee441edbd722831fb2a3bd041f85a9e4c893c5/Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py#L325'>Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py:325:8:</a> E999 SyntaxError: Multiple exception types must be parenthesized
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---

_Label `rule` added by @dhruvmanila on 2024-06-19 01:25_

---

_Label `breaking` added by @dhruvmanila on 2024-06-19 01:25_

---

_Added to milestone `v0.5.0` by @dhruvmanila on 2024-06-19 01:26_

---

_Marked ready for review by @dhruvmanila on 2024-06-19 01:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-19 07:39_

---

_Comment by @MichaReiser on 2024-06-19 08:16_

What happens if a user specifies `E999` in `select`, `ignore`, `exclude`? 

---

_Comment by @MichaReiser on 2024-06-19 08:17_

I wonder if it still makes sense to model `SyntaxError` as a rule, now that it no longer behaves like a rule. Is it just `Message` where we need to pass `E999` to? Could we use an `enum` in `Message`?

For example, I suspect that `E999` is still valid according to the schema?

---

_Comment by @dhruvmanila on 2024-06-19 10:10_

> What happens if a user specifies `E999` in `select`, `ignore`, `exclude`?

It shouldn't affect anything, the `E999` diagnostics (if any) will still be displayed.

> I wonder if it still makes sense to model `SyntaxError` as a rule, now that it no longer behaves like a rule. Is it just `Message` where we need to pass `E999` to? Could we use an `enum` in `Message`?

Good point. Let me check.

---

_Comment by @MichaReiser on 2024-06-19 10:17_

> It shouldn't affect anything, the E999 diagnostics (if any) will still be displayed.

But we don't show a warning. I'm worried that this is confusing for users that would expect `E999` not to show up

---

_Comment by @dhruvmanila on 2024-06-21 12:57_

> For example, I suspect that `E999` is still valid according to the schema?

Yeah, that's correct. I guess that doesn't make sense since it, now, doesn't behave as a rule (as you mentioned). Although I wonder how would one go about configuring the fixable part if and when we introduce them.

For some more context, Pyright doesn't seem to consider it as a rule either, at least that's what I'm inferring from the fact that there's no "rule" field in the JSON output:
```json
       {
            "file": "/Users/dhruv/playground/ruff/parser/_.py",
            "severity": "error",
            "message": "Expected expression to the right of \"=\"",
            "range": {
                "start": {
                    "line": 3,
                    "character": 17
                },
                "end": {
                    "line": 4,
                    "character": 0
                }
            }
        }
```
Vs the one with the "rule" field:
```json
       {
            "file": "/Users/dhruv/playground/ruff/src/lsp.py",
            "severity": "error",
            "message": "\"name\" is not defined",
            "range": {
                "start": {
                    "line": 9,
                    "character": 6
                },
                "end": {
                    "line": 9,
                    "character": 10
                }
            },
            "rule": "reportUndefinedVariable"
        }
```

I'm not sure how does Biome does it but they have a separate `ParseDiagnostic` and it's in a separate category ("parse") not present in the documentation.

---

_Comment by @MichaReiser on 2024-06-21 13:01_

> I'm not sure how does Biome does it but they have a separate ParseDiagnostic and it's in a separate category ("parse") not present in the documentation.

Biome supports different `Diagnostic`s and only Formatter and Linter diagnostics are suppressable. 

---

_Comment by @dhruvmanila on 2024-06-21 13:12_

> For example, I suspect that `E999` is still valid according to the schema?

I think we can just remove this code from the rule table so that it's not considered valid in config. But, that opens up another set of problems.

---

_Comment by @charliermarsh on 2024-06-23 15:12_

What's the motivation for making it unsuppressable?

---

_Comment by @MichaReiser on 2024-06-23 15:48_

There's some reasoning in the internal discord thread https://discord.com/channels/1039017663004942429/1082324250112823306/1250836504738267157

---

_Comment by @charliermarsh on 2024-06-23 15:51_

I don't see the motivation described in there, just more context on why it was added initially.

---

_Comment by @dhruvmanila on 2024-06-24 09:21_

Historically, I think the main motivation was because the parser didn't support new syntax which isn't the case with the new parser. We guarantee that the Ruff parser will always match the latest Python grammar. This means there's no need to suppress the syntax errors although there can be use cases in a test fixture for which the file should be exluded instead. This also means that we don't need to show the syntax errors as log messages in addition to the diagnostics.

**Other syntax errors:** In the future, Ruff will also start providing [syntax error diagnostics for unsupported syntax as per the target version](https://github.com/astral-sh/ruff/issues/6591) or the ones [raised by the compiler](https://github.com/astral-sh/ruff/issues/11934). Allowing to disable syntax errors means that Ruff won't be able to provide diagnostics for these kinds of syntax errors in case it's disabled. But, the syntax errors indicate that Python won't be able to run the code itself.

Looking at the **ecosystem**, the following tools support ignoring syntax errors:
* `flake8` via `noqa: E999`
* `pylint`
	We can't disable syntax errors in Pylint via comment:
	```py
	x =  # pylint: disable=syntax-error
	```
	But, we can via the config file:
	```toml
	[tool.pylint."MESSAGES CONTROL"]
	disable = ["syntax-error"]
	```

While, the following doesn't:
* Pyright: https://github.com/microsoft/pyright/blob/main/docs/configuration.md#main-configuration-options (typeCheckingMode)
    * Also, Pyright doesn't have any rule id corresponding to a syntax error
* `mypy`: https://mypy.readthedocs.io/en/stable/error_code_list.html#report-syntax-errors-syntax
* `biome`, under the "parse" category
	```js
	// biome-ignore parse: reason
	import fs from 'fs;
	```
* `eslint` (no rule id corresponding to syntax errors)
* `oxc`

**Searching for `E999` on https://grep.app gives the following relevant results:**
* Ignoring syntax errors from Pylint because Ruff will raise them 
    * `home-assistant/core`: https://github.com/home-assistant/core/blob/dev/pyproject.toml#L209
    * `ansible/molecule`: https://github.com/ansible/molecule/blob/fd3ed55b11bf896aa1892ee75a6f32761eed3032/pyproject.toml#L138C34-L138C35
* `codium-ai` is ignoring `E999` from Ruff; https://github.com/Codium-ai/pr-agent/blob/74bb07e9c40e735b47eed0d13b621e27a0f76bef/pyproject.toml#L75 although I cloned the repository and there are no syntax errors that Ruff is reporting when I removed it from the config
* `hatch` includes `E999` in the selection: https://github.com/pypa/hatch/blob/72e09428a2e6846962413b227534c0eb365a86ed/ruff_defaults.toml#L133
* `sphinx-gallery` has a usage within per-file-ignores (https://github.com/sphinx-gallery/sphinx-gallery/blob/0d7823073f9307cfeaad370ad41207354dfe9713/pyproject.toml#L113-L115) for which I think the file should be excluded completely
* Used to ignore a file containing `match` statements which we already support now: https://github.com/alice-biometrics/meiga/blob/4856023988835dc8c1cac6d3bdc72ca842ecb59c/pyproject.toml#L134-L136

**tl;dr** for the above search is that most usages are to include `E999` and to exclude syntax errors from Pylint.

---

_Comment by @AlexWaygood on 2024-06-24 13:05_

> **Other syntax errors:** In the future, Ruff will also start providing [syntax error diagnostics for unsupported syntax as per the target version](https://github.com/astral-sh/ruff/issues/6591) or the ones [raised by the compiler](https://github.com/astral-sh/ruff/issues/11934). Allowing to disable syntax errors means that Ruff won't be able to provide diagnostics for these kinds of syntax errors in case it's disabled. But, the syntax errors indicate that Python won't be able to run the code itself.

Some of these syntax errors emitted by the Python compiler are very subtle -- some have been the subject of several revisions at CPython (e.g. https://github.com/python/cpython/pull/100581).

From the perspective of Python users, these are all just `SyntaxError`s, so I think they should all be bucketed under E999; Ruff users shouldn't have to care about the distinction of where the SyntaxErrors originate from in the CPython internals. But if we get the logic wrong somehow for one of these rules that we plan to implement outside of the parser, making E999 unsuppressable might mean that users who encounter E999 false positives on their code are unable to upgrade their Ruff version until there's a release with a bugfix. That doesn't sound ideal to me.

---

_Comment by @charliermarsh on 2024-06-24 13:08_

> But if we get the logic wrong somehow for one of these rules that we plan to implement outside of the parser, making E999 unsuppressable might mean that users who encounter E999 false positives on their code are unable to upgrade their Ruff version until there's a release with a bugfix. That doesn't sound ideal to me.

You can always omit the file entirely via `exclude` -- it's effectively the same thing.

---

_Comment by @AlexWaygood on 2024-06-24 13:11_

> You can always omit the file entirely via `exclude` -- it's effectively the same thing.

but for the ones we plan on detecting outside of the parser especially, it feels like it would be nicer if we could move in the other direction to this PR, and support suppressing specific instances of E999 using `# noqa` comments. And even for the ones detected in the parser, that might make sense in some situations, given that our parser now supports error recovery?

---

_Comment by @MichaReiser on 2024-06-24 13:19_

The reason why I'm in favor of removing suppressions for `E999` is that there's almost never a good reason to suppress them (other than a bug in Ruff). This is considerably different from other lints where it is often fine to suppress them. Having this distinction seems useful to me. 

---

_Comment by @AlexWaygood on 2024-06-24 13:28_

I agree that this rule is in a different, distinct category to most (probably all) of our other rules. However, I don't think it necessarily follows that our users will benefit if we prevent them from suppressing rules in this other category. I'm not sure that the reasons for this restriction will necessarily be self-evident to users, and I doubt that having the restriction there will significantly help clarify the distinction between the categories in the minds of our users.

---

_Comment by @AlexWaygood on 2024-06-25 12:37_

(We discussed this internally and the TL;DR is that I'm now okay with this change. While I'd still prefer to have a world where there are as few special cases as possible in our configuration setup, the truth of the matter is that it's not possible to `# noqa` E999 on a speicfic line today, and it doesn't make much sense to try to allow that until and unless we have a parser and linter that are both resilient to syntax errors. If and when we have that, we could consider revisiting this special case in our configuration setup, but it'll probably be a while until we get there. Until we achieve those long-term goals, I agree that this PR gets us to a better place.)

---

_Converted to draft by @dhruvmanila on 2024-06-25 16:43_

---

_Comment by @dhruvmanila on 2024-06-25 16:44_

Putting this into draft for now. All changes are done locally. I'll write up the test cases, validate the behavior and push it tomorrow.

---

_Review request for @MichaReiser removed by @dhruvmanila on 2024-06-26 04:35_

---

_Renamed from "Disable all disablement methods for syntax error diagnostics" to "Remove `E999` as a rule, disallow any disablement methods for syntax error" by @dhruvmanila on 2024-06-26 09:35_

---

_Marked ready for review by @dhruvmanila on 2024-06-26 09:42_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-26 09:42_

---

_Assigned to @AlexWaygood by @dhruvmanila on 2024-06-26 09:42_

---

_Unassigned @AlexWaygood by @dhruvmanila on 2024-06-26 09:42_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-26 09:42_

---

_Comment by @MichaReiser on 2024-06-26 10:55_

I'll review the PR in detail in a minute, but I think we should wait to remove the `E999` rule. Instead, we should mark it as deprecated and, ideally, emit a warning when it is used in any of the selectors (maybe that's already done automatically when it is marked as deprecated)? 

The main reason is that we try to first deprecate a feature before completely removing it (gives users a nicer upgrade experience).

---

_Comment by @dhruvmanila on 2024-06-26 10:59_

> I'll review the PR in detail in a minute, but I think we should wait to remove the `E999` rule. Instead, we should mark it as deprecated and, ideally, emit a warning when it is used in any of the selectors (maybe that's already done automatically when it is marked as deprecated)?

Yeah, I just thought of that, I'm already on it.

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:83 on 2024-06-26 11:08_

Nit: `f.write_str(self.0.as_str())`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/github.rs`:53 on 2024-06-26 11:14_

Nit, maybe:

```rust
if let Some(rule) = message.rule() {
    write!(writer, " {code}" ,rule.noqa_code())?;
}

writeln!(writer, " {}", message.body())
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/json.rs`:82 on 2024-06-26 11:16_

Nit: I'm inclined to add a `code` and `url` method to `Message` that return `Option<String>` and `Option<Url>` to avoid the repetition in the different emitters. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:108 on 2024-06-26 11:19_

Uh, our parse errors only have a start offset and not a range. That's funny. 

---

_@MichaReiser approved on 2024-06-26 11:24_

This is great! I'm also like that `Message` is now an enum. I expect that we want to incorporate formatter errors in the future as well and it being an enum already moves in the right direction. 



---

_Comment by @MichaReiser on 2024-06-26 11:25_

The only functionality that we "loose" when merging this PR is that it is no longer possible to disable `E999` in the editor to avoid that both pyright and ruff show syntax errors. Is this something you plan to add to `ruff server` (and ruff lsp?)

Huh, RustRover's review feature isnt' there yet.... Submitting my review once would have been enough.

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-26 11:26_

---

_Comment by @dhruvmanila on 2024-06-26 11:40_

> The only functionality that we "loose" when merging this PR is that it is no longer possible to disable `E999` in the editor to avoid that both pyright and ruff show syntax errors. Is this something you plan to add to `ruff server` (and ruff lsp?)

It should be low effort if we want to add it to the new server. I don't think we should add any new "features" to `ruff-lsp`.

Any suggestions for the config name?
1. `lint.hide_syntax_errors`
2. `lint.exclude_syntax_errors`
3. `hide_syntax_errors` (global)
4. `exclude_syntax_errors` (global)

---

_Comment by @AlexWaygood on 2024-06-26 11:41_

> Any suggestions for the config name?

`lint.ignore_syntax_errors`?

---

_@dhruvmanila reviewed on 2024-06-26 11:47_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/message/mod.rs`:108 on 2024-06-26 11:47_

I'm not sure I follow here. This is mainly a copy-paste from the existing logic :)

---

_Comment by @MichaReiser on 2024-06-26 11:51_

I'm leaning towards a global option because we may want to always show (or not show) syntax error in the future even if linting is disabled. 

I'm leaning towards using `enable` because we already support a few `enable` options (and it's about enabling/disabling a LSP feature). I would not use the `ignore` semantics because it's too close to how rules are enabled/disabled. 

`syntax_errors.enable`? 

---

_Comment by @AlexWaygood on 2024-06-26 11:53_

Makes sense. `show_syntax_errors = true` / `show_syntax_errors = false`?

---

_Comment by @dhruvmanila on 2024-06-26 12:04_

> `syntax_errors.enable`?

This would be useful if we want to provide more options apart from enabling / disabling. I'm not sure what they would be right now and don't know if there's any other functionality we could provide related to syntax errors (maybe "fix" enabled or not?). I'm leaning towards what Alex has suggested.

---

_Comment by @dhruvmanila on 2024-06-26 12:20_

(I'm going to make the server changes in a follow-up PR.)

---

_Merged by @dhruvmanila on 2024-06-27 02:21_

---

_Closed by @dhruvmanila on 2024-06-27 02:21_

---

_Branch deleted on 2024-06-27 02:21_

---
