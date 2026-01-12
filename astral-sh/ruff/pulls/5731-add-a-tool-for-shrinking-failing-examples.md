```yaml
number: 5731
title: Add a tool for shrinking failing examples
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: minimizer
created_at: 2023-07-13T09:33:10Z
updated_at: 2023-07-18T08:18:00Z
url: https://github.com/astral-sh/ruff/pull/5731
synced_at: 2026-01-12T15:55:19Z
```

# Add a tool for shrinking failing examples

---

_@konstin_

## Summary

For formatter instabilities, the message we get look something like this:
```text
Unstable formatting /home/konsti/ruff/target/checkouts/deepmodeling:dpdispatcher/dpdispatcher/slurm.py
@@ -47,9 +47,9 @@
-            script_header_dict["slurm_partition_line"] = (
-                NOT_YET_IMPLEMENTED_ExprJoinedStr
-            )
+            script_header_dict[
+                "slurm_partition_line"
+            ] = NOT_YET_IMPLEMENTED_ExprJoinedStr
Unstable formatting /home/konsti/ruff/target/checkouts/deepmodeling:dpdispatcher/dpdispatcher/pbs.py
@@ -26,9 +26,9 @@
-            pbs_script_header_dict["select_node_line"] += (
-                NOT_YET_IMPLEMENTED_ExprJoinedStr
-            )
+            pbs_script_header_dict[
+                "select_node_line"
+            ] += NOT_YET_IMPLEMENTED_ExprJoinedStr
``` 

For ruff crashes. you don't even get that but just the file that crashed it. To extract the actual bug, you'd need to manually remove parts of the file, rerun to see if the bug still occurs (and revert if it doesn't) until you have a minimal example.

With this script, you run

```shell
cargo run --bin ruff_shrinking -- target/checkouts/deepmodeling:dpdispatcher/dpdispatcher/slurm.py target/minirepo/code.py "Unstable formatting" "target/debug/ruff_dev format-dev --stability-check target/minirepo"
```

and get

```python
class Slurm():
    def gen_script_header(self, job):
        if resources.queue_name != "":
            script_header_dict["slurm_partition_line"] = f"#SBATCH --partition {resources.queue_name}"
```

which is an nice minimal example.

I've been using this script and it would be easier for me if this were part of main. The main disadvantage to merging is that it adds additional dependencies.

## Test Plan

I've been using this for a number of minimization. This is an internal helper script you only run manually. I could add a test that minimizes a rule violation if required.

---

_Label `internal` added by @konstin on 2023-07-13 09:33_

---

_Comment by @github-actions[bot] on 2023-07-13 10:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13     12.9±0.42ms     3.2 MB/sec    1.00     11.4±0.27ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.4±0.06ms     6.9 MB/sec    1.00      2.2±0.06ms     7.4 MB/sec
formatter/numpy/globals.py                 1.06   275.9±16.09µs    10.7 MB/sec    1.00   260.1±14.76µs    11.3 MB/sec
formatter/pydantic/types.py                1.10      5.4±0.14ms     4.8 MB/sec    1.00      4.9±0.18ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.03     17.4±0.56ms     2.3 MB/sec    1.00     16.8±0.38ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.16ms     3.9 MB/sec    1.00      4.2±0.20ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   571.9±20.28µs     5.2 MB/sec    1.00   566.7±21.88µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.8±0.26ms     3.3 MB/sec    1.00      7.7±0.27ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.05      8.7±0.27ms     4.7 MB/sec    1.00      8.2±0.25ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1779.3±54.79µs     9.4 MB/sec    1.00  1719.9±45.11µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    210.7±9.42µs    14.0 MB/sec    1.00    206.3±6.76µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.8±0.18ms     6.7 MB/sec    1.00      3.6±0.09ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.17ms     3.7 MB/sec    1.02     11.4±0.20ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.7 MB/sec    1.03      2.2±0.07ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    244.2±5.81µs    12.1 MB/sec    1.02    248.9±8.63µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.10ms     5.3 MB/sec    1.01      4.8±0.11ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     16.2±0.35ms     2.5 MB/sec    1.00     16.0±0.51ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.3±9.86µs     5.9 MB/sec    1.01   506.3±10.09µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.24ms     3.4 MB/sec    1.00      7.3±0.19ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.15ms     5.0 MB/sec    1.02      8.3±0.22ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1700.2±30.04µs     9.8 MB/sec    1.01  1724.1±25.33µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.2±4.29µs    15.4 MB/sec    1.01    193.6±8.00µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.08ms     6.9 MB/sec    1.00      3.6±0.06ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-13 23:47_

> I've been using this script and it would be easier for me if this were part of main. The main disadvantage to merging is that it adds additional dependencies.

Should we consider having some internal ruff tooling in another repository? 

---

_Review comment by @MichaReiser on `crates/ruff_minimizer/Cargo.toml`:9 on 2023-07-14 06:30_

What's the motivation for this to be its own crate vs making it part of the `cargo dev` crate? We could also start splitting `cargo dev` into multiple sub crates, see https://github.com/rome/tools/tree/main/xtask

---

_Review comment by @MichaReiser on `crates/ruff_minimizer/src/main.rs`:209 on 2023-07-14 06:34_

```suggestion
            input[TextRange::up_to(range.start())].to_string() + &input[range.end().to_usize()..]
```

---

_Review comment by @MichaReiser on `crates/ruff_minimizer/Cargo.toml`:2 on 2023-07-14 06:36_

This is called `shrinking` in property based testing https://www.educative.io/answers/what-is-shrinking

---

_Review comment by @MichaReiser on `crates/ruff_minimizer/src/main.rs`:64 on 2023-07-14 06:37_

Nit: This could be implemented as an enum, considering that it is a fixed set of strategies. But I can see how this allows you to split the code nicer. 

---

_@MichaReiser approved on 2023-07-14 06:37_

---

_Review comment by @konstin on `crates/ruff_minimizer/Cargo.toml`:9 on 2023-07-14 08:12_

having less dependencies / faster compilation, but i'm happy to move it anywhere

---

_@konstin reviewed on 2023-07-14 08:12_

---

_Review comment by @konstin on `crates/ruff_minimizer/src/main.rs`:64 on 2023-07-14 08:16_

i've had been thinking about an enum too, with traits it's marginally easier to attach methods

---

_@konstin reviewed on 2023-07-14 08:16_

---

_@konstin reviewed on 2023-07-14 08:16_

---

_Review comment by @konstin on `crates/ruff_minimizer/Cargo.toml`:2 on 2023-07-14 08:16_

that link is helpful, thanks!

---

_Renamed from "Add a failing example minimizer" to "Add a tool for shrinking failing examples" by @konstin on 2023-07-14 08:16_

---

_Comment by @konstin on 2023-07-14 08:32_

> Should we consider having some internal ruff tooling in another repository?

I've prototyped this in this (internal) repo: https://github.com/astral-sh/ruff-tools

---

_Comment by @charliermarsh on 2023-07-14 16:03_

My initial take is that i'm generally pro-monorepo, and it seems reasonable to add this to `ruff_dev`. I'm less concerned about the added dependencies since `ruff_dev` isn't included in the Ruff binary anyway.


---

_Merged by @konstin on 2023-07-18 08:03_

---

_Closed by @konstin on 2023-07-18 08:03_

---

_Branch deleted on 2023-07-18 08:03_

---
