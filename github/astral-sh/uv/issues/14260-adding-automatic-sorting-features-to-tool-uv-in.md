---
number: 14260
title: "Adding automatic sorting features to [tool.uv] in pyproject.toml"
type: issue
state: closed
author: ya7010
labels:
  - enhancement
assignees: []
created_at: 2025-06-25T13:15:12Z
updated_at: 2025-06-27T14:09:40Z
url: https://github.com/astral-sh/uv/issues/14260
synced_at: 2026-01-07T13:12:18-06:00
---

# Adding automatic sorting features to [tool.uv] in pyproject.toml

---

_Issue opened by @ya7010 on 2025-06-25 13:15_

### Summary

Hi

I am currently writing a TOML Language Server called [Tombi](https://github.com/tombi-toml/tombi), and developing features for uv that make it easier to develop workspaces in Python (such as moving definitions between workspaces).

Tombi has an auto-sort feature, but [it needs additional information in the JSON Schema](https://tombi-toml.github.io/tombi/docs/json-schema).

Would you be interested in adding `x-tombi-*` for Tombi to uv's JSON Schema?

I am willing to submit a PR.

### Example

This is Cargo.toml, but it is an example of automatic sorting.

https://github.com/user-attachments/assets/15abc0a5-b3dd-4da1-a2d3-8fff7932f834

Tombi uses Schemars to add extends.
https://github.com/tombi-toml/tombi/blob/cf3402bbd2ad21a166949ae7a2f18b5f664c1bad/crates/tombi-config/src/lib.rs#L33

---

_Label `enhancement` added by @ya7010 on 2025-06-25 13:15_

---

_Renamed from "Adding automatic sorting annotations in pyproject.toml" to "Adding automatic sorting features in [tool.uv] of pyproject.toml" by @ya7010 on 2025-06-25 13:23_

---

_Renamed from "Adding automatic sorting features in [tool.uv] of pyproject.toml" to "Adding automatic sorting features to [tool.uv] in pyproject.toml" by @ya7010 on 2025-06-25 13:23_

---

_Comment by @konstin on 2025-06-27 12:18_

Why do you need the order in the schema itself, instead of using the existing one (as `uv add` does) or defining one that works for your plugin?

---

_Comment by @ya7010 on 2025-06-27 12:57_

Because Tombi targets more general TOML.

There are tools that do not offer features such as `uv add`,
and tools that simply append to the end of the table without auto-sorting.

In addition, even for normal properties rather than additionalProperties, there are requests for the order, such as wanting the name to come first.

[Similar discussions are taking place in JSON Schema](https://github.com/json-schema-org/json-schema-vocabularies/issues/7), and I think the order of properties is within the scope of the Schema's role.

Also, Tombi doesn't provide any third-party extensions right now.
Auto-sorting should be easy to provide, and JSON Schema Store was picked as a place to get info without reading the docs.


---

_Comment by @konstin on 2025-06-27 13:04_

At the current time, I don't think we want to add `x-tombi-*` metadata to the uv schema.

---

_Comment by @ya7010 on 2025-06-27 13:15_

I see.  I think the implementation should follow the decision of the project side.

Simply curious, what are some of the reasons why you are not looking for automatic sorting?

I met a user on the forum who said that he has disabled auto sorting because he has created a group with roles in dependencies.

Is there the same or another reason ?

---

_Comment by @konstin on 2025-06-27 13:20_

Sorting keys in an external tool is fine, it's just that we're always hesitant to add output that only applies for a specific tool.

---

_Comment by @ya7010 on 2025-06-27 13:52_

That is a plausible reason!

I think we are done discussing here, so I will close this Issue.

---

_Closed by @ya7010 on 2025-06-27 13:52_

---
