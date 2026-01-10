```yaml
number: 15888
title: "[flake8-pyi] Significantly improve accuracy of `PYI019` if preview mode is enabled"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: alex/pyi019-heuristics
created_at: 2025-02-02T22:51:29Z
updated_at: 2025-02-03T15:49:47Z
url: https://github.com/astral-sh/ruff/pull/15888
synced_at: 2026-01-10T19:57:22Z
```

# [flake8-pyi] Significantly improve accuracy of `PYI019` if preview mode is enabled

---

_Pull request opened by @AlexWaygood on 2025-02-02 22:51_

(This PR is stacked on top of #15885; review that first.)

## Summary

The logic `PYI019` uses to detect TypeVars that could be replaced with `Self` is flawed in several significant ways:
- The rule only looks at methods that have return-type annotations, but there are lots of methods that don't have return-type annotations that could still have their custom TypeVars replaced with `Self`.
- The rule assumes that if the first parameter has the same annotation as the return-type annotation in an instance method, the method will be using a TypeVar in its annotations. This heuristic is surprisingly accurate, but it's still a somewhat unprincipled heuristic. It means that the rule flags code like this as "using TypeVars that could be replaced with `Self`, when that's not true at all:
  ```py
  class _NotATypeVar: ...

  class Foo:
      def m(self: _NotATypeVar) -> _NotATypeVar: ...  # PYI019 flagged here
  ```

  Following @InSyncWithFoo's excellent work in https://github.com/astral-sh/ruff/pull/15821 to move this rule to be a bindings-based rule, there's now no longer any need to use heuristics like this; we can do much better.

Unfortunately, fixing these issues expands the scope of the rule (it will start flagging methods that it did not previously flag). This PR therefore applies these fixes only if preview mode is enabled. Otherwise, we continue to use the old heuristics that we always have.

Because the rule in preview mode can now apply to methods that do not have return-type annotations, we can no longer use the range of the return-type annotation as the range of the diagnostic. This PR therefore also changes the behaviour in preview mode so that we use the range of the "function header" for the diagnostic range (starting from the end of the function name, and ending either at the return-type annotation end (if there is a return-type annotation) or at the end of the function's parameters.

## Test Plan

- New snippets added to existing fixtures
- A new snapshot has been added to show what the behaviour is when the rule runs on `.py` files with preview mode enabled. (We already have a snapshot for `.pyi` files with stable mode, `.py` files with stable mode, and `.pyi` files with preview mode.)


---

_Label `rule` added by @AlexWaygood on 2025-02-02 22:51_

---

_Label `preview` added by @AlexWaygood on 2025-02-02 22:51_

---

_Comment by @github-actions[bot] on 2025-02-02 22:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+30 -0 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/9e5876dc1739dfca5c5ffd87719cb71bb2e20b91/superset/sql/parse.py#L167'>superset/sql/parse.py:167:21:</a> PYI019 Use `Self` instead of custom TypeVar `TBaseSQLStatement`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a68048ea026f09fc56e1a9963c489ff0beaae651/pandas/_libs/tslibs/timedeltas.pyi#L97'>pandas/_libs/tslibs/timedeltas.pyi:97:16:</a> PYI019 Use `Self` instead of custom TypeVar `_S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stdlib/_typeshed/__init__.pyi#L328'>stdlib/_typeshed/__init__.pyi:328:16:</a> PYI019 Use `Self` instead of custom TypeVar `Self`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stdlib/_typeshed/__init__.pyi#L330'>stdlib/_typeshed/__init__.pyi:330:24:</a> PYI019 Use `Self` instead of custom TypeVar `Self`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stdlib/typing.pyi#L960'>stdlib/typing.pyi:960:15:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_T`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stdlib/typing_extensions.pyi#L239'>stdlib/typing_extensions.pyi:239:15:</a> PYI019 Use `Self` instead of custom TypeVar `_T`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/PyYAML/yaml/representer.pyi#L28'>stubs/PyYAML/yaml/representer.pyi:28:24:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_R`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/PyYAML/yaml/representer.pyi#L30'>stubs/PyYAML/yaml/representer.pyi:30:30:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_R`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/internal/containers.pyi#L36'>stubs/protobuf/google/protobuf/internal/containers.pyi:36:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/internal/containers.pyi#L52'>stubs/protobuf/google/protobuf/internal/containers.pyi:52:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/internal/containers.pyi#L76'>stubs/protobuf/google/protobuf/internal/containers.pyi:76:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/internal/containers.pyi#L99'>stubs/protobuf/google/protobuf/internal/containers.pyi:99:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/message.pyi#L30'>stubs/protobuf/google/protobuf/message.pyi:30:21:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/message.pyi#L31'>stubs/protobuf/google/protobuf/message.pyi:31:23:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
+ <a href='https://github.com/python/typeshed/blob/aac4394eb29d86797628b57d6f01f5e17f5ff83f/stubs/protobuf/google/protobuf/message.pyi#L34'>stubs/protobuf/google/protobuf/message.pyi:34:19:</a> PYI019 [*] Use `Self` instead of custom TypeVar `_M`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/db/filtering.py#L1138'>rotkehlchen/db/filtering.py:1138:13:</a> PYI019 Use `Self` instead of custom TypeVar `T_EthSTakingFilterQ`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/db/filtering.py#L373'>rotkehlchen/db/filtering.py:373:15:</a> PYI019 Use `Self` instead of custom TypeVar `T_FilterQ`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/db/filtering.py#L866'>rotkehlchen/db/filtering.py:866:13:</a> PYI019 Use `Self` instead of custom TypeVar `T_HistoryBaseEntryFilterQ`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/history/events/structures/base.py#L372'>rotkehlchen/history/events/structures/base.py:372:39:</a> PYI019 Use `Self` instead of custom TypeVar `T`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L101'>rotkehlchen/utils/mixins/enums.py:101:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L126'>rotkehlchen/utils/mixins/enums.py:126:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 Use `Self` instead of custom TypeVar `T`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L151'>rotkehlchen/utils/mixins/enums.py:151:28:</a> PYI019 Use `Self` instead of custom TypeVar `D`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L172'>rotkehlchen/utils/mixins/enums.py:172:28:</a> PYI019 Use `Self` instead of custom TypeVar `D`
+ <a href='https://github.com/rotki/rotki/blob/7273a75e2e848662f8b1d75cdbc3446234414137/rotkehlchen/utils/mixins/enums.py#L77'>rotkehlchen/utils/mixins/enums.py:77:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/42bc59b8027a074b31054f5fc0f1e6440fdd5252/zerver/data_import/slack.py#L109'>zerver/data_import/slack.py:109:18:</a> PYI019 Use `Self` instead of custom TypeVar `SlackBotEmailT`
+ <a href='https://github.com/zulip/zulip/blob/42bc59b8027a074b31054f5fc0f1e6440fdd5252/zerver/lib/thumbnail.py#L55'>zerver/lib/thumbnail.py:55:20:</a> PYI019 Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/d39444a9abddd745c99f5fcc85be3e350dc8b27b/src/trio/testing/_fake_net.py#L103'>src/trio/testing/_fake_net.py:103:29:</a> PYI019 Use `Self` instead of custom TypeVar `T_UDPEndpoint`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/d81c3f9b4cbc4d71ff3bf00a079b574c21aa8b22/astropy/cosmology/_src/core.py#L485'>astropy/cosmology/_src/core.py:485:26:</a> PYI019 Use `Self` instead of custom TypeVar `_FlatCosmoT`
+ <a href='https://github.com/astropy/astropy/blob/d81c3f9b4cbc4d71ff3bf00a079b574c21aa8b22/astropy/cosmology/_src/flrw/base.py#L1705'>astropy/cosmology/_src/flrw/base.py:1705:16:</a> PYI019 Use `Self` instead of custom TypeVar `_FlatFLRWMixinT`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 30 | 30 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2025-02-02 23:11_

> `ruff-ecosystem` results

These all look like true positives from what I can see 🥳

---

_Comment by @MichaReiser on 2025-02-03 14:36_

Would you mind rebasing the PR? The diff is a bit chaotic otherwise.

---

_Comment by @AlexWaygood on 2025-02-03 14:57_

> Would you mind rebasing the PR? The diff is a bit chaotic otherwise.

done!

---

_Comment by @MichaReiser on 2025-02-03 15:06_

This is another case where diagnsotics with multiple ranges were really nice CC: @BurntSushi 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:150 on 2025-02-03 15:13_

This one will be hard to find because it won't show up when searching for `preview.is_enabled` or `preview.is_disabled` (which I normally use). 

It makes me wonder if we should start doing something similar to the formatter where we have a `preview.rs` file and each preview feature defines a `is_feature_name_enabled(&settings)`. It makes it very easy to see all current preview behaviors and promoting it is as easy as removing the function -> Rust will tell you exactly where you have to make changes. 

However, this will likely mean that you'd have to change this match to inline the preview check into the match arms. Although I'd kind of prefer this anyway because it reduces the number of lines that need changing when promoting the feature (the match header remains unchanged).

```rust
match (function_kind, returns.as_deref() {
	(FunctionType::Function | FunctionType::StaticMethod, _) => return None,
	(FunctionType::ClassMethod, returns) => {
		if is_more_precise_custom_type_var_for_self_enabled(settings) {
			...
		} else if let Some(returns) = returns {
			...
		}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:606 on 2025-02-03 15:21_

```suggestion
```suggestion
    if let [sole_typevar] == parameters.as_slice() {
        return (sole_typevar.range() == custom_typevar.range())
            .then(|| Edit::range_deletion(type_params.range));
    }
```

---

_@MichaReiser approved on 2025-02-03 15:23_

This is excellent. Thank you

---

_@AlexWaygood reviewed on 2025-02-03 15:40_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:150 on 2025-02-03 15:40_

I've switched to use `preview.is_enabled()`. It's not pretty... but I'm consoling myself that we'll be able to get rid of it soonish 😆

---

_Merged by @AlexWaygood on 2025-02-03 15:45_

---

_Closed by @AlexWaygood on 2025-02-03 15:45_

---

_Branch deleted on 2025-02-03 15:45_

---

_@MichaReiser reviewed on 2025-02-03 15:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:150 on 2025-02-03 15:46_

Oh no, I hoped you start using the preview feature functions... I'll think of you when I have to promote the behavior change 🤣 

---

_@AlexWaygood reviewed on 2025-02-03 15:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:150 on 2025-02-03 15:49_

Ohh sorry lol. You can give this one to me when the time comes 🤣

---
