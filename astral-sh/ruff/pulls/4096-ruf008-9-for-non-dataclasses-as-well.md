```yaml
number: 4096
title: RUF008/9 for non-dataclasses as well
type: pull_request
state: closed
author: adampauls
labels: []
assignees: []
base: main
head: main
created_at: 2023-04-25T15:58:25Z
updated_at: 2023-06-12T16:55:40Z
url: https://github.com/astral-sh/ruff/pull/4096
synced_at: 2026-01-12T03:43:29Z
```

# RUF008/9 for non-dataclasses as well

---

_Pull request opened by @adampauls on 2023-04-25 15:58_

Closes #4053. 

This is my first PR and I didn't know if discussion in #4053 ended with a tacit approval of this change or not, so I didn't put this up fully polished just yet. I'm hoping for early feedback since this is a pretty simple PR, but happy to go back and polish first if that's preferred. 

Makes new rules for non-dataclasses, which might lead to some confusion since there is such high overlap. Still, I didn't want to break backwards compatibility, even for very new rules. 

---

_Comment by @github-actions[bot] on 2023-04-25 16:32_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     15.5±0.09ms     2.6 MB/sec    1.00     14.8±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.02    388.1±1.39µs     7.6 MB/sec    1.00    378.9±1.70µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±0.01ms     3.9 MB/sec    1.00      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.10      8.3±0.01ms     4.9 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1700.6±3.97µs     9.8 MB/sec    1.00   1584.1±2.46µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.06    181.9±0.77µs    16.2 MB/sec    1.00    172.2±0.18µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.08      3.6±0.00ms     7.0 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.01      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1145.6±1.01µs    14.5 MB/sec    1.00   1148.8±1.32µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    116.9±0.27µs    25.2 MB/sec    1.00    117.3±1.14µs    25.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.07ms     2.5 MB/sec    1.00     16.6±0.28ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.03ms     3.9 MB/sec    1.00      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.7±8.08µs     6.7 MB/sec    1.02    449.0±6.43µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.40ms     4.3 MB/sec    1.01      9.5±0.11ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.04ms     8.2 MB/sec    1.04      2.1±0.03ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    213.8±3.23µs    13.8 MB/sec    1.02    219.0±7.13µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.07ms     5.8 MB/sec    1.04      4.6±0.04ms     5.6 MB/sec
parser/large/dataset.py                    1.00      7.0±0.39ms     5.8 MB/sec    1.01      7.1±0.06ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1305.9±6.27µs    12.8 MB/sec    1.01  1324.0±12.39µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    134.4±1.07µs    21.9 MB/sec    1.02    137.2±4.75µs    21.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.01ms     8.8 MB/sec    1.01      2.9±0.02ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:867 on 2023-04-26 00:37_

Nit
```suggestion
                    let is_dataclass = ruff::rules::is_dataclass(self, decorator_list);
                    
                    if is_dataclass {
	                    if self.settings.rules.enabled(Rule::MutableDataclassDefault) {
	                        ruff::rules::mutable_class_default(self, true, body);
	                    }

	                    if self
	                            .settings
	                            .rules
	                            .enabled(Rule::FunctionCallInDataclassDefaultArgument)
	                    {
	                        ruff::rules::function_call_in_class_defaults(
	                            self,
	                            body,
	                            is_dataclass,
	                            true,
	                        );
	                    }
                    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/mod.rs`:169 on 2023-04-26 00:49_

Nit
```suggestion

    #[test_case(Rule::MutableClassDefault, Path::new("RUF010.py"); "RUF010")]
    #[test_case(Rule::FunctionCallInClassDefaultArgument, Path::new("RUF011.py"); "RUF011")]
```

---

_@MichaReiser approved on 2023-04-26 00:50_

---

_Comment by @adampauls on 2023-04-26 04:54_

I don't know what's going on with the test failure. It doesn't fail locally on my mac. Did I exceed some constant number of rules or something? 

---

