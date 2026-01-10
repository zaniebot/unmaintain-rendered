```yaml
number: 9613
title: "[`ruff`] Detect unnecessary `dict` comprehensions for iterables (`RUF025`)"
type: pull_request
state: merged
author: mikeleppane
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-RUF025
created_at: 2024-01-22T12:27:04Z
updated_at: 2024-01-24T02:23:42Z
url: https://github.com/astral-sh/ruff/pull/9613
synced_at: 2026-01-10T22:57:09Z
```

# [`ruff`] Detect unnecessary `dict` comprehensions for iterables (`RUF025`)

---

_Pull request opened by @mikeleppane on 2024-01-22 12:27_

## Summary

Checks for unnecessary `dict` comprehension when creating a new dictionary from iterable. Suggest to replace with `dict.fromkeys(iterable)`

See: https://github.com/astral-sh/ruff/issues/9592

## Test Plan

```bash
cargo test
```


---

_Comment by @mikeleppane on 2024-01-22 13:06_

### Remark
I'm getting the following error when running tests locally with ```cargo test```: 
```bash
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/flake8_executable/snapshots/ruff_linter__rules__flake8_executable__tests__EXE001_1.py.snap
Snapshot: EXE001_1.py
Source: crates/ruff_linter/src/rules/flake8_executable/mod.rs:43
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
    0       │-EXE001_1.py:1:1: EXE001 Shebang is present but file is not executable␊
    1       │-  |␊
    2       │-1 | #!/usr/bin/python␊
    3       │-  | ^^^^^^^^^^^^^^^^^ EXE001␊
    4       │-2 | ␊
    5       │-3 | if __name__ == '__main__':␊
    6       │-  |␊
────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'rules::flake8_executable::tests::rules::path_new_exe001_1_py_expects' panicked at /home/mikko/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.34.0/src/runtime.rs:563:9:
snapshot assertion for 'EXE001_1.py' failed in line 43
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    rules::flake8_executable::tests::rules::path_new_exe001_1_py_expects
    rules::flake8_executable::tests::rules::path_new_exe002_1_py_expects
```
Is this a known issue or should I do something differently?


### Suggestion 

