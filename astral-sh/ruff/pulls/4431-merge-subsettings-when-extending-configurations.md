```yaml
number: 4431
title: Merge subsettings when extending configurations
type: pull_request
state: merged
author: bendoerry
labels: []
assignees: []
merged: true
base: main
head: 4348-merge-subsettings
created_at: 2023-05-14T14:12:00Z
updated_at: 2023-05-16T20:22:49Z
url: https://github.com/astral-sh/ruff/pull/4431
synced_at: 2026-01-12T03:50:02Z
```

# Merge subsettings when extending configurations

---

_Pull request opened by @bendoerry on 2023-05-14 14:12_

Currently we override entire config sections when extending configurations.
This change allows us to preserve sections and only override specific config options.

I've verified this works with the tree specified in the linked issue, and with the codebase that spurred me to look into this issue.

(This is my first OSS contribution so please lmk if I've got the process wrong somehow :) )

Closes  #4348

---

_Comment by @github-actions[bot] on 2023-05-14 14:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.15ms     2.5 MB/sec    1.00     16.5±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    497.7±1.62µs     5.9 MB/sec    1.00    493.9±3.59µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.03ms     5.1 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1726.8±8.59µs     9.6 MB/sec    1.00   1713.4±8.70µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.1±1.51µs    15.5 MB/sec    1.00    190.5±5.78µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
parser/large/dataset.py                    1.01      6.5±0.06ms     6.2 MB/sec    1.00      6.4±0.03ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1256.5±5.16µs    13.3 MB/sec    1.00   1256.0±5.58µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    127.2±0.61µs    23.2 MB/sec    1.00    126.8±0.73µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.3 MB/sec    1.00      2.7±0.01ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.4±0.11ms     2.5 MB/sec    1.00     16.3±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.2±6.13µs     6.0 MB/sec    1.00    488.3±6.07µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.13ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.05ms     5.0 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1724.4±24.04µs     9.7 MB/sec    1.00  1709.0±16.76µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.9±4.86µs    15.2 MB/sec    1.00    194.6±5.14µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.02      6.7±0.08ms     6.0 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.02  1284.9±13.50µs    13.0 MB/sec    1.00  1258.6±12.15µs    13.2 MB/sec
parser/numpy/globals.py                    1.01    131.2±2.33µs    22.5 MB/sec    1.00    129.4±2.93µs    22.8 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-14 17:07_

This looks great, I like the solution you settled on here with the trait-based approach.

---

_@charliermarsh reviewed on 2023-05-14 17:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/settings.rs`:108 on 2023-05-14 17:11_

We may be able to code-gen these via a Derive macro (you could look at how `ConfigurationOptions` works as an example), if you're interested.

---

_Review comment by @bendoerry on `crates/ruff/src/settings/configuration.rs`:317 on 2023-05-14 19:50_

This part I'm slightly unsure of, I feel like there's a better way of doing this.
I looked at doing something along the lines of
```rust
if let (Some(base), Some(other)) = (self, other) {
    Some(base.combine(other))
} else {
    self.or(other)
}
```
but ran into an issue with `other` being used after move.

Seeing as the `match` statement worked, if being a bit more verbose, I left it at that for now.

---

_Review comment by @bendoerry on `crates/ruff/src/rules/flake8_annotations/settings.rs`:108 on 2023-05-14 20:03_

Not done any macro stuff before, but I can give it a go. Would this potentially run into an issue with [`flake8_gettext`](https://github.com/charliermarsh/ruff/pull/4431/files#diff-31f36219ba85fb7b29ddb087a9bf221cdec719f50d5af228a2f6e3dc3de1ce1aR79-R83) since this is using a `Vec` instead of an `Option<T>`? This is the only occurrence of not using `Option<T>` that I came across, so arguably I could just not use a derive for that instance, but it does strike me as slightly strange that this isn't an `Option<T>`. I see no reference/reasoning for the difference given in the associated commits/PR.

---

_@bendoerry reviewed on 2023-05-14 20:23_

---

_@charliermarsh reviewed on 2023-05-14 22:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/settings.rs`:108 on 2023-05-14 22:24_

Is that the only field that's not `Option`?

---

_@charliermarsh reviewed on 2023-05-14 22:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/configuration.rs`:317 on 2023-05-14 22:25_

I was gonna suggest roughly what you wrote in this comment. Lemme see if I can figure it out.

---

_@bendoerry reviewed on 2023-05-14 22:29_

---

_Review comment by @bendoerry on `crates/ruff/src/rules/flake8_annotations/settings.rs`:108 on 2023-05-14 22:29_

Derive macro ended up not being too difficult to implement once I'd figured out how it worked, so I've handled the `Vec<T>` field case as well. So now all `Options` structs have their `CombinePluginOptions` impls derived (at least the ones that had an impl in the first place).

---

_@bendoerry reviewed on 2023-05-14 22:41_

---

_Review comment by @bendoerry on `crates/ruff/src/rules/flake8_annotations/settings.rs`:108 on 2023-05-14 22:41_

> Is that the only field that's not `Option`?

Yep thats the only field in all the plugin options thats not `Option` that I could find.

---

_Merged by @charliermarsh on 2023-05-15 02:34_

---

_Closed by @charliermarsh on 2023-05-15 02:34_

---

_Comment by @charliermarsh on 2023-05-15 02:36_

Thanks for this! Really nice PR. I removed the need for `Vec` support via #4434.

---

_Comment by @hotpxl on 2023-05-16 20:22_

Thanks for doing this!!!

---
