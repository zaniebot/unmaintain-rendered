```yaml
number: 3390
title: Setup ecosystem CI
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: ecosystem-ci
created_at: 2023-03-07T21:18:54Z
updated_at: 2023-03-16T22:50:21Z
url: https://github.com/astral-sh/ruff/pull/3390
synced_at: 2026-01-12T04:39:44Z
```

# Setup ecosystem CI

---

_Pull request opened by @sciyoshi on 2023-03-07 21:18_

This PR sets up an "ecosystem" check as an optional part of the CI step for pull requests. The primary piece of this is a new script in `scripts/check_ecosystem.py` which takes two ruff binaries as input and compares their outputs against a corpus of open-source code in parallel. I used ruff's `text` reporting format and stdlib's `difflib` (rather than JSON output and jsondiffs) to avoid adding another dependency. There is a new ecosystem-comment workflow to add a comment to the PR (see [this link](https://securitylab.github.com/research/github-actions-preventing-pwn-requests/) which explains why it needs to be done as a new workflow for security reasons).

Here's [what the comment looks like when there are changes](https://github.com/sciyoshi/ruff/pull/3#issuecomment-1458879463):

![image](https://user-images.githubusercontent.com/39950/223554602-fb18b6d3-7c3e-4616-a4e1-c29b39d7cae0.png)

And here's [what the comment looks like with no changes](https://github.com/sciyoshi/ruff/pull/2#issuecomment-1458747197):

![image](https://user-images.githubusercontent.com/39950/223554712-e12c21ec-7eb3-4d77-be81-b1294edcd1be.png)

I'm open to improvements to make this report more useful; linking to the actual versions of the respective repos might be helpful, for example.

A few points:

- This runs on every PR, but not on main and not as a nightly task as suggested in the linked issue. I think running on every PR is correct because it gives direct feedback on any potential problems before merging. However, a possible later improvement would be to also run on main and compare against the latest released version. (This is what black diff-shades does).
- The "ecosystem" right now consists of zulip and bokeh (latest version of each), and uses their ruff configs. However, the most valuable way of setting this up is actually to test messy codebases that fail many checks, not ones that already use ruff, because that would be the only way to catch PRs introducing false negatives. @charliermarsh any suggestions on what to use for this purpose? Unlike other tools, ruff is fast enough that adding a few large codebases here won't make much of a difference in testing time.
- The base version of ruff used as a comparison is pulled from the latest artifact on the PR merge base ref (i.e. PRs to main will compare the latest version on main to the PR head after the merge commit). For PRs that don't merge cleanly, the results might be inconsistent (I haven't tested this).

Closes charliermarsh/ruff#2677.

---

_Comment by @charliermarsh on 2023-03-08 04:26_

Dang, this is so cool, thank you! I'll review tomorrow and respond to the questions in the PR summary.

---

_Review comment by @twoertwein on `scripts/check_ecosystem.py`:81 on 2023-03-08 20:56_

Running ruff in the isolated mode with all rules enabled could help a bit with 

> However, the most valuable way of setting this up is actually to test messy codebases that fail many checks, not ones that already use ruff, because that would be the only way to catch PRs introducing false negatives.


---

_@twoertwein reviewed on 2023-03-08 20:56_

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:59 on 2023-03-09 00:50_

I'm happy to amend this list after merging, but others that come to mind:

- https://github.com/scikit-build/scikit-build/blob/main/pyproject.toml
- https://github.com/apache/airflow/blob/main/pyproject.toml

Not sure what the incremental cost is for each additional repo. I guess the two costs are "job time" and "job output" -- e.g., if we make a mistake, we might see it repeated across repos. But here, I'm thinking back to projects that we've broken in the past, and that have reasonably advanced configurations.


---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:81 on 2023-03-09 00:52_

If we're looking to catch false negatives, we could consider running over CPython? It's a fairly diverse codebase.

---

_@charliermarsh reviewed on 2023-03-09 00:52_

This is really impressive work and will be _super_ useful for Ruff. No major comments on the code itself, just a couple questions on process (and pardon my ignorance on some of the GitOps stuff):

- If I open a PR, and `ecosystem` job completes, then I update the PR, and the `ecosystem` job runs again, does it appear as two separate comments? Or does it "update" the comment?
- How much more difficult would it to be have this run on explicit PR comment (e.g., reviewer adds `+ecosystem`, and the job kicks off)? I'm mostly thinking here about the noise involved in having the bot comment on every PR. I'm not fully decided on what's "better" here, but I thought I'd ask.
- I assume it'd be _possible_ for me to run this locally prior to cutting a release, by comparing against the binary from the previous release. Any reason that wouldn't be possible with the current configuration?


---

_Comment by @sciyoshi on 2023-03-09 21:52_

> * If I open a PR, and `ecosystem` job completes, then I update the PR, and the `ecosystem` job runs again, does it appear as two separate comments? Or does it "update" the comment?

The workflow will update an existing comment if it finds one.

> How much more difficult would it to be have this run on explicit PR comment (e.g., reviewer adds +ecosystem, and the job kicks off)? I'm mostly thinking here about the noise involved in having the bot comment on every PR. I'm not fully decided on what's "better" here, but I thought I'd ask.

I think this would be fairly easy, although I haven't looked into what would be needed to do it. If this is merged and it feels like the comment is too noisy on every PR, I could look at doing a followup to make it run only when pinged. It could also be changed to not post a comment at all if there are no changes, although then it might be harder to know if that workflow has broken.

> I assume it'd be possible for me to run this locally prior to cutting a release, by comparing against the binary from the previous release. Any reason that wouldn't be possible with the current configuration?

Yes, definitely. The script requires Python 3.11 (for `asyncio.TaskGroup`) but otherwise has no dependencies, and clones repos to temporary directories that are cleaned up automatically so it's fairly self-contained.

On a separate note, I'm not sure why the `ecosystem` check is failing on this PR with the message `API rate limit exceeded for installation ID 22967278`, but likely it's because the base branch for this PR doesn't have a built artifact that it can use for the test.

---

_@sciyoshi reviewed on 2023-03-10 00:23_

---

_Review comment by @sciyoshi on `scripts/check_ecosystem.py`:81 on 2023-03-10 00:23_

CPython could work but it includes a lot of non-Python code, meaning it would do a bunch of extra work to download/clone files that it won't end up checking. I do like the idea of using isolated mode and setting all rules to enabled so I've made that change.

---

_Comment by @sciyoshi on 2023-03-10 00:33_

Out of curiosity I ran `check_ecosystem.py` with the latest `main` vs the latest PyPI version (0.0.254). Ignoring the new rule PLC1901 and the changes to RET504 and the inclusion of `.pyi` files, here are the changes detected:

<details><summary>zulip</summary>
<p>

```diff
+ zerver/lib/ccache.py:81:14: PLR2004 Magic value used in comparison, consider replacing -2147483648 with a constant variable
```

</p>
</details>
<details><summary>bokeh</summary>
<p>

```diff
+ tests/unit/bokeh/models/test_ranges.py:160:37: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/models/test_ranges.py:71:33: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/integration/tools/test_range_tool.py:272:36: PLR2004 Magic value used in comparison, consider replacing -0.1 with a constant variable
+ tests/unit/bokeh/test_events.py:286:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:533:26: PLR2004 Magic value used in comparison, consider replacing -0.5 with a constant variable
+ tests/unit/bokeh/test_events.py:304:28: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/core/test_serialization.py:795:23: PLR2004 Magic value used in comparison, consider replacing -684115200000 with a constant variable
+ tests/unit/bokeh/test_events.py:139:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/integration/models/test_plot.py:161:31: PLR2004 Magic value used in comparison, consider replacing -2.3 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:532:26: PLR2004 Magic value used in comparison, consider replacing -1.5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:531:25: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/test_events.py:180:27: PLR2004 Magic value used in comparison, consider replacing -0.1 with a constant variable
+ tests/unit/bokeh/test_events.py:182:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:527:28: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/core/test_validation.py:100:23: PLR2004 Magic value used in comparison, consider replacing -5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:529:29: PLR2004 Magic value used in comparison, consider replacing -0.5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:528:29: PLR2004 Magic value used in comparison, consider replacing -1.5 with a constant variable
+ tests/unit/bokeh/test_events.py:209:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/integration/tools/test_range_tool.py:166:36: PLR2004 Magic value used in comparison, consider replacing -0.2 with a constant variable
+ tests/unit/bokeh/util/test_util__serialization.py:125:69: PLR2004 Magic value used in comparison, consider replacing -2208988800000.0 with a constant variable
+ tests/unit/bokeh/test_events.py:110:28: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
```

</p>
</details>
<details><summary>scikit-build</summary>
<p>

```diff
```

</p>
</details>
<details><summary>airflow</summary>
<p>

```diff
- tests/system/providers/amazon/aws/utils/__init__.py:256:12: PIE802 [*] Unnecessary list comprehension.
- dev/breeze/src/airflow_breeze/utils/selective_checks.py:270:55: PIE802 [*] Unnecessary list comprehension.
- tests/models/test_taskinstance.py:1859:16: PIE802 [*] Unnecessary list comprehension.
+ airflow/task/task_runner/standard_task_runner.py:168:24: PLR2004 Magic value used in comparison, consider replacing -9 with a constant variable
+ tests/providers/google/cloud/hooks/test_bigquery.py:1296:16: SIM300 [*] Yoda conditions are discouraged, use `result == -1` instead
+ tests/system/providers/amazon/aws/utils/__init__.py:256:16: PIE802 [*] Unnecessary list comprehension.
+ dev/breeze/src/airflow_breeze/utils/selective_checks.py:271:13: PIE802 [*] Unnecessary list comprehension.
+ tests/task/task_runner/test_standard_task_runner.py:287:40: PLR2004 Magic value used in comparison, consider replacing -9 with a constant variable
+ tests/triggers/test_temporal.py:61:12: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/models/test_taskinstance.py:1859:20: PIE802 [*] Unnecessary list comprehension.
```

</p>
</details>

It seems like PLR2004 is now catching more cases than before (maybe when there's more than one case on a line?)

---

_Comment by @sciyoshi on 2023-03-10 00:34_

It also seems like some of the position reporting might have changed. I think the diff printing is a bit broken however, I'll look into that.

---

_Comment by @sciyoshi on 2023-03-10 00:36_

Here's the fixed summary of the current diff:

<details><summary>zulip</summary>
<p>

```diff
+ zerver/lib/ccache.py:81:14: PLR2004 Magic value used in comparison, consider replacing -2147483648 with a constant variable
```

</p>
</details>
<details><summary>bokeh</summary>
<p>

```diff
+ tests/integration/models/test_plot.py:161:31: PLR2004 Magic value used in comparison, consider replacing -2.3 with a constant variable
+ tests/integration/tools/test_range_tool.py:166:36: PLR2004 Magic value used in comparison, consider replacing -0.2 with a constant variable
+ tests/integration/tools/test_range_tool.py:272:36: PLR2004 Magic value used in comparison, consider replacing -0.1 with a constant variable
+ tests/unit/bokeh/core/test_serialization.py:795:23: PLR2004 Magic value used in comparison, consider replacing -684115200000 with a constant variable
+ tests/unit/bokeh/core/test_validation.py:100:23: PLR2004 Magic value used in comparison, consider replacing -5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:527:28: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:528:29: PLR2004 Magic value used in comparison, consider replacing -1.5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:529:29: PLR2004 Magic value used in comparison, consider replacing -0.5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:531:25: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:532:26: PLR2004 Magic value used in comparison, consider replacing -1.5 with a constant variable
+ tests/unit/bokeh/models/test_annotations.py:533:26: PLR2004 Magic value used in comparison, consider replacing -0.5 with a constant variable
+ tests/unit/bokeh/models/test_ranges.py:160:37: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/models/test_ranges.py:71:33: PLR2004 Magic value used in comparison, consider replacing -1.0 with a constant variable
+ tests/unit/bokeh/test_events.py:110:28: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/test_events.py:139:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/test_events.py:180:27: PLR2004 Magic value used in comparison, consider replacing -0.1 with a constant variable
+ tests/unit/bokeh/test_events.py:182:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/test_events.py:209:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/test_events.py:286:24: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/test_events.py:304:28: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
+ tests/unit/bokeh/util/test_util__serialization.py:125:69: PLR2004 Magic value used in comparison, consider replacing -2208988800000.0 with a constant variable
```

</p>
</details>
<details><summary>scikit-build</summary>
<p>

```diff
```

</p>
</details>
<details><summary>airflow</summary>
<p>

```diff
+ airflow/task/task_runner/standard_task_runner.py:168:24: PLR2004 Magic value used in comparison, consider replacing -9 with a constant variable
- dev/breeze/src/airflow_breeze/utils/selective_checks.py:270:55: PIE802 [*] Unnecessary list comprehension.
+ dev/breeze/src/airflow_breeze/utils/selective_checks.py:271:13: PIE802 [*] Unnecessary list comprehension.
- tests/models/test_taskinstance.py:1859:16: PIE802 [*] Unnecessary list comprehension.
+ tests/models/test_taskinstance.py:1859:20: PIE802 [*] Unnecessary list comprehension.
+ tests/providers/google/cloud/hooks/test_bigquery.py:1296:16: SIM300 [*] Yoda conditions are discouraged, use `result == -1` instead
- tests/system/providers/amazon/aws/utils/__init__.py:256:12: PIE802 [*] Unnecessary list comprehension.
+ tests/system/providers/amazon/aws/utils/__init__.py:256:16: PIE802 [*] Unnecessary list comprehension.
+ tests/task/task_runner/test_standard_task_runner.py:287:40: PLR2004 Magic value used in comparison, consider replacing -9 with a constant variable
+ tests/triggers/test_temporal.py:61:12: PLR2004 Magic value used in comparison, consider replacing -2 with a constant variable
```

</p>
</details>

---

_Comment by @charliermarsh on 2023-03-10 00:46_

These diffs are insanely useful.

> It seems like PLR2004 is now catching more cases than before (maybe when there's more than one case on a line?)

I fixed a bug that was ignoring negated constants (like `-1`, which is a `UnaryOp`, not a `Constant`).

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:81 on 2023-03-10 00:51_

The downside of using isolated mode is that in some cases, it will _avoid_ exercising certain behaviors. For example, Airflow uses `namespace-packages`, and if we run in isolated mode, we might miss breakages to our namespace packages handling, since in isolated mode, it's effectively running with `namespace-packages = []`. I don't know how to resolve that though. I suppose we could run without `--isolated`, but with `--select ALL`?


---

_@charliermarsh reviewed on 2023-03-10 00:51_

---

_Merged by @charliermarsh on 2023-03-10 22:39_

---

_Closed by @charliermarsh on 2023-03-10 22:39_

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:81 on 2023-03-10 22:41_

I may end up tweaking this.

---

_@charliermarsh reviewed on 2023-03-10 22:41_

---

_Branch deleted on 2023-03-10 22:58_

---

_Comment by @henryiii on 2023-03-16 21:21_

Ha, wow, I was just thinking this would be a good idea (working in typeshed and enjoying mypy_primer), and thought I'd suggest it, and here it is, already in!

---

_@henryiii reviewed on 2023-03-16 21:27_

---

_Review comment by @henryiii on `scripts/check_ecosystem.py`:59 on 2023-03-16 21:27_

I think this would be better as a data file (yaml or toml or something), it would make it easier to maintain. The cost per repo would be very interesting, I'd be happy to add a few of the projects I work on (and also am aware of). :P Pretty sure it's low because Ruff is fast? (Usually this stops me from setting up something like this for things like pybind11 and scikit-build - though scikit-build is planned anyway).

I'd also avoid showing any repos that aren't broken in the default output. I think with that you could scale up to at least a few dozen. Mypy primer does something ike 50. (Also, it hardcodes the values in just like this, and doesn't use a config file for it).

---

_Comment by @charliermarsh on 2023-03-16 21:28_

Let us know if there are projects that you think could be good candidate to include here :)

(We added typeshed recently :))


---

_Comment by @henryiii on 2023-03-16 21:34_

A nice addition to check_ecosystem would be a way to run it on a single version of Ruff, maybe even selecting a single project. I'd like to verify a project passes before adding it.

---

_Comment by @henryiii on 2023-03-16 21:48_

Opened one in https://github.com/charliermarsh/ruff/pull/3563.

Other ideas for things I'm directly involved in:
* pypa/cibuildwheel
* pypa/build
* scikit-hep/awkward (large number of files, but might be a bit repetitive though if something triggers)

Slightly less directly:

* wntrblm/nox

And others:

* pandas-dev/pandas

What about CPython?

---

_Comment by @onerandomusername on 2023-03-16 21:51_

Not sure if DisnakeDev/disnake is a good candidate but given we've found 3-4 bugs now it might be worth it (but I'll leave that up to you).

We're still in the process of adding more ruff rules, if that makes a difference. 

---

_Comment by @charliermarsh on 2023-03-16 22:09_

I'm guessing most of the cost will be in cloning... But I could be wrong purely a guess. I think we should add more repos, but run them without `--select ALL` IMO. E.g., run a subset with `--select ALL` so that we can see the "impact" of a new rule, but run a wider range with their own Ruff settings.


---

_Comment by @onerandomusername on 2023-03-16 22:47_

> I'm guessing most of the cost will be in cloning... But I could be wrong purely a guess. 

I'm pretty sure a shallow clone would make it checkout quite fast. I think that what diff shades (for black) does but I don't remember. 

---

_Comment by @henryiii on 2023-03-16 22:50_

This is a shallow clone. My guess is most of the total time is compiling Ruff, and the per-repo part is quick.

---
