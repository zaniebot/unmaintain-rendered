```yaml
number: 9601
title: "[`flake8-bugbear`] Implement `class-as-data-structure` (`B903`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - accepted
  - preview
assignees: []
merged: true
base: main
head: add-R0903
created_at: 2024-01-21T22:34:35Z
updated_at: 2025-01-07T03:20:59Z
url: https://github.com/astral-sh/ruff/pull/9601
synced_at: 2026-01-10T20:34:00Z
```

# [`flake8-bugbear`] Implement `class-as-data-structure` (`B903`)

---

_Pull request opened by @diceroll123 on 2024-01-21 22:34_

## Summary

Adds `class-as-data-structure` rule (`B903`).

Took some creative liberty with this by allowing the class to have any decorators or base classes. There are years-old issues on pylint that don't approve of the strictness when it comes to these things.

Especially considering that dataclass is a decorator and namedtuple _can be_ a base class. I feel ignoring those explicitly is redundant all things considered, but it's not a hill I'm willing to die on!

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-21 22:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+40 -0 violations, +0 -0 fixes in 10 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L437'>tests/particles/test_decorators.py:437:5:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L456'>tests/particles/test_decorators.py:456:5:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators.py#L494'>tests/particles/test_decorators.py:494:5:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8da7619b617b1eb8b4aabdb1aeb18655065cf6a0/tests/particles/test_decorators_annotations.py#L16'>tests/particles/test_decorators_annotations.py:16:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1208679cd8ab68852ca11a80e1918e948ed1eaf0/airflow/www/security_appless.py#L27'>airflow/www/security_appless.py:27:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/apache/airflow/blob/1208679cd8ab68852ca11a80e1918e948ed1eaf0/providers/src/airflow/providers/celery/executors/celery_executor_utils.py#L200'>providers/src/airflow/providers/celery/executors/celery_executor_utils.py:200:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/apache/airflow/blob/1208679cd8ab68852ca11a80e1918e948ed1eaf0/providers/src/airflow/providers/google/cloud/operators/dataflow.py#L64'>providers/src/airflow/providers/google/cloud/operators/dataflow.py:64:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/9e17304523f74cb97558cbb1e78c57e4f2045a14/superset/views/base_api.py#L133'>superset/views/base_api.py:133:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/187131c55c1b788da38124c6e7917151249746d6/libs/core/tests/unit_tests/tracers/test_langchain.py#L103'>libs/core/tests/unit_tests/tracers/test_langchain.py:103:5:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/315b54908bd0dc8a7bc913a38fb22f03377eb6eb/pandas/io/sas/sas7bdat.py#L91'>pandas/io/sas/sas7bdat.py:91:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/ffbf6f0ce61170d6437ad5ff3a90086200ba9e2a/tests/lib/__init__.py#L1347'>tests/lib/__init__.py:1347:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/pypa/pip/blob/ffbf6f0ce61170d6437ad5ff3a90086200ba9e2a/tests/unit/test_network_auth.py#L331'>tests/unit/test_network_auth.py:331:5:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/misc/perf_checker.py#L14'>misc/perf_checker.py:14:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypy/nodes.py#L659'>mypy/nodes.py:659:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypy/plugins/attrs.py#L96'>mypy/plugins/attrs.py:96:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypy/stubgen.py#L182'>mypy/stubgen.py:182:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypy/stubutil.py#L326'>mypy/stubutil.py:326:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypy/test/teststubtest.py#L189'>mypy/test/teststubtest.py:189:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/python/mypy/blob/20355d5c9b54e2349d1eff49f0d635562d7acdaf/mypyc/ir/class_ir.py#L453'>mypyc/ir/class_ir.py:453:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/eae15e3a83efa9552a2c5f4bfa5305fb3586333c/tests/units/utils/test_serializers.py#L53'>tests/units/utils/test_serializers.py:53:5:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial002.py#L11'>docs_src/dependencies/tutorial002.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial002_an.py#L12'>docs_src/dependencies/tutorial002_an.py:12:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial002_an_py310.py#L11'>docs_src/dependencies/tutorial002_an_py310.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial002_an_py39.py#L11'>docs_src/dependencies/tutorial002_an_py39.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial002_py310.py#L9'>docs_src/dependencies/tutorial002_py310.py:9:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial003.py#L11'>docs_src/dependencies/tutorial003.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial003_an.py#L12'>docs_src/dependencies/tutorial003_an.py:12:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial003_an_py310.py#L11'>docs_src/dependencies/tutorial003_an_py310.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial003_an_py39.py#L11'>docs_src/dependencies/tutorial003_an_py39.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial003_py310.py#L9'>docs_src/dependencies/tutorial003_py310.py:9:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial004.py#L11'>docs_src/dependencies/tutorial004.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial004_an.py#L12'>docs_src/dependencies/tutorial004_an.py:12:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial004_an_py310.py#L11'>docs_src/dependencies/tutorial004_an_py310.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial004_an_py39.py#L11'>docs_src/dependencies/tutorial004_an_py39.py:11:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/dependencies/tutorial004_py310.py#L9'>docs_src/dependencies/tutorial004_py310.py:9:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/docs_src/python_types/tutorial010.py#L1'>docs_src/python_types/tutorial010.py:1:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/tests/test_jsonable_encoder.py#L17'>tests/test_jsonable_encoder.py:17:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/tiangolo/fastapi/blob/144f09ea146b2cc026bf317f730aa0e0dbc3de24/tests/test_jsonable_encoder.py#L22'>tests/test_jsonable_encoder.py:22:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b4fe15c8aafa9a423cd4d24c24efa61e89436a3/confirmation/models.py#L252'>confirmation/models.py:252:1:</a> B903 Class could be dataclass or namedtuple
+ <a href='https://github.com/zulip/zulip/blob/0b4fe15c8aafa9a423cd4d24c24efa61e89436a3/zerver/lib/markdown/__init__.py#L390'>zerver/lib/markdown/__init__.py:390:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B903 | 40 | 40 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @diceroll123 on 2024-01-21 23:47_