_@adampauls reviewed on 2023-04-28 16:14_

---

_Review comment by @adampauls on `crates/ruff/src/registry/rule_set.rs`:10 on 2023-04-28 16:14_

Is this okay? It fixed a test failure. 

---

_@MichaReiser reviewed on 2023-05-01 09:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/registry/rule_set.rs`:10 on 2023-05-01 09:03_

Yeah, this is fine. I assume your rule increased ruff's rule count over 576 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:858 on 2023-05-01 09:06_

Nit: Guaranteed to always be `true`
```suggestion
                                true,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:875 on 2023-05-01 09:07_

Nit: guaranteed to always be `false`
```suggestion
                            `false`,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:263 on 2023-05-01 09:12_

Nit: It can be difficult to understand the semantic of `true` in a call of `mutable_class_default(checker, true, body)` without looking at the method signature. 

Introducing an enum can improve readability:

```rust
#[derive(Copy, Clone, Debug)]
enum ClassKind {
	Class,
	Dataclass
}
```

It has the added benefit that you can implement methods on the kind

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-01 09:13_

Nit: Do we need both booleans. It seems that `emit_dataclass_error` is always `true` if `is_dataclass` is true

---

_@MichaReiser reviewed on 2023-05-01 09:13_

---

_Review comment by @adampauls on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-02 00:37_

Well, it depends on if you think RUF008/9 are distinct from RUF010/11. Should the latter issue the same error as the former if the former are not enabled? Probably not right?

I would personally rather just collapse the error but I wasn't sure if it was acceptable to break backwards compatibility like that. That said, RUF008/9 are quite new so maybe it's okay? 

---

_@adampauls reviewed on 2023-05-02 00:37_

---

_@adampauls reviewed on 2023-05-02 00:39_

---

_Review comment by @adampauls on `crates/ruff/src/checkers/ast/mod.rs`:875 on 2023-05-02 00:39_

`is_dataclass` can be `true` here. Same confusion as explained in [this comment](https://github.com/charliermarsh/ruff/pull/4096/files#r1181991664).  

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:848 on 2023-05-02 06:57_

Is my understanding correct that ruff now emits two diagnostics for the same locations if both the `MutableDataclassDefault` and `MutableClassDefault` rules are enabled? 

I think we should avoid this when possible. Is the motivation of having two different rules so that we can show two different messages? We could e.g. parameterize the diagnostic with the class kind to emit two different messages but implement it as a single rule. @charliermarsh any thoughts on this?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-02 07:01_

Oh I see, the nesting in the call-site is different than I thought. But does that mean that Ruff now emits two errors when seeing a non-immutable function call: Once the dataclass error and once the normal error if both rules are enabled? 

@charliermarsh what's your opinion on merging the rules and/or parametrizing the diagnostics with whether it is a dataclass or not? 

---

_@MichaReiser reviewed on 2023-05-02 07:01_

---

_@adampauls reviewed on 2023-05-09 16:45_

---

_Review comment by @adampauls on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-09 16:45_

How about this: I'll put up a PR that expands RUF008 to all classes, not just dataclasses. I think that's uncontroversial, since mutable defaults are never a good idea. There are some issues with RUF009 because we need exemptions for `field_specifiers` in general for `@dataclass_transforms`, which is not easy to do in general. Then we can sort out whether we want to keep RUF009 scoped to dataclasses or expand it to any class with exemptions for dataclass-like things.

---

_@MichaReiser reviewed on 2023-05-10 07:21_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-10 07:21_

Sounds good to me. @charliermarsh what's your take?

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_class_fields.rs`:216 on 2023-05-12 21:58_

I've added my opinion over in https://github.com/charliermarsh/ruff/pull/4390#issuecomment-1546348728

---

_@zanieb reviewed on 2023-05-12 21:58_

---

_Comment by @charliermarsh on 2023-06-12 16:55_

Closing in favor of #4390.

---

_Closed by @charliermarsh on 2023-06-12 16:55_

---
