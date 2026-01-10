```yaml
number: 21931
title: "[`flake8-bugbear`] Allow `B901` in pytest hook wrappers"
type: pull_request
state: open
author: assadyousuf
labels:
  - rule
  - preview
assignees: []
base: main
head: fix-b901-pytest-hookimpl-wrapper
created_at: 2025-12-12T04:00:22Z
updated_at: 2025-12-17T16:24:17Z
url: https://github.com/astral-sh/ruff/pull/21931
synced_at: 2026-01-10T16:42:11Z
```

# [`flake8-bugbear`] Allow `B901` in pytest hook wrappers

---

_Pull request opened by @assadyousuf on 2025-12-12 04:00_


## Summary
Stop raising return-in-generator with pytest hook wrappers (@hookimpl(wrapper=True)). They are specifically designed to use this pattern: https://docs.pytest.org/en/stable/how-to/writing_hook_functions.html#hook-wrappers-executing-around-other-hooks

Before: return-in-generator reports would surface with pytest hook wrappers

After: specifically check for pytest hook wrappers before reporting return-in-generator 

Testing:
Wrote some tests to cover different cases

---

_Label `rule` added by @ntBre on 2025-12-12 18:27_

---

_Label `preview` added by @ntBre on 2025-12-12 18:27_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B901.py`:3 on 2025-12-17 16:02_

Thanks for updating this! This kind of thing is a bit fragile, though. Would you mind instead using something like a `# B901` comment on each of the lines that should trigger the diagnostic?

That should allow us to delete this line, which will also offset the new pytest import and avoid churn in the snapshot line numbers.

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 16:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -24 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/.github/scripts/authors_in_cff.py#L40'>.github/scripts/authors_in_cff.py:40:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `pathlib.Path("CITATION.cff").read_text()`
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/.github/scripts/citation_updater.py#L69'>.github/scripts/citation_updater.py:69:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `citation_rst_file.write_text(citation_rst_text)`
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/docs/_author_list_from_cff.py#L193'>docs/_author_list_from_cff.py:193:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `pathlib.Path(rst_file).write_text(authors_rst, encoding="utf-8")`
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/docs/_global_substitutions.py#L217'>docs/_global_substitutions.py:217:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `pathlib.Path(rst_file).write_text(content, encoding="utf-8")`
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/data/test_downloader.py#L191'>tests/utils/data/test_downloader.py:191:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `filepath.write_text("Not data")`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/partners/chroma/langchain_chroma/vectorstores.py#L489'>libs/partners/chroma/langchain_chroma/vectorstores.py:489:14:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(uri).read_bytes()`
- <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/standard-tests/langchain_tests/conftest.py#L70'>libs/standard-tests/langchain_tests/conftest.py:70:14:</a> FURB101 [*] `Path.open()` followed by `read()` can be replaced by `cassette_path.read_bytes()`
- <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/standard-tests/langchain_tests/conftest.py#L88'>libs/standard-tests/langchain_tests/conftest.py:88:14:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `cassette_path.write_bytes(data)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/ast/ast.py#L88'>src/scikit_build_core/ast/ast.py:88:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(sys.argv[1]).read_text(encoding="utf-8-sig")`
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/ast/tokenizer.py#L77'>src/scikit_build_core/ast/tokenizer.py:77:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(sys.argv[1]).read_text(encoding="utf-8-sig")`
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/metadata/regex.py#L58'>src/scikit_build_core/metadata/regex.py:58:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(input_filename).read_text(encoding="utf-8")`
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L274'>tests/test_dynamic_metadata.py:274:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("__init__.py").write_text("__version__ = '0.1.0'")`
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L290'>tests/test_dynamic_metadata.py:290:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("version.hpp")....`
- <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L326'>tests/test_dynamic_metadata.py:326:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("version.hpp")....`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/158b2a603253b4099d4943b3efa31b33e4fbe908/indico/vendor/django_mail/message.py#L369'>indico/vendor/django_mail/message.py:369:14:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `path.read_bytes()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/faulthandler.py#L102'>src/_pytest/faulthandler.py:102:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/helpconfig.py#L152'>src/_pytest/helpconfig.py:152:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/setuponly.py#L36'>src/_pytest/setuponly.py:36:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/warnings.py#L109'>src/_pytest/warnings.py:109:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/warnings.py#L118'>src/_pytest/warnings.py:118:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/warnings.py#L128'>src/_pytest/warnings.py:128:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/warnings.py#L89'>src/_pytest/warnings.py:89:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/src/_pytest/warnings.py#L98'>src/_pytest/warnings.py:98:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
- <a href='https://github.com/pytest-dev/pytest/blob/2fb64da82c83d4a485b3b2626ab8b39ede340d43/testing/conftest.py#L91'>testing/conftest.py:91:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B901 | 9 | 0 | 9 | 0 | 0 |
| FURB103 | 8 | 0 | 8 | 0 | 0 |
| FURB101 | 7 | 0 | 7 | 0 | 0 |

</p>
</details>





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/helpers.rs`:80 on 2025-12-17 16:16_

We have a helper for this kind of thing:

https://github.com/astral-sh/ruff/blob/0f373603ebd007196af4f211e14d421024a6b947/crates/ruff_python_ast/src/nodes.rs#L3317-L3318

and a smaller one for the boolean literal:

https://github.com/astral-sh/ruff/blob/0f373603ebd007196af4f211e14d421024a6b947/crates/ruff_python_ast/src/helpers.rs#L675-L676

I think we may want to accept more arguments here, though. I checked `help(pytest.hookimpl)` in the REPL, and this is the signature:

```
class HookimplMarker(builtins.object)
 |  __call__(
 |      self,
 |      function: _F | None = None,
 |      hookwrapper: bool = False,
 |      optionalhook: bool = False,
 |      tryfirst: bool = False,
 |      trylast: bool = False,
 |      specname: str | None = None,
 |      wrapper: bool = False
 |  ) -> _F | Callable[[_F], _F]
```

so it seems that `wrapper` could technically also be passed positionally. I also think the `hookwrapper` argument has a similar function, although it's referred to as the "old-style hook wrapper" in the [pluggy docs](https://pluggy.readthedocs.io/en/stable/index.html#wrappers), which the pytest docs redirected me to.

If we decide to accept either `wrapper` or `hookwrapper` and look for them positionally too, we could do something like:

```rust
let hookwrapper = arguments.find_argument_value("hookwrapper", 1);
let wrapper = arguments.find_argument_value("wrapper", 6);
```

and then process the returned `Option`s.

I'm assuming that most usage in the wild will be with a keyword `wrapper` argument, though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/mod.rs`:7 on 2025-12-17 16:17_

nit: I'd probably omit this and just import straight from `helpers` in the rule file.

---

_@ntBre reviewed on 2025-12-17 16:22_

Thank you! This looks great to me overall. I just had a couple of small suggestions besides the snapshots needing to be updated.

---

_Renamed from "Fixes #21881: return-in-generator (B901) - should not be reported on pytest hook wrappers" to "[`flake8-bugbear`] Allow `B901` in pytest hook wrappers" by @ntBre on 2025-12-17 16:24_

---
