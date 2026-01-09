---
number: 106
title: (feature) awesome flake8 extensions support
type: issue
state: closed
author: kierun
labels: []
assignees: []
created_at: 2022-09-05T14:44:58Z
updated_at: 2025-01-16T17:01:36Z
url: https://github.com/astral-sh/ruff/issues/106
synced_at: 2026-01-07T13:12:14-06:00
---

# (feature) awesome flake8 extensions support

---

_Issue opened by @kierun on 2022-09-05 14:44_

There's a lot of [awesome flake8 extensions](https://github.com/DmytroLitvinov/awesome-flake8-extensions)… Is there a plan to support those?

I suspect that it won't be possible to just add them as the underlying code is so different.

---

_Label `enhancement` added by @charliermarsh on 2022-09-06 15:36_

---

_Comment by @charliermarsh on 2022-09-30 23:27_

The plan is to tackle this on two fronts.

First, in the shorter term, we'd like to implement some of the most popular extensions in ruff directly. (If there are any that are particularly important to you, shout them out!)

Second, in the longer term, we're going to implement a plugin system for ruff. The dream for me right now is that plugins can _either_ be written in Rust _or_ Python (trading off performance for convenience / familiarity). This will take longer and there's no immediate timeline on it -- but it's within scope.


---

_Closed by @charliermarsh on 2022-09-30 23:27_

---

_Comment by @kierun on 2022-10-03 08:13_

@charliermarsh That is great to hear! Thank you.

> If there are any that are particularly important to you, shout them out!

Here is the list I use in my latest project:

```
flake8-black
flake8-comprehensions
flake8-debugger
flake8-fixme
flake8-isort
flake8-mock
flake8-plugin-utils
flake8-pytest-style
flake8-secure-coding-standard
flake8-simplify
flake8-warnings
```

---

_Comment by @amotl on 2022-10-03 09:21_

Hi,

while I don't know if @gutzbenj really needs all of them, I would like to share the list we are currently using on [Wetterdienst](https://github.com/earthobservations/wetterdienst) on this matter.

```
flakeheaven 
flake8-bandit
flake8-black
flake8-bugbear
flake8-builtins
flake8-comprehensions
flake8-copyright
flake8-docstrings
flake8-eradicate
flake8-isort
flake8-print
flake8-return
flake8-2020
```

With kind regards,
Andreas.

---

_Comment by @charliermarsh on 2022-10-03 19:05_

Awesome, thanks for these data points!

---

_Comment by @andersk on 2022-11-12 04:06_

