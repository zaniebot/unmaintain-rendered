```yaml
number: 11605
title: "Raise `pep8-naming` errors for methods without any arguments"
type: pull_request
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
base: main
head: charlie/n
created_at: 2024-05-30T02:35:30Z
updated_at: 2024-05-30T13:17:32Z
url: https://github.com/astral-sh/ruff/pull/11605
synced_at: 2026-01-10T21:56:00Z
```

# Raise `pep8-naming` errors for methods without any arguments

---

_Pull request opened by @charliermarsh on 2024-05-30 02:35_

## Summary

If a method has _no_ arguments, we should still raise the `pep8-naming` error.

Closes https://github.com/astral-sh/ruff/issues/11600.


---

_Label `rule` added by @charliermarsh on 2024-05-30 02:35_

---

_Comment by @github-actions[bot] on 2024-05-30 02:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+137 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/tile_providers.py#L154'>src/bokeh/tile_providers.py:154:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L58'>tests/unit/bokeh/plotting/test__plot.py:58:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L68'>tests/unit/bokeh/plotting/test__plot.py:68:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L72'>tests/unit/bokeh/plotting/test__plot.py:72:9:</a> N805 First argument of a method should be named `self`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+133 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py#L11'>Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py:11:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py#L8'>Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py:8:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1015'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1015:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1042'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1042:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1064'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1064:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1086'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1086:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1109'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1109:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L152'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:152:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L174'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:174:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2008'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2008:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L200'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:200:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2023'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2023:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2050'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2050:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2067'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2067:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2094'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2094:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2146'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2146:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2161'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2161:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2224'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2224:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L222'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:222:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2239'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2239:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2298'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2298:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2382'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2382:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L245'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:245:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2489'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2489:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2496'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2496:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2578'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2578:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2588'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2588:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2685'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2685:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2692'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2692:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2702'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2702:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2738'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2738:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2745'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2745:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2755'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2755:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2820'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2820:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2830'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2830:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L283'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:283:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2840'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2840:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2908'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2908:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2918'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2918:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2928'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2928:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2987'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2987:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3032'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3032:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L303'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:303:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3042'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3042:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3052'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3052:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3104'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3104:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3141'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3141:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3148'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3148:13:</a> N805 First argument of a method should be named `self`
... 85 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N805 | 137 | 137 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+137 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/tile_providers.py#L154'>src/bokeh/tile_providers.py:154:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L58'>tests/unit/bokeh/plotting/test__plot.py:58:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L68'>tests/unit/bokeh/plotting/test__plot.py:68:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__plot.py#L72'>tests/unit/bokeh/plotting/test__plot.py:72:9:</a> N805 First argument of a method should be named `self`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+133 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py#L11'>Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py:11:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py#L8'>Packs/AWS-SQS/Integrations/AWS-SQS/AWS-SQS_test.py:8:9:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1015'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1015:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1042'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1042:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1064'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1064:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1086'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1086:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L1109'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:1109:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L152'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:152:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L174'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:174:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2008'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2008:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L200'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:200:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2023'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2023:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2050'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2050:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2067'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2067:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2094'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2094:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2146'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2146:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2161'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2161:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2224'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2224:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L222'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:222:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2239'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2239:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2298'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2298:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2382'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2382:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L245'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:245:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2489'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2489:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2496'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2496:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2578'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2578:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2588'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2588:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2685'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2685:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2692'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2692:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2702'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2702:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2738'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2738:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2745'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2745:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2755'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2755:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2820'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2820:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2830'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2830:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L283'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:283:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2840'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2840:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2908'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2908:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2918'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2918:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2928'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2928:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L2987'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:2987:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3032'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3032:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L303'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:303:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3042'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3042:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3052'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3052:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3104'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3104:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3141'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3141:13:</a> N805 First argument of a method should be named `self`
+ <a href='https://github.com/demisto/content/blob/242a92d157163967c06db9fb1528213f6767ae0c/Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py#L3148'>Packs/GoogleChronicleBackstory/Integrations/GoogleChronicleBackstory/GoogleChronicleBackstory_test.py:3148:13:</a> N805 First argument of a method should be named `self`
... 85 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N805 | 137 | 137 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:62 on 2024-05-30 07:24_

I feel conflicted about this change. Or at least, I wouldn't expect methods without arguments to be flagged when reading the documentation:

> Checks for instance methods that use a name other than self for their first argument.

I understand that this implies that the method must have at least one argument for the rule to raise a violation. 

---

_@MichaReiser reviewed on 2024-05-30 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:239 on 2024-05-30 07:26_

What about `kwarg`?

---

_@MichaReiser reviewed on 2024-05-30 07:27_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-05-30 07:27_

---

_@AlexWaygood reviewed on 2024-05-30 07:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:62 on 2024-05-30 07:46_

Yeah, I'm also not sure about this... Do we not have other rules that flag if an instance or class method has zero parameters? If not, maybe we should? I think it's important to flag that, as it's almost certainly a bug, but I'm not sure it's a pep8-naming rule's responsibility to flag it

---

_@MichaReiser reviewed on 2024-05-30 07:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:62 on 2024-05-30 07:51_

Edit: I think if we change this, then it should probably be a preview only change because it changes the scope of the rule (and it's less clear that it doesn't change the intent of the rule)

---

_@charliermarsh reviewed on 2024-05-30 13:17_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_first_argument_name.rs`:62 on 2024-05-30 13:17_

Thanks, I said the same thing and came to the same conclusion in the linked issue.

---

_Comment by @charliermarsh on 2024-05-30 13:17_

Only still open to see ecosystem changes.

---

_Closed by @charliermarsh on 2024-05-30 13:17_

---
