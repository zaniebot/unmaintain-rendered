```yaml
number: 4265
title: "Support new `extend-per-file-ignores` setting"
type: pull_request
state: merged
author: aacunningham
labels: []
assignees: []
merged: true
base: main
head: update-per-file-ignores-to-merge
created_at: 2023-05-07T05:26:54Z
updated_at: 2023-05-19T16:24:05Z
url: https://github.com/astral-sh/ruff/pull/4265
synced_at: 2026-01-12T15:55:15Z
```

# Support new `extend-per-file-ignores` setting

---

_@aacunningham_

Putting this up as a POC in response to #4230. End result should look something like this:

```toml
# ruff.toml
extend = "pyproject.toml"
[extend-per-file-ignores]
"file_a.py" = ["RUF001"]
"file_b.py" = ["RUF002"]

# pyproject.toml
[per-file-ignores]
"file_a.py" = ["RUF002"]
"file_c.py" = ["RUF003"]


# Final result
# Roughly equivalent to (but not exactly)
[per-file-ignores]
"file_a.py" = ["RUF001", "RUF002"]
"file_b.py" = ["RUF002"]
"file_c.py" = ["RUF003"]
```

This has been reworked from modifying the existing `per-file-ignores` to adding a new `extend-per-file-ignores`. Kind of resolves issues with the old approach, but adds more complexity to the code.

---

_Comment by @github-actions[bot] on 2023-05-07 05:40_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.05     16.7±0.52ms     2.4 MB/sec     1.00     15.9±0.37ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.12ms     4.2 MB/sec     1.00      3.9±0.13ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   489.5±17.20µs     6.0 MB/sec     1.01   495.2±22.99µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.22ms     3.7 MB/sec     1.00      6.8±0.32ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      7.8±0.18ms     5.2 MB/sec     1.00      7.6±0.26ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1680.3±130.27µs     9.9 MB/sec    1.00  1630.9±53.31µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    197.5±9.49µs    14.9 MB/sec     1.00    193.8±7.87µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.11ms     7.2 MB/sec     1.00      3.4±0.12ms     7.4 MB/sec
parser/large/dataset.py                    1.00      6.0±0.20ms     6.8 MB/sec     1.01      6.1±0.15ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1211.1±51.69µs    13.7 MB/sec     1.00  1217.1±39.44µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    122.2±4.64µs    24.1 MB/sec     1.01    123.7±4.43µs    23.9 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.11ms     9.6 MB/sec     1.00      2.6±0.08ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     21.8±0.54ms  1908.7 KB/sec    1.00     20.9±0.78ms  1994.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      5.7±0.17ms     2.9 MB/sec    1.00      5.3±0.15ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   646.4±24.51µs     4.6 MB/sec    1.00   625.7±22.99µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.07      9.4±0.30ms     2.7 MB/sec    1.00      8.8±0.23ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.12     11.9±0.24ms     3.4 MB/sec    1.00     10.6±0.22ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12      2.5±0.08ms     6.7 MB/sec    1.00      2.2±0.06ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.06    273.8±6.67µs    10.8 MB/sec    1.00   258.7±15.96µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.13      5.3±0.30ms     4.8 MB/sec    1.00      4.7±0.22ms     5.4 MB/sec
parser/large/dataset.py                    1.03      8.6±0.18ms     4.7 MB/sec    1.00      8.4±0.16ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.04  1653.7±39.99µs    10.1 MB/sec    1.00  1596.3±43.92µs    10.4 MB/sec
parser/numpy/globals.py                    1.00    167.6±4.96µs    17.6 MB/sec    1.00    168.0±4.94µs    17.6 MB/sec
parser/pydantic/types.py                   1.02      3.7±0.10ms     7.0 MB/sec    1.00      3.6±0.06ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-05-07 08:15_

How does it handle `toml` not on the same depth?
Should it remove the name of the folder or add `../` depending on the path of the `toml`?

---

_Comment by @aacunningham on 2023-05-07 17:26_

> How does it handle `toml` not on the same depth?
> Should it remove the name of the folder or add `../` depending on the path of the `toml`?

Ye sorry, that example was probably a bit reductive. I don't _definitively_ know, but from my cursory reading it's still applying the same logic to the paths, converting them to absolute paths based on the source configuration's location. So a ruff.toml in `src/a` that extends a pyproject.toml in `src` should end up with patterns that are prefixed with `src/a` and `src` respectively.

But ye, I can double check, and there's probably tests somewhere that can validate that.

---

_Comment by @MichaReiser on 2023-05-08 06:21_

Whether changing the strategy makes sense is a difficult question because it depends on your use case. You may actually want to override the ignores from the extended configuration, which then becomes impossible (you'll never be able to subtract ignores). 

What's the merging strategy for other ignore settings? 

---

_Comment by @charliermarsh on 2023-05-08 19:48_

So, in general, when we `extend` another configuration file, all settings defined in the new "child" configuration file override those defined in the "parent".

The only exceptions are the `extend-*` variants. So, for example, `extend-include` gets combined with the parent, as does `extend-select`.

So, if we support this, I think it should be via an `extend-per-file-ignores` option, and I think it probably _should_ do a deep merge (i.e., if two dictionaries include the same file key, then the list of ignores for each key should be concatenated).

Being able to extend `per-file-ignores` feels like a bit of an edge case. I'm not totally opposed to it, since we have precedent for these "extendable" configuration options, but I'd be curious how complicated the code becomes to support it.


---

_Comment by @aacunningham on 2023-05-08 20:24_

I noticed that the `rules_selections` gets turned into a Vec of all configs combined (which I think includes `select` and `ignore` and others). But I think the rule selection system probably warrants some complexity to determine what's valid and what's not. I think with some of the messaging around `extend-select` and `extend-ignore`, wasn't sure if the `extend-` pattern was something that was being moved away from, but that may have just been related to this `rule_selections` logic.

But ya to Reiser's point, I think the mental overhead of understanding "which rules extend/merge and which ones override" gets bigger if this one merges and _many_ other settings don't, and there's no longer a mechanism to remove ignores.

> So, if we support this, I think it should be via an extend-per-file-ignores option, and I think it probably should do a deep merge (i.e., if two dictionaries include the same file key, then the list of ignores for each key should be concatenated).
> 
> Being able to extend per-file-ignores feels like a bit of an edge case. I'm not totally opposed to it, since we have precedent for these "extendable" configuration options, but I'd be curious how complicated the code becomes to support it.

Making this setting extendable definitely feels edge case-y. I put this up to see what it would even look like, but I ain't 100% sold on it. Let me mess around with switching this logic over to `extend-per-file-ignores` and see where that takes this. Then we can give it a quick look and if it's too much complexity/overhead, we can make a note in that original issue and mention this. Sound good?

---

_Renamed from "`per-file-ignores` now merges instead of overwrites" to "Support new `extend-per-file-ignores` setting" by @aacunningham on 2023-05-13 21:20_

---

_Comment by @aacunningham on 2023-05-13 21:25_

Aight y'all, if you're free and wanna give this a quick gut check before I invest more time into tests for the changes. It's a cleaner approach with the separate setting, but it's still another setting being added, and the benefits might be kinda fringe.

~~(p.s. gonna fix that build issue real quick...)~~ _ezpz_

---

_Marked ready for review by @charliermarsh on 2023-05-19 03:33_

---

_Comment by @charliermarsh on 2023-05-19 03:38_

My inclination is to merge this, since it gives us consistent `extend-*` variants for all of the rule selection settings.

---

_Merged by @charliermarsh on 2023-05-19 16:24_

---

_Closed by @charliermarsh on 2023-05-19 16:24_

---