Have you considered using [cargo-make](https://sagiegurari.github.io/cargo-make/)? It could be used to execute e.g. fmt, clippy and tests in one task.




---

_Comment by @github-actions[bot] on 2024-01-22 13:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+28 -0 violations, +0 -0 fixes in 11 projects; 32 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/ext/commands/flag_converter.py#L314'>disnake/ext/commands/flag_converter.py:314:28:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/evaluation/marker_stats.py#L114'>rasa/core/evaluation/marker_stats.py:114:51:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/core/featurizers/single_state_featurizer.py#L119'>rasa/core/featurizers/single_state_featurizer.py:119:20:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/core/featurizers/test_precomputation.py#L110'>tests/core/featurizers/test_precomputation.py:110:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5c4364aafe57f089746a833d96a76c68c8869e20/helm_tests/airflow_aux/test_annotations.py#L409'>helm_tests/airflow_aux/test_annotations.py:409:63:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/file_output.py#L145'>src/bokeh/command/subcommands/file_output.py:145:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/command/subcommands/serve.py#L828'>src/bokeh/command/subcommands/serve.py:828:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/58174e5e19fb22ba48ad4722953fc88428292e11/ibis/backends/dask/tests/execution/conftest.py#L71'>ibis/backends/dask/tests/execution/conftest.py:71:20:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/ibis-project/ibis/blob/58174e5e19fb22ba48ad4722953fc88428292e11/ibis/common/egraph.py#L768'>ibis/common/egraph.py:768:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/632797c490f8750119de85e8f14f13f2a12cfc93/src/build/__main__.py#L37'>src/build/__main__.py:37:14:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/c0e4b16ab9dde5dab19f7b6cf2d21e13f58058f0/cibuildwheel/util.py#L558'>cibuildwheel/util.py:558:24:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/api/rest.py#L4472'>rotkehlchen/api/rest.py:4472:28:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L290'>rotkehlchen/chain/ethereum/modules/convex/decoder.py:290:17:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L291'>rotkehlchen/chain/ethereum/modules/convex/decoder.py:291:27:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L768'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:768:62:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L772'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:772:24:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/ethereum/modules/uniswap/v3/decoder.py#L551'>rotkehlchen/chain/ethereum/modules/uniswap/v3/decoder.py:551:16:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/evm/decoding/gitcoin/decoder.py#L266'>rotkehlchen/chain/evm/decoding/gitcoin/decoder.py:266:65:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/evm/decoding/gitcoin/decoder.py#L269'>rotkehlchen/chain/evm/decoding/gitcoin/decoder.py:269:21:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/evm/decoding/gitcoin/decoder.py#L272'>rotkehlchen/chain/evm/decoding/gitcoin/decoder.py:272:21:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/evm/decoding/interfaces.py#L249'>rotkehlchen/chain/evm/decoding/interfaces.py:249:37:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/optimism/modules/velodrome/decoder.py#L247'>rotkehlchen/chain/optimism/modules/velodrome/decoder.py:247:62:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/rotki/rotki/blob/0ea98b07af3370dcedab8c196e495743d662b9b9/rotkehlchen/chain/optimism/modules/velodrome/decoder.py#L251'>rotkehlchen/chain/optimism/modules/velodrome/decoder.py:251:24:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/030e6c2abb685a74b176dad50dbdc3c612b9f73f/skbuild/setuptools_wrap.py#L521'>skbuild/setuptools_wrap.py:521:22:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/scikit-build/scikit-build/blob/030e6c2abb685a74b176dad50dbdc3c612b9f73f/skbuild/setuptools_wrap.py#L524'>skbuild/setuptools_wrap.py:524:19:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/b2ea03037631c084f774bf3843c08f9ea3c5f58e/src/scikit_build_core/_logging.py#L102'>src/scikit_build_core/_logging.py:102:14:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/8559032d3ab37dc71d8265479b019226e1d28286/zerver/tests/test_message_fetch.py#L3373'>zerver/tests/test_message_fetch.py:3373:23:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
+ <a href='https://github.com/zulip/zulip/blob/8559032d3ab37dc71d8265479b019226e1d28286/zproject/backends.py#L168'>zproject/backends.py:168:31:</a> RUF025 [*] Unnecessary dict comprehension for iterable; use `dict.fromkeys` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF025 | 28 | 28 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-22 15:56_

---

_Label `rule` added by @charliermarsh on 2024-01-22 15:56_

---

_Label `preview` added by @charliermarsh on 2024-01-22 15:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:35 on 2024-01-23 10:12_

nit: we could use an enum instead of a boolean. A boolean value is hard to read until it is linked with some context like a variable name. An enum can prove to be readable in such cases.
```rust
enum Value {
	// A `None` literal
	NoneLiteral,
	// Any value other than `None`
	Other
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:43 on 2024-01-23 10:15_

I believe this must be a leftover and can be removed now?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:56 on 2024-01-23 10:15_

I believe this must be a leftover and can be removed now?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:80 on 2024-01-23 10:17_

nit: can we combine these comments and use bullet points instead?
```rust
// Don't suggest `dict.fromkeys` for
// - ..., because ...
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:86 on 2024-01-23 10:22_

nit: we can use the `is_` method on the node: `generator.target.is_name_expr`

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:129 on 2024-01-23 10:27_

Is there a specific reason you choose to use `libcst` nodes instead of the [AST nodes](https://github.com/astral-sh/ruff/blob/993e605c6c57786acd836599a37f9e7c442b237b/crates/ruff_python_ast/src/nodes.rs) to construct the fixed node? I might be wrong but I think it might be easier to construct the node using the AST nodes with the `Generator` to generate the equivalent source code. I'm fine with either but just trying to understand this.

---

_@dhruvmanila approved on 2024-01-23 10:34_

Looks pretty solid, thanks for working on this!

I've left some minor nits and one question about the fix generation but otherwise it looks good to go.

---

_@mikeleppane reviewed on 2024-01-23 12:51_

---

_Review comment by @mikeleppane on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:129 on 2024-01-23 12:51_

No, specific reason. I was not quite sure what would be the correct way to construct the fixed node. Seems that using AST node with `Generator` would be more reasonable. 

---

_@mikeleppane reviewed on 2024-01-23 12:53_

---

_Review comment by @mikeleppane on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:35 on 2024-01-23 12:53_

Yea, I thought about using an Enum but it kind of felt slightly over board for such a simple thing.

---

_Comment by @mikeleppane on 2024-01-23 12:54_

@dhruvmanila thanks for the good comments! 👍 

---

_@charliermarsh approved on 2024-01-24 01:48_

Thanks!

---

_@charliermarsh reviewed on 2024-01-24 01:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:77 on 2024-01-24 01:49_

@mikeleppane - I ended up using `is_constant` here with doesn't include `Expr::Name`, so that changed some of the test case results.

For example, we no longer flag this:

```python
def func():
    values = ["a", "b", "c"]
    [{n: values for n in [1,2,3]}] # RUF025
```

But I think we're correct not to flag, since in that case, `values` is actually mutable.

---

_@charliermarsh reviewed on 2024-01-24 01:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:77 on 2024-01-24 01:50_

Oh wait, hmm, but the user is _already_ sharing it between elements. What you had is probably correct then.

---

_@charliermarsh reviewed on 2024-01-24 01:56_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_dict_comprehension_for_iterable.rs`:77 on 2024-01-24 01:56_

Refining back.

---

_Renamed from "[RUF] - Add unnecessary dict comprehension for iterable rule (RUF025)" to "[`ruff`] Detect unnecessary `dict` comprehensions for iterables (`RUF025`)" by @charliermarsh on 2024-01-24 02:05_

---

_Merged by @charliermarsh on 2024-01-24 02:15_

---

_Closed by @charliermarsh on 2024-01-24 02:15_

---
