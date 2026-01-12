```yaml
number: 14582
title: "[airflow] Avoid deprecated values (AIR302)"
type: pull_request
state: merged
author: uranusjr
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: airflow-member-removals
created_at: 2024-11-25T07:34:34Z
updated_at: 2024-12-02T09:14:00Z
url: https://github.com/astral-sh/ruff/pull/14582
synced_at: 2026-01-12T15:55:48Z
```

# [airflow] Avoid deprecated values (AIR302)

---

_@uranusjr_

## Summary

Airflow 3.0 removes various deprecated functions, members, modules, and other values. They have been deprecated in 2.x, but the removal causes incompatibilities that we want to detect.

(We are deprecating a lot more things. I want to use this to establish a basic structure so future checks can be submitted more easily.)

Ref: #14626

## Test Plan

A test fixture is included in the PR.


---

_Comment by @github-actions[bot] on 2024-11-25 09:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/1e0a499d1357a2025e71a38038fb61490969a9df/performance/src/performance_dags/performance_dag/performance_dag.py#L230'>performance/src/performance_dags/performance_dag/performance_dag.py:230:11:</a> AIR301 DAG should have an explicit `schedule` argument
+ <a href='https://github.com/apache/airflow/blob/1e0a499d1357a2025e71a38038fb61490969a9df/performance/src/performance_dags/performance_dag/performance_dag.py#L244'>performance/src/performance_dags/performance_dag/performance_dag.py:244:9:</a> AIR302 `schedule_interval` is removed in Airflow 3.0; use schedule instead
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR302 | 1 | 1 | 0 | 0 | 0 |
| AIR301 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-11-25 09:47_

---

_Label `preview` added by @MichaReiser on 2024-11-25 09:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1034 on 2024-11-25 09:50_

Can you tell me a bit about your intended rule numbering? It seems you've something in mind because you chose 302 over e.g 301.

Somewhat related. The numpy rule is called `Numpy2Deprecation`. Should we align this rule's name to `Airflow3Deprecation`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:68 on 2024-11-25 09:51_

We could consider adding a `Modules::Airflow` to avoid semantic name lookups for users not using airflow. See https://github.com/astral-sh/ruff/blob/928ffd66503759679823c5f346941ef58486766e/crates/ruff_linter/src/rules/numpy/rules/numpy_2_0_deprecation.rs#L162-L164

---

_@MichaReiser approved on 2024-11-25 09:52_

This is great. Thank you. I've two small questions/suggestions. 

---

_@uranusjr reviewed on 2024-11-25 11:41_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/codes.rs`:1034 on 2024-11-25 11:41_

Well simply because I implemented #14581 (which used AIR301) before this. We don’t really have a rule on these; I can switch them around if it is nicer.

(Now that I think of it, should we start with 300 instead…?)

---

_@sbrugman reviewed on 2024-11-25 11:45_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:42 on 2024-11-25 11:45_

Is there some documentation available already, such as a migration guide? We could also link the changelog or deprecation PRs.

---

_@MichaReiser reviewed on 2024-11-25 11:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1034 on 2024-11-25 11:58_

> these; I can switch them around if it is nicer.

Ah no, it's fine. Either way works for us.

> (Now that I think of it, should we start with 300 instead…?)

We haven't been very consistent about whether we start with 0 or 1 across linters, but I think we've been mostly consistent for a single inter. That's why I would prefer starting with 301, considering that there's already 001. 



---

_@uranusjr reviewed on 2024-11-26 08:34_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:42 on 2024-11-26 08:34_

Unfortunately there’s not. We may create it later, but would it also make sense to use this as the documentation? I can make the description here a bit longer, and the guide can link back to these rules instead.

---

_@MichaReiser reviewed on 2024-11-26 08:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:42 on 2024-11-26 08:39_

It probably "depends," but I have a slight preference toward having this documentation outside of Ruff, at least if it should also cover *why* something has been deprecated and provide guidance with what it should be replaced instead. Otherwise, the "burden" for maintaining the list falls on us and I don't see us having the authority or knowledge to maintain the list. 

---

_@uranusjr reviewed on 2024-11-26 10:09_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:42 on 2024-11-26 10:09_

Got it, that makes sense. We will create a separate migration guide then. This will be done later and we’ll add the link here.

---

_@uranusjr reviewed on 2024-11-26 10:27_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/deprecated_members.rs`:68 on 2024-11-26 10:27_

Module declared in #14581 instead. I’ll wait for that to be merged first and rebase this.

---

_Comment by @MichaReiser on 2024-11-26 12:39_

Feel free to ping me when you rebased the PR and I'll merge it. This is great work. 

---

_Comment by @uranusjr on 2024-11-27 10:17_

Alright, I’ve added argument deprecation to AIR302. I also added some structure in the module since we’re probably going to add other kinds of deprecation (like `foo["deprecated"]`).

---

_@dhruvmanila reviewed on 2024-11-27 10:39_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-11-27 10:39_

I think this is the same as the existing `AIR301`, right? If so, we should remove the `AIR301` implementation.

---

_@uranusjr reviewed on 2024-11-28 06:30_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-11-28 06:30_

The point of `AIR301` is to detect DAGs without a schedule argument, like this

```python
DAG(dag_id="foo")
```

while the rule here is to detect a DAG using a deprecated schedule argument like

```python
DAG(dag_id="foo", schedule_interval="@daily")
```

We shouldn’t remove AIR301 because AIR302 does not catch the error in the first DAG. I think we should also detect `schedule_interval` and `timetable` in AIR301, so the second DAG only emits AIR302 instead of both errors.

---

_Review requested from @MichaReiser by @uranusjr on 2024-11-28 13:28_

---

_@uranusjr reviewed on 2024-11-28 13:28_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-11-28 13:28_

Done in the latest commit

---

_@MichaReiser reviewed on 2024-11-29 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-11-29 16:58_

Returning a `vec` of possible deprecations here is suboptimal for performance because it means that Ruff allocates at least two vectors for every `airflow` call expression. 

I think we could do better by changing the API slightly

```rust
fn deprecated_keyword_argument(arguments: &Arguments, argument: &str, replacement: Option<&str>) -> Option<Diagnostic> {
	let keyword = arguments.find_keyword(argument)?;
	
	Some(Diagnostic::new(
        Airflow3Removal {
            deprecated: argument.to_string(),
            replacement: match replacement {
                Some(name) => Replacement::Name(name.to_owned()),
                None => Replacement::None,
            },
        },
        keyword
            .arg
            .as_ref()
            .map_or_else(|| keyword.range(), Ranged::range),
    ));
}
```

and you can then use it like this

```rust
    let deprecations = match qualname.segments() {
        ["airflow", .., "DAG" | "dag"] => {
					checker.diagnostics.extend(removed_argument(arguments, "timetable", Some("schedule")));
					checker.diagnostics.extend(removed_argument(arguments, "schedule_interval", Some("schedule")));
		}
```

---

_@uranusjr reviewed on 2024-12-01 15:45_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-12-01 15:45_

Sounds good to me. I was struggling to find a way to do this more efficiently.

---

_@uranusjr reviewed on 2024-12-02 06:37_

---

_Review comment by @uranusjr on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:67 on 2024-12-02 06:37_

Alright this is pretty nice! Need to suppress Clippy since we do want to add more cases here in the future.

---

_Merged by @MichaReiser on 2024-12-02 07:39_

---

_Closed by @MichaReiser on 2024-12-02 07:39_

---

_Branch deleted on 2024-12-02 09:14_

---