---

_Marked ready for review by @diceroll123 on 2024-01-22 00:30_

---

_Comment by @zanieb on 2024-01-24 16:16_

Hi! Thanks for implementing this.

I'm hesitant to include this rule  in Ruff. It looks like "too few methods" is used as a proxy for "this should be a dataclass" — it seems like a rule like "it only has an `__init__`, it should be a dataclass" would make more sense? Are there more use cases for this rule?

Separately, there's a false positive here for [nested classes](https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/api_connexion/schemas/task_instance_schema.py#L42-L45) used for configuration — perhaps those should be excluded from the rule?

---

_Label `rule` added by @MichaReiser on 2024-01-24 16:18_

---

_Comment by @diceroll123 on 2024-01-25 01:49_

> Hi! Thanks for implementing this.
> 
> I'm hesitant to include this rule in Ruff. It looks like "too few methods" is used as a proxy for "this should be a dataclass" — it seems like a rule like "it only has an `__init__`, it should be a dataclass" would make more sense? Are there more use cases for this rule?
> 
> Separately, there's a false positive here for [nested classes](https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/api_connexion/schemas/task_instance_schema.py#L42-L45) used for configuration — perhaps those should be excluded from the rule?

Tweaked accordingly!

Changing up the name of the rule slightly may be in order, I figure? 🤔 Please advise!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_few_public_methods.rs`:11 on 2024-02-26 12:46_

```suggestion
/// without base classes and 0 decorators.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_few_public_methods.rs`:38 on 2024-02-26 12:50_

