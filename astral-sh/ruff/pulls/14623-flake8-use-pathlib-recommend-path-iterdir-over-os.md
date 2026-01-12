```yaml
number: 14623
title: "[`flake8-use-pathlib`] Recommend `Path.iterdir()` over `os.scandir()` (`PTH209`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
base: main
head: PTH209
created_at: 2024-11-27T04:38:57Z
updated_at: 2024-11-27T13:11:21Z
url: https://github.com/astral-sh/ruff/pull/14623
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-use-pathlib`] Recommend `Path.iterdir()` over `os.scandir()` (`PTH209`)

---

_@InSyncWithFoo_

## Summary

Per discussion at #14509.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-27 04:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e4f758196277336b9f1209f2e20d0c5dafc3912a/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L315'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:315:18:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_ext.py#L69'>tests/unit/bokeh/test_ext.py:69:19:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/_shutil.py#L93'>src/scikit_build_core/_shutil.py:93:10:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/build/_pathutil.py#L23'>src/scikit_build_core/build/_pathutil.py:23:18:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH209 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @dhruvmanila on 2024-11-27 06:17_

---

_Label `preview` added by @dhruvmanila on 2024-11-27 06:17_

---

_Comment by @AlexWaygood on 2024-11-27 08:54_

How does `Path.iterdir` compare in terms of performance? I realise that most of the `flake8-use-pathlib` rules risk suggesting changes that might make your code slower, but I believe this may be particularly bad. See the [docs for `Path.scandir`](https://docs.python.org/3.14/library/pathlib.html#pathlib.Path.scandir), which has been added in Python 3.14 (which is still in the alpha period):

> Using [scandir()](https://docs.python.org/3.14/library/pathlib.html#pathlib.Path.scandir) instead of [iterdir()](https://docs.python.org/3.14/library/pathlib.html#pathlib.Path.iterdir) can significantly increase the performance of code that also needs file type or file attribute information, because [os.DirEntry](https://docs.python.org/3.14/library/os.html#os.DirEntry) objects expose this information if the operating system provides it when scanning a directory.

It might at least be worth mentioning in the docs for this rule, if `Path.iterdir` is significantly slower.

---

_Comment by @InSyncWithFoo on 2024-11-27 09:55_

@AlexWaygood Quite bad, in fact:

```python
# s.py
from pathlib import Path

d = Path(__file__).parent / 'd'
d.mkdir(exist_ok = True)

for i in range(10_000):
	(d / f'd{i}').mkdir()

	with (d / f'f{i}.txt').open('w'):
		pass
```

```python
# t.py
from timeit import timeit

N = 1000
SETUP = '''
from pathlib import Path
import os

d = '/workspaces/test/d'
p = Path(d)
'''

a = timeit(stmt = '''for _ in p.iterdir(): ...''', setup = SETUP, number = N)
print(a)

b = timeit(stmt = '''for _ in os.listdir(d): ...''', setup = SETUP, number = N)
print(b)
```

Results on a 4-core, 16GB RAM GitHub Codespace:

```shell
@InSyncWithFoo ➜ /workspaces/test (main) $ uv run -p 3.13 python s.py
@InSyncWithFoo ➜ /workspaces/test (main) $ uv run -p 3.13 python t.py
28.847444689999975
10.691104289999885
```

That's a ~180% increase.

---

_Comment by @AlexWaygood on 2024-11-27 10:53_

@InSyncWithFoo it looks like you're comparing `Path.iterdir()` with `os.listdir()` there rather than `os.scandir()`. But yeah, I'd expect a comparison with `scandir()` to be similarly unfavourable. Please add a `## Known issues` to the docs for this rule saying that we expect this rule to result in slower code, and that you shouldn't enable it in performance-critical places.

If https://github.com/astral-sh/ruff/pull/14632 is accepted, we can also have this rule suggest `Path.scandir()` rather than `Path.iterdir()` if the user's target version is Python 3.14 or higher. That should address some of the performance concerns.

---

_Comment by @MichaReiser on 2024-11-27 11:48_

I'm concerned about adding a rule that is a considerable foot gun for the majority of users when its only benefit is consistency. Especially because the use of `os.scandir` might be exactly because of its performance characteristics. 

I suggest we wait to add this rule until Python 3.14 is closer to stabilizing and only then suggest using `Path.scandir`. 

---

_Label `needs-decision` added by @MichaReiser on 2024-11-27 11:48_

---

_Comment by @AlexWaygood on 2024-11-27 12:12_

@MichaReiser and I discussed this offline. There's a general issue with our `flake8-pathlib` rules that they could impact performance, but this case seems especially bad.

Our decision is that we'll keep `PTH208` (because `Path.iterdir()` is a drop-in replacement for `os.listdir()`), but we'll unfortunately reject this PR (because `Path.iterdir()` is not a drop-in replacement for `os.scandir()`, it does something _slightly_ different, and the likely reason they're using `os.scandir()` rather than `os.listdir()` is precisely because it's the option you want to use for maximum performance).

When Python 3.14 enters the beta period, we can consider adding a rule suggesting to replace `os.scandir` with `Path.scandir`, assuming the new API is still present when the beta period starts.

Sorry about this @InSyncWithFoo -- thanks for the PR anyway! Cc. @sbrugman also, since you were involved in the discussion in #14509

---

_Closed by @AlexWaygood on 2024-11-27 12:12_

---

_Label `needs-decision` removed by @AlexWaygood on 2024-11-27 12:12_

---

_Branch deleted on 2024-11-27 12:53_

---

_Comment by @InSyncWithFoo on 2024-11-27 13:11_

I guess I'll just have to wait for 3.14.0 beta 1, to be released sometime in May. Hopefully I won't forget.

---
