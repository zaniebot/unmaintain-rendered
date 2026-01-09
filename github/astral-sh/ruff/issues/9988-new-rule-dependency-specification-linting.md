---
number: 9988
title: "New rule: dependency specification linting"
type: issue
state: open
author: QMalcolm
labels:
  - rule
assignees: []
created_at: 2024-02-14T16:33:49Z
updated_at: 2024-02-15T04:25:27Z
url: https://github.com/astral-sh/ruff/issues/9988
synced_at: 2026-01-07T13:12:15-06:00
---

# New rule: dependency specification linting

---

_Issue opened by @QMalcolm on 2024-02-14 16:33_

# Context

Working in many python projects over the years, I've experienced on occasion _bad things happening_ ᵀᴹ due to the accidental allowance of more than one major version of a package via dependency specification. Sometimes this is known and intentional, other times it is accidental and dangerous. A recent example of this that I experienced was that I [upgraded dbt-semantic-interfaces](https://github.com/dbt-labs/dbt-semantic-interfaces/pull/238) to support both Pydantic 1 and Pydantic 2 (intentionally). A downstream package installed alongside `dbt-semantic-interfaces` had it's pydantic dependency specified as `pydantic>=1.0,<=2.0`. However, it was never intended to actually support Pydantic 2. It should have been `pydantic>=1.0,<2.0`, but because of the `<=` it started blowing up at runtime.

# Desired Improvement

It'd be amazing to have a rule for linting python dependencies that ensures the allowance of multiple major versions of a dependency is always known and intentional. In my mind there are a few different types of dependency major version range specification that exist: unconstrained, constrained, and singular.

## Singular Major Version

A singular major version specification means that only one major version for a given dependency is allowed by the specification. Thus, the linter should never raise an error for singular major version specifications of dependencies. Singular major version specifications look like the following

```
# only ever 1.5.7, thus only major version 1
package_a == 1.5.

# Allows for versions >= 1.0 and < 2, thus only major version 1
package_b ~= 1.0

# Allows for exactly what it says, thus only major version 1     
package_c >= 1.3.4, <2
```

## Constrained Major Version

A constrained major version means that there is an upper bound specification, and the major version of the upper bound specification is different that the specified or implicit lower bound specification (if a lower bound isn’t specified it is implicitly 0.0). 

This type of version specification is dangerous because major versions are generally where large breaking changes happen. Sometimes there is compatibility between major versions or compatibility might be possible through shimming, but this should be known and intentional. Thus, the linter should raise an error for this type of dependency version specification unless there is a comment to acknowledge it like `# ignore:constrained_range` or something similar. The following are examples of constrained major version specifications

```
# major versions allowed are [0, 1], an error should be raised
package_a < 2

# major versions allowed are [1, 2, 3], an error should be raised
package_b >= 1.0, <= 3

# major versions allowed are [1, 2], an error should be raised
package_c >= 1.0, < 3
```

Again all the above should have an error raised by the linter unless an acknowledgement is made that this is intentional like the following

```
# major versions allowed are [1, 2]
package_a >= 1.0, < 3  # ignore:constrained_range
```

## Unconstrained Major Version

An unconstrained major version means that lower bound is specified or implicit, but no upper bound is provided. These are incredibly dangerous as large breaking changes are normal across major versions, this type of specification allows for any new major version to be automatically included as available. This type of version specification _should always raise_ an error by the linter and not be ignorable. The following are examples unconstrained major version specifications

```
# allows for major versions [0, 1, 2, ..., infinitey]
package_a

# allows for major versions [2, 3, 4, ..., infinitey]
package_b >= 2
```

---

I'm happy to help contribute this, although I'm not super well versed in Rust. I'm currently working on a prototype in python.

---

_Renamed from "New rule: dependency linting" to "New rule: dependency specification linting" by @QMalcolm on 2024-02-14 16:36_

---

_Label `rule` added by @MichaReiser on 2024-02-14 16:45_

---
