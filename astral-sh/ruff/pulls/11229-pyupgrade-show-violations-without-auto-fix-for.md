```yaml
number: 11229
title: "[`pyupgrade`] Show violations without auto-fix for `UP031`"
type: pull_request
state: merged
author: autinerd
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: up031-more-violations
created_at: 2024-05-01T08:39:50Z
updated_at: 2024-08-15T16:14:22Z
url: https://github.com/astral-sh/ruff/pull/11229
synced_at: 2026-01-12T15:55:37Z
```

# [`pyupgrade`] Show violations without auto-fix for `UP031`

---

_@autinerd_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This shows violations of the `UP031` rule which can't be auto-fixed, so that they can be fixed manually (similar to the pylint `consider-using-f-strings` rule)

## Test Plan

<!-- How was it tested? -->

Updated fixtures.


---

_Comment by @github-actions[bot] on 2024-05-01 08:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+134 -0 violations, +0 -0 fixes in 10 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L84'>examples/models/widgets.py:84:37:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L86'>examples/models/widgets.py:86:37:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L154'>src/bokeh/command/subcommands/file_output.py:154:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L984'>src/bokeh/command/subcommands/serve.py:984:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L678'>src/bokeh/models/sources.py:678:42:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L684'>src/bokeh/models/sources.py:684:42:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L699'>src/bokeh/models/sources.py:699:42:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/sources.py#L710'>src/bokeh/models/sources.py:710:46:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/selenium.py#L379'>tests/support/util/selenium.py:379:49:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_meta.py#L684'>securedrop/pretty_bad_protocol/_meta.py:684:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_meta.py#L689'>securedrop/pretty_bad_protocol/_meta.py:689:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_meta.py#L92'>securedrop/pretty_bad_protocol/_meta.py:92:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_parsers.py#L1388'>securedrop/pretty_bad_protocol/_parsers.py:1388:18:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_parsers.py#L1390'>securedrop/pretty_bad_protocol/_parsers.py:1390:22:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_parsers.py#L1454'>securedrop/pretty_bad_protocol/_parsers.py:1454:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_parsers.py#L903'>securedrop/pretty_bad_protocol/_parsers.py:903:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_util.py#L142'>securedrop/pretty_bad_protocol/_util.py:142:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/_util.py#L95'>securedrop/pretty_bad_protocol/_util.py:95:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/pretty_bad_protocol/gnupg.py#L559'>securedrop/pretty_bad_protocol/gnupg.py:559:17:</a> UP031 Use format specifiers instead of percent format
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/impala/tests/test_exprs.py#L135'>ibis/backends/impala/tests/test_exprs.py:135:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/impala/tests/test_exprs.py#L421'>ibis/backends/impala/tests/test_exprs.py:421:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/impala/tests/test_exprs.py#L477'>ibis/backends/impala/tests/test_exprs.py:477:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/backends/impala/tests/test_exprs.py#L514'>ibis/backends/impala/tests/test_exprs.py:514:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/ibis-project/ibis/blob/1d64e585dba093b1d030cdd5c280c6d55a1dc997/ibis/tests/expr/test_window_functions.py#L41'>ibis/tests/expr/test_window_functions.py:41:24:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/b492ec72ec82c74b477cf08c5b98f70643a7609c/mlflow/cli.py#L600'>mlflow/cli.py:600:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b492ec72ec82c74b477cf08c5b98f70643a7609c/mlflow/store/tracking/file_store.py#L494'>mlflow/store/tracking/file_store.py:494:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/mlflow/mlflow/blob/b492ec72ec82c74b477cf08c5b98f70643a7609c/mlflow/utils/autologging_utils/events.py#L66'>mlflow/utils/autologging_utils/events.py:66:17:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+57 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/misc/incremental_checker.py#L358'>misc/incremental_checker.py:358:15:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L1180'>mypy/build.py:1180:36:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L281'>mypy/build.py:281:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L2979'>mypy/build.py:2979:24:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L3219'>mypy/build.py:3219:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L3247'>mypy/build.py:3247:25:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L3294'>mypy/build.py:3294:35:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/build.py#L3347'>mypy/build.py:3347:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/dmypy/client.py#L597'>mypy/dmypy/client.py:597:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/dmypy/client.py#L599'>mypy/dmypy/client.py:599:15:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/main.py#L42'>mypy/main.py:42:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/memprofile.py#L78'>mypy/memprofile.py:78:11:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/memprofile.py#L85'>mypy/memprofile.py:85:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/messages.py#L3077'>mypy/messages.py:3077:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/nodes.py#L2866'>mypy/nodes.py:2866:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/report.py#L900'>mypy/report.py:900:15:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/semanal.py#L5711'>mypy/semanal.py:5711:23:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/server/mergecheck.py#L55'>mypy/server/mergecheck.py:55:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/server/mergecheck.py#L56'>mypy/server/mergecheck.py:56:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/server/update.py#L854'>mypy/server/update.py:854:32:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python/mypy/blob/8332c6e7992fa9655b79ef55c410a805ad1e6d34/mypy/stats.py#L372'>mypy/stats.py:372:22:</a> UP031 Use format specifiers instead of percent format
... 36 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/tools/profiling/sampler.py#L44'>tools/profiling/sampler.py:44:22:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/7b6d77f9e5591857619c8983b907370c7482a147/indico/web/http_api/hooks/base.py#L118'>indico/web/http_api/hooks/base.py:118:32:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/2fd9c8f48e564283cefac4d0f5a691683f4fc43e/notes-to-self/how-does-windows-so-reuseaddr-work.py#L52'>notes-to-self/how-does-windows-so-reuseaddr-work.py:52:19:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python-trio/trio/blob/2fd9c8f48e564283cefac4d0f5a691683f4fc43e/notes-to-self/how-does-windows-so-reuseaddr-work.py#L57'>notes-to-self/how-does-windows-so-reuseaddr-work.py:57:31:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/python-trio/trio/blob/2fd9c8f48e564283cefac4d0f5a691683f4fc43e/notes-to-self/how-does-windows-so-reuseaddr-work.py#L75'>notes-to-self/how-does-windows-so-reuseaddr-work.py:75:27:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+38 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/bench/empty.py#L5'>bench/empty.py:5:10:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/_code/code.py#L224'>src/_pytest/_code/code.py:224:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/_code/code.py#L310'>src/_pytest/_code/code.py:310:16:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/_io/pprint.py#L543'>src/_pytest/_io/pprint.py:543:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/_py/error.py#L72'>src/_pytest/_py/error.py:72:48:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/assertion/util.py#L409'>src/_pytest/assertion/util.py:409:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/assertion/util.py#L513'>src/_pytest/assertion/util.py:513:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/assertion/util.py#L523'>src/_pytest/assertion/util.py:523:13:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/cacheprovider.py#L391'>src/_pytest/cacheprovider.py:391:39:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/compat.py#L79'>src/_pytest/compat.py:79:20:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/compat.py#L80'>src/_pytest/compat.py:80:12:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/doctest.py#L355'>src/_pytest/doctest.py:355:21:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/fixtures.py#L878'>src/_pytest/fixtures.py:878:17:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pytest-dev/pytest/blob/38ad84bafd18d15ceff1960d636c693560337844/src/_pytest/main.py#L353'>src/_pytest/main.py:353:13:</a> UP031 Use format specifiers instead of percent format
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/4de696394016a749da84a436b65490bef6450808/src/pdm/cli/utils.py#L100'>src/pdm/cli/utils.py:100:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pdm-project/pdm/blob/4de696394016a749da84a436b65490bef6450808/src/pdm/cli/utils.py#L118'>src/pdm/cli/utils.py:118:26:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pdm-project/pdm/blob/4de696394016a749da84a436b65490bef6450808/src/pdm/cli/utils.py#L120'>src/pdm/cli/utils.py:120:30:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pdm-project/pdm/blob/4de696394016a749da84a436b65490bef6450808/src/pdm/cli/utils.py#L89'>src/pdm/cli/utils.py:89:29:</a> UP031 Use format specifiers instead of percent format
+ <a href='https://github.com/pdm-project/pdm/blob/4de696394016a749da84a436b65490bef6450808/src/pdm/cli/utils.py#L94'>src/pdm/cli/utils.py:94:29:</a> UP031 Use format specifiers instead of percent format
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP031 | 134 | 134 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-08-14 08:56_

---

_Comment by @MichaReiser on 2024-08-14 08:58_

I reverted the change to the diagnostic message because it floods the ecosystem changes, making it difficult to see the results related to unconditionally emitting the diagnostic.

---

_Comment by @MichaReiser on 2024-08-14 09:18_

The changes look reasonable to me but I would like a second opinion, especially if we should gate this behind preview first. @AlexWaygood wdyt?

---

_Comment by @AlexWaygood on 2024-08-14 10:55_

This seems reasonable to me too. For a stylistic/modernisation rule like this, it can be annoying if it has lots of complaints that can't be autofixed. However, the unfixable complaints here are relatively small in number: most complaints from this rule are still autofixed.

I'd also prefer if this was a preview-only change first, though, just to be on the safe side.

---

_Label `preview` added by @MichaReiser on 2024-08-14 11:15_

---

_Merged by @MichaReiser on 2024-08-14 11:59_

---

_Closed by @MichaReiser on 2024-08-14 11:59_

---

_Branch deleted on 2024-08-15 16:14_

---