I took a look at [PEP557](https://peps.python.org/pep-0557/) that introduced data classes and they describe them as classes that preliminary store values ([source](https://peps.python.org/pep-0557/#rationale)). From that, how about `ValueClass` (reads as `allow(ValueClass)` or `disallow(ValueClass)`). 


From the PEP, it sounds to me that all fields must have type annotations or using `dataclass` isn't allowed. If that's correct, we should add checks that verify that all fields have a type annotaiton.

---

_@MichaReiser reviewed on 2024-02-26 12:52_

I left an inline comment with a rule name suggestion. @AlexWaygood do you have ideas/opinions on a name for this rule?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/too_few_public_methods.rs`:15 on 2024-02-26 17:22_

I'm not sure "lightweight" is accurate (or at least, it feels imprecise)... I believe both namedtuples and dataclasses will result in you having slower import times, and I don't think they'll get you any speedup after the class is imported either.

Both lead to less _boilerplate_ (and it's possible `namedtuple` might lead to less memory usage? not sure)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/too_few_public_methods.rs`:38 on 2024-02-26 17:24_

> From the PEP, it sounds to me that all fields must have type annotations or using `dataclass` isn't allowed. If that's correct, we should add checks that verify that all fields have a type annotaiton.

This is true for dataclasses, but it's not true for `namedtuples`, FWIW. There's `typing.NamedTuple`, which does require type annotations, but there's also an older form `collections.namedtuple`, which does not.

---

_@AlexWaygood reviewed on 2024-02-26 17:26_

---

_Comment by @AlexWaygood on 2024-02-26 17:29_

> @AlexWaygood do you have ideas/opinions on a name for this rule?

Possibly something like `consider-using-dataclass` could work well... but there might be other reasons you might want to consider using a dataclass, of course (such as code generation for several useful methods like `__repr__`, `__eq__`, etc.)!

I think the current name is _okay_, albeit a bit verbose, and there is some advantage to it having the same name as the original pylint rule

---

_Comment by @MichaReiser on 2024-02-26 17:33_

> Possibly something like consider-using-dataclass could work well... but there might be other reasons you might want to consider using a dataclass, of course (such as code generation for several useful methods like __repr__, __eq__, etc.)!

I like the name but it, unfortunately, doesn't fit our [rule naming convention](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention)

> I think the current name is okay, albeit a bit verbose, and there is some advantage to it having the same name as the original pylint rule

We can use the same rule code as pylint, but I would prefer another name that's more explicit about its motivation. Having a different name might also help to set expectations, considering that we're probably changing the semantics of the rule.

---

_@MichaReiser reviewed on 2024-02-26 17:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/too_few_public_methods.rs`:38 on 2024-02-26 17:34_

Hmm, you're saying it's complicated ;)

I would prefer being more strict and setting the baseline to what's required for data classes (type annotations), considering that the fields used to be named. It otherwise becomes necessary to suppress the rule if users don't want to use a tuple, but can't use a data class.

---

_Comment by @AlexWaygood on 2024-02-26 17:40_

Maybe something like `NonDataclassSimpleClass`?

---

_Comment by @MichaReiser on 2024-02-27 17:21_

This is interesting https://github.com/astral-sh/ruff/pull/10146 I  haven't looked at how the two rules overlap but maybe there's an opportunity.

---

_Comment by @AlexWaygood on 2024-02-27 17:22_

> This is interesting #10146 I haven't looked at how the two rules overlap but maybe there's an opportunity.

That is interesting, indeed... it's flagging a different antipattern, but one that has the same solution

---

_Comment by @diceroll123 on 2024-03-15 04:57_

Let me know if there's any action items blocking this PR, not sure what to make of these comments tbh

---

_Label `packaging` added by @MichaReiser on 2024-04-05 10:20_

---

_Label `packaging` removed by @MichaReiser on 2024-04-05 10:20_

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:20_

---

_Comment by @MichaReiser on 2024-04-05 10:22_

Note for reviewing: The rule itself is a style rule but we need to find a good name that expresses the motivation to use dataclasses/named tuples instead of enforcing a number of public methods. The name should read as `allow(rule-name)`.

But the intent of the rule itself fits into ruff as a style rule.

---

_Comment by @Rizhiy on 2024-09-09 22:43_

@zanieb @MichaReiser  Is there any progress on merging this? I think it would be a beneficial rule to add.

In addition to suggesting data-classes this also covers the case where people create empty "base" classes, which is an antipattern in my opinion.

Ideally, I would also like to have a rule which flags classes with `__init__` and one public classes (no decorators, no parent classes). I believe that is another antipattern, such classes are frequently better refactored to functions.

---

_Comment by @MichaReiser on 2024-09-10 20:34_

> @zanieb @MichaReiser Is there any progress on merging this? I think it would be a beneficial rule to add.

Not much other than that we have to come up with a good rule name that captures the motivation. Do you have any suggestions for a good rule name?

---

_Comment by @Rizhiy on 2024-09-10 21:46_

> Not much other than that we have to come up with a good rule name that captures the motivation. Do you have any suggestions for a good rule name?

What is the naming convention/naming rules?

IMHO, there are three different rules here, they can be named:
* For empty base class: `unnecessary-base-class`
* For dataclass case (no public methods except `__init__`): `prefer-dataclass`
* For class with `__init__` and one public method: `potential-obfuscated-function`

---

_Comment by @MichaReiser on 2024-09-11 19:18_

I guess the blockers are now if it should be one or three rules and how they should be named 😅 

---

_Comment by @Rizhiy on 2024-09-11 19:42_

> I guess the blockers are now if it should be one or three rules and how they should be named 😅 

If the names are satisfactory I can submit a new PR with three rules.

---

_Comment by @dylwil3 on 2024-12-16 20:24_

Hey @diceroll123 , sorry to leave this simmering for so long!

If you're still interested in working on this, I bet we can get it over the line.

I think the immediate non-name-related blockers for this PR are the following:

1. Resolve merge conflicts
2. Update the doc-comment as Alex suggested (so replace "are more lightweight" with "have less boilerplate")
3. Update the logic of the lint rule to require that the parameters of `__init__` have type annotations
4. Can you add a test case that has (annotated) class variables and an `__init__` and nothing else? Also would be good to add some examples with docstrings and comments. I just want to document the behavior we have for these.

If we get everything but the name in a good spot, then we can always merge it in with the current name and change it later (changing human-readable names is not a breaking change.)

And since I can't help but also suggest a name: one possibility is `class-as-data-structure`, since both dataclasses and namedtuples are a sort of "bare" data structure.

---

_Label `preview` added by @dhruvmanila on 2024-12-17 04:31_

---

_Comment by @diceroll123 on 2024-12-19 06:13_

Sounds good @dylwil3, I'll get around to it this weekend or so.

---

_Renamed from "[`pylint`] - Implement `too-few-public-methods` rule (`PLR0903`)" to "[`flake8-bugbear`] Implement `class-as-data-structure` rule (`B903`)" by @dylwil3 on 2025-01-03 23:01_

---

_Renamed from "[`flake8-bugbear`] Implement `class-as-data-structure` rule (`B903`)" to "[`flake8-bugbear`] Implement `class-as-data-structure` (`B903`)" by @dylwil3 on 2025-01-03 23:01_

---

_Comment by @dylwil3 on 2025-01-03 23:06_

Just some updates here:

1. I've made the rule rather strict, much closer to the bugbear implementation, since there seemed to be quite a few false positives in the ecosystem results. There were classes in the ecosystem results where the suggestion to replace with a namedtuple or dataclass didn't make any sense because there were things happening in the `__init__` function that were too complicated to appear in either of these structures.
2. With that in mind, I've moved the rule to `B903` and changed the name to better reflect the behavior.

Gonna check out the ecosystem results and possibly update the documentation, but otherwise I think this is close to done unless there are any objections!

Update: Ecosystem results look much better to me now!

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-01-06 03:10_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-01-06 03:10_

---

_@MichaReiser reviewed on 2025-01-06 09:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/class_as_data_structure.rs`:11 on 2025-01-06 09:42_

```suggestion
/// without base classes and decorators.
```

---

_@MichaReiser reviewed on 2025-01-06 09:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/class_as_data_structure.rs`:106 on 2025-01-06 09:43_

```suggestion
/// Checks whether a statement is a, possibly augmented,
/// assignment of a name to an attribute.
fn is_simple_assignment_to_attribute(stmt: &ast::Stmt) -> bool {
```

---

_@MichaReiser reviewed on 2025-01-06 09:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/class_as_data_structure.rs`:92 on 2025-01-06 09:44_

Should we bail if we see any unexpected statement. E.g an `if` statement with a conditional method? 

---

_@MichaReiser reviewed on 2025-01-06 09:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/class_as_data_structure.rs`:115 on 2025-01-06 09:45_

```suggestion
            target.is_attribute_expr() && value.is_name_expr()
        }
        ast::Stmt::AnnAssign(ast::StmtAnnAssign { target, value, .. }) => {
            target.is_attribute_expr() && value.as_ref().is_some_and(|val| val.is_name_expr())
```

---

_@MichaReiser reviewed on 2025-01-06 09:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/class_as_data_structure.rs`:60 on 2025-01-06 09:46_

Should we disable the rule for stub files?

---

_@MichaReiser approved on 2025-01-06 09:46_

---

_Merged by @dylwil3 on 2025-01-07 03:18_

---

_Closed by @dylwil3 on 2025-01-07 03:18_

---

_Comment by @dylwil3 on 2025-01-07 03:20_

Thanks for implementing this @diceroll123 and sorry for the delay on getting it over the line!

---
