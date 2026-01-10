```yaml
number: 14764
title: "[airflow]: Import modules that has been moved to airflow providers (AIR303)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: AIR303
created_at: 2024-12-04T05:00:01Z
updated_at: 2024-12-18T00:07:18Z
url: https://github.com/astral-sh/ruff/pull/14764
synced_at: 2026-01-10T20:42:27Z
```

# [airflow]: Import modules that has been moved to airflow providers (AIR303)

---

_Pull request opened by @Lee-W on 2024-12-04 05:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Many core Airflow features have been deprecated and moved to Airflow Providers since users might need to install an additional package (e.g., `apache-airflow-provider-fab==1.0.0`); a separate rule (AIR303) is created for this.

As some of the changes only relate to the module/package moved, instead of listing out all the functions, variables, and classes in a module or a package, it warns the user to import from the new path instead of the specific name.

The following is the ones that has been moved to `apache-airflow-provider-fab==1.0.0`

* module moved
    * `airflow.api.auth.backend.basic_auth` → `airflow.providers.fab.auth_manager.api.auth.backend.basic_auth`
    * `airflow.api.auth.backend.kerberos_auth` → `airflow.providers.fab.auth_manager.api.auth.backend.kerberos_auth`
    * `airflow.auth.managers.fab.api.auth.backend.kerberos_auth` → `airflow.providers.fab.auth_manager.api.auth.backend.kerberos_auth`
    * `airflow.auth.managers.fab.security_manager.override` → `airflow.providers.fab.auth_manager.security_manager.override`
* classes (e.g., functions, classes) moved
    * `airflow.www.security.FabAirflowSecurityManagerOverride` → `airflow.providers.fab.auth_manager.security_manager.override.FabAirflowSecurityManagerOverride`
    * `airflow.auth.managers.fab.fab_auth_manager.FabAuthManager` → `airflow.providers.fab.auth_manager.security_manager.FabAuthManager`

https://github.com/astral-sh/ruff/pull/14764

<details>
    <summary><h2>How to contribute to AIR303</h2></summary>