Here are the most popular packages listed in [awesome-flake8-extensions](https://github.com/DmytroLitvinov/awesome-flake8-extensions), sorted by downloads in the last month as reported by the [pypistats.org](https://pypistats.org/) API:

```
1512427 flake8-bugbear
1351772 pep8-naming
1118826 flake8-docstrings
864462 flake8-comprehensions
779488 flake8-isort
644446 flake8-black
603333 flake8-builtins
565059 flake8-quotes
484757 flake8-print
474654 flake8-bandit
460612 flake8-eradicate
406898 flake8-debugger
384780 flake8-broken-line
383094 darglint
352497 flake8-commas
335986 flake8-import-order
313707 flake8-logging-format
305386 flake8-tidy-imports
303112 flake8-simplify
277196 flake8-use-fstring
```

<details>
<summary>More</summary>

```
250942 flake8-rst-docstrings
233391 hacking
232374 flake8-string-format
222559 wemake-python-styleguide
220547 flake8-annotations
194298 flake8-pytest-style
180801 dlint
176481 flake8-pie
170483 flake8-markdown
166841 flake8-return
151870 flake8-variables-names
146199 flake8-typing-imports
137422 flake8-noqa
131391 flake8-2020
121394 flake8-assertive
113746 flake8-executable
113006 flake8-cognitive-complexity
109638 flake8-functions
106822 flake8-expression-complexity
106428 flake8-fixme
101105 flake8-django
96728 flake8-coding
96426 flake8-pyi
88108 flake8-annotations-complexity
73761 flake8-class-attributes-order
73496 flake8-pep3101
71925 flake8-html
65871 flake8-use-pathlib
60792 flake8-pylint
60435 flake8-no-pep420
60075 pandas-vet
58903 flake8-no-implicit-concat
56429 flake8-future-annotations
56290 flake8-annotations-coverage
55921 flake8-pytest
53449 flake8-aaa
52163 flake8-encodings
51337 flake8-functions-names
50828 flake8-absolute-import
50525 flake8-warnings
50419 flake8-comments
50413 flake8-secure-coding-standard
48626 flake8-literal
48570 flake8-slots
48223 flake8-useless-assert
47220 flake8-copyright
46027 jupyterlab-flake8
44606 flake8-pep585
34602 flake8-unused-arguments
33370 flake8-alfred
32124 flake8-implicit-str-concat
31527 flakehell
31325 flake8-printf-formatting
29845 flake8-gl-codeclimate
28151 flake8-json
27423 flake8-multiline-containers
24691 flake8-requirements
23309 flake8-mock
21025 yesqa
18207 flake8-datetimez
17588 flake8-todo
16342 tryceratops
16229 flake8-spellcheck
14993 flake8-future-import
14739 flake8-rst
13861 flake8-todos
12941 flake8-type-checking
12941 flake8-type-checking
12774 flake8-length
11957 flake8-scream
11825 flake8-newspaper-style
9864 flake8-new-union-types
7823 flake8-alphabetize
7737 nitpick
6578 flake8-sql
3862 flake8-walrus
2810 flake8-nb
2670 cohesion
2288 flake8-pep604
2273 flake8-strict
2238 flake8-dunder-all
1978 flake8-author
1445 flake8-strftime
1339 flake8-fastapi
1231 flake8-test-name
1152 flake8-docstring-checker
1056 flake8-no-types
1050 flake8-import-order-spoqa
954 flake8-for-pycharm
922 flake8-pyprojecttoml
817 flake8-django-migrations
779 flake8-obey-import-goat
686 flake8-jira-todo-checker
437 flake8-sphinx-links
435 flake8-dashboard
386 flake8-datetime-utcnow-plugin
374 flake8-codes
306 flake8-too-many
291 flake8-forbidden-func
214 flake8-scrapy
211 flake8-import-style
50 flake8-ownership
41 flake8-match
24 flake8-ado
8 flake8-pytestrail
7 flake8-ruler
```

</details>

---

_Comment by @edgarrmondragon on 2022-11-12 09:55_

I'm working on a `flake8-bandit` PR 🙂

---

_Comment by @razy69 on 2022-12-02 14:45_

Hi,

I would like to know if there is any plan to support:

- https://pypi.org/project/flake8-simplify/
- https://pypi.org/project/flake8-return/
- https://pypi.org/project/flake8-classmethod-staticmethod/

Thanks for this awesome project !



---

_Comment by @charliermarsh on 2022-12-02 16:49_

@razy69 - Yes! They haven't been started but they are planned:

- `flake8-return`: https://github.com/charliermarsh/ruff/issues/304
- `flake8-simplify`: https://github.com/charliermarsh/ruff/issues/998


---

_Comment by @arthur-st on 2022-12-12 18:22_

Hey! I'd like to shout out `dlint` as something I've found useful in the past. https://github.com/dlint-py/dlint

---

_Comment by @WorkHardes on 2022-12-13 08:35_

Hello, 
Is there a plan to support this flake8 extension?
https://pypi.org/project/flake8-class-attributes-order/


---

_Referenced in [astral-sh/ruff#2425](../../astral-sh/ruff/issues/2425.md) on 2023-01-31 23:02_

---

_Comment by @vergenzt on 2025-01-16 17:01_

I'm just gonna mention (for the googlability) that https://github.com/datatheorem/flake8-alfred is substitutable with https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-api. 🙂 

---