### Browse the airflow / ruff migration guideline
[How to create a Ruff lint rule for Airflow 2-to-3 migrations](https://hackmd.io/@uranusjr/ByyCA_-m1l)

### Setup local development environment

```sh
cargo install cargo-insta

uv tool install pre-commit
pre-commit install

cargo install cargo-nextest --locked
```

[Contributing to Ruff](https://docs.astral.sh/ruff/contributing/)

### Check the existing rules from airflow issue

All the ruff rules are listed down in [Airflow 2 to 3 auto migration rules - ruff #44556](https://github.com/apache/airflow/issues/44556)

Go to `AIR303: moved to provider` in the issue description and you'll find rules like

```markdown
* `airflow.www.security.FabAirflowSecurityManagerOverride` → `airflow.providers.fab.auth_manager.security_manager.override.FabAirflowSecurityManagerOverride` (from https://github.com/apache/airflow/pull/41758)
```

It means `airflow.www.security.FabAirflowSecurityManagerOverride` has been moved to airflow provider and the import should be changed to `airflow.providers.fab.auth_manager.security_manager.override.FabAirflowSecurityManagerOverride`. This changed is made in https://github.com/apache/airflow/pull/41758

Some of the rules might not be correct, so checking the pull request again is suggested.

### Add a new rule

Go to `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`

In function `moved_to_provider`, add code section like the following

```rust
                ["airflow", "www", "security", "FabAirflowSecurityManagerOverride"] => Some((
                    qualname.to_string(),
                    Replacement::ProviderName {
                        name: "airflow.providers.fab.auth_manager.security_manager.override.FabAirflowSecurityManagerOverride",
                        provider: "fab",
                        version: "1.0.0"
                    },
                )),
```

* `"airflow", "www", "security", "FabAirflowSecurityManagerOverride"`: the deprecated import path
* `name` is the new name that should be used.
* `provider` is which provider this name has been moved to.
* `version` is the first version of the provider that has been moved to.


For rules like 

```markdown
* [ ] `airflow.api.auth.backend.basic_auth` → `airflow.providers.fab.auth_manager.api.auth.backend.basic_auth` (from https://github.com/apache/airflow/pull/41663)
```

The whole module/pacakge has been moved to the provider. Thus, we'll need to warn the users when something is imported from the old path instead. 

```rust
                ["airflow", "api", "auth", "backend", "basic_auth", ..] => Some((
                    qualname.to_string(),
                    Replacement::ImportPathMoved{
                        original_path: "airflow.api.auth.backend.basic_auth",
                        new_path: "airflow.providers.fab.auth_manager.api.auth.backend.basic_auth",
                        provider:"fab",
                        version: "1.0.0"
                },
                )),
```

* `"airflow", "api", "auth", "backend", "basic_auth", ..`: anything under this import path
* `original path`
* `new path`: new import path in the provider 
* `provider` is which provider this name has been moved to.
* `version` is the first version of the provider that has been moved to.

### Add test case
Add a case that violate this rule to `crates/ruff_linter/resources/test/fixtures/airflow/AIR303.py`

### Test and generate snapshop to review

```sh
cargo insta test
cargo insta review
```

### Now you're ready to create a pull request
</details>

## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.

---

_Renamed from "feat(AIR303): initial air303" to "[airflow]: Import modules that has been moved to airflow providers (AIR303)" by @Lee-W on 2024-12-04 05:00_

---

_Comment by @github-actions[bot] on 2024-12-09 03:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2024-12-10 11:05_

---

_Comment by @Lee-W on 2024-12-11 05:11_

@MichaReiser would love to know whether the overall idea of this PR makes sense if you have some time. I plan on writing a guideline for folks from the airflow community to contribute to AIR303 more easily. 🙂 

---

_@dhruvmanila reviewed on 2024-12-11 06:47_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 06:47_

I think this is a pretty long message, it might be useful to break it down into two parts - the description and the suggestion. For the description we can avoid the entire import path and just provide the symbol name itself while for the help text we can guide the user to install the provider and use the provider namespace for the symbol. For example:

```
AIR303.py:10:1: AIR303 `basic_auth` will be moved into `fab` provider in Airflow 3.0
   |
 8 | from airflow.www.security import FabAirflowSecurityManagerOverride
 9 | 
10 | basic_auth, kerberos_auth
   | ^^^^^^^^^^ AIR303
11 | auth_current_user
12 | backend_kerberos_auth
   |
   = help: Install `apache-airflow-provider-fab` and import from `airflow.providers.fab` instead
```

The help text is rendered using the `fix_title` method: https://github.com/astral-sh/ruff/blob/a3bb0cd5ec4d4dffea5d42d184e8c4a062976663/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs#L65-L72

I'm also a bit worried about the version number for the package, I'd prefer to omit them as it can get outdated pretty quickly.

Curious to know your thoughts on this :)

---

_Comment by @dhruvmanila on 2024-12-11 06:51_

Is it correct that the entire module itself has been moved to being used as a provider without changing any of the public symbols available in that module? i.e., are the symbols that were available in the old module the same as that in the provider module? If so, we could consider the rule to work on the module instead of individual symbols from the module and suggest to use the provider instead. That way we'll only emit a single diagnostic for the entire module which will be the import statement.

---

_Label `rule` added by @dhruvmanila on 2024-12-11 06:51_

---

_Label `preview` added by @dhruvmanila on 2024-12-11 06:51_

---

_Comment by @Lee-W on 2024-12-11 06:57_

> Is it correct that the entire module itself has been moved to being used as a provider without changing any of the public symbols available in that module? i.e., are the symbols that were available in the old module the same as that in the provider module?

Yep, that's what I think for some of the modules, but I'll confirm it again.

> If so, we could consider the rule to work on the module instead of individual symbols from the module and suggest to use the provider instead. That way we'll only emit a single diagnostic for the entire module which will be the import statement.

Yep, some of the modules can be done this way, but others might not, so that why I'd like to keep the individual symbols check as well

---

_@Lee-W reviewed on 2024-12-11 07:00_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 07:00_

> I'm also a bit worried about the version number for the package, I'd prefer to omit them as it can get outdated pretty quickly.

One of my worries is that it might be removed in the future 🤔 (unlikely, but still possible I think)

---

_Comment by @dhruvmanila on 2024-12-11 07:01_

> Yep, some of the modules can be done this way, but others might not, so that why I'd like to keep the individual symbols check as well

That's sounds reasonable.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-11 07:28_

Do you for see cases where any of the strings are dynamic (contain e.g. an expression)? I'd otherwise suggest to use `&'static str` here to avoid the annoying `to_string` calls and reduce the amount of allocations
```suggestion
enum Replacement {
    ProviderName(&'static str, &'static str &'static str),
    ImportPathMoved(&'static str, &'static str, &'static str, &'static str),
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-11 07:29_

I'd prefer if we use variants with named fields. It's kind of hard to guess what the 4 Strings in `ImportPathMoved` mean
```suggestion
enum Replacement {
    ProviderName { name: String, provider_name: String, version: String},
    ImportPathMoved { original_path: String, new_path: String, provider_name: String, provider_version: String },
}
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 07:31_

> I'm also a bit worried about the version number for the package, I'd prefer to omit them as it can get outdated pretty quickly.

Agree. What's the motivation for including the version? Is it because they need a specific minimal version? 

---

_@MichaReiser approved on 2024-12-11 07:34_

> writing a guideline for folks from the airflow community to contribute to AIR303 more easily. 

This sounds great. I'd appreciate your with reviewing and coordinating the PRs as you have the most knowledge about what the rules should cover. 

---

_@Lee-W reviewed on 2024-12-11 07:36_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 07:36_

> Agree. What's the motivation for including the version? Is it because they need a specific minimal version?

Yep, some module moves might not happen in the `1.0.0` version of that provider.

---

_@Lee-W reviewed on 2024-12-11 07:37_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-11 07:37_

I guess not 🤔  Thanks for your suggestion! I'll change it this way. we can change it back if we really need to do that.

---

_@Lee-W reviewed on 2024-12-11 07:39_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-11 07:39_

Sure! can't believe I missed it 🤦‍♂️ Thanks for your suggestion! Will update it

---

_Comment by @Lee-W on 2024-12-11 07:39_

> > writing a guideline for folks from the airflow community to contribute to AIR303 more easily.
> 
> This sounds great. I'd appreciate your with reviewing and coordinating the PRs as you have the most knowledge about what the rules should cover.

Will do 👍 

---

_@dhruvmanila reviewed on 2024-12-11 08:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 08:19_

We could potentially use `>=1.0.0,<2` presuming that the changes follow semantic versioning. The `1.0.0` can be updated to the provider version in which the change takes place.

---

_@Lee-W reviewed on 2024-12-11 08:20_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 08:20_

sounds good! this is a great suggestion! 

---

_@MichaReiser reviewed on 2024-12-11 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 08:24_

I'm not sure about the upper bound. It would require us to update the rule if a new provider version gets released. Maybe just mention *newer than x.x.x* 

---

_@Lee-W reviewed on 2024-12-11 08:26_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-11 08:26_

After a second thought, yep, it would make things easier, `>=1.0.0` sounds like a good enough solution 🤔 

---

_@Lee-W reviewed on 2024-12-13 09:18_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR303_AIR303.py.snap`:13 on 2024-12-13 09:18_

Updated!

---

_@Lee-W reviewed on 2024-12-13 09:18_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-13 09:18_

fixed. Thanks!

---

_@Lee-W reviewed on 2024-12-13 09:19_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/moved_to_provider_in_3.rs`:22 on 2024-12-13 09:19_

fixed. Thanks!

---

_@MichaReiser approved on 2024-12-13 09:37_

---

_Merged by @MichaReiser on 2024-12-13 09:38_

---

_Closed by @MichaReiser on 2024-12-13 09:38_

---

_Branch deleted on 2024-12-18 00:07_

---
