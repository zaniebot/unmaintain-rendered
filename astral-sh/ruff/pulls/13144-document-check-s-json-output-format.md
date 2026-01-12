```yaml
number: 13144
title: "Document `check`'s JSON output format"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - documentation
assignees: []
base: main
head: main
created_at: 2024-08-29T00:55:42Z
updated_at: 2024-09-18T08:17:31Z
url: https://github.com/astral-sh/ruff/pull/13144
synced_at: 2026-01-12T15:55:43Z
```

# Document `check`'s JSON output format

---

_@InSyncWithFoo_

Sources:

* `OneIndexed`: [`line_index.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_source_file/src/line_index.rs#L294)
* `Applicability`: [`fix.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_diagnostics/src/fix.rs#L12-L24)
* `SourceLocation`: [`lib.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_source_file/src/lib.rs#L234-L237)
* `ExpandedEdit`: [`json.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_linter/src/message/json.rs#L155-L159)
* `Fix`: [`json.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_linter/src/message/json.rs#L56-L60)
* `Message`: [`json.rs`](https://github.com/astral-sh/ruff/blob/770ef2ab2719de85c8beec348595f1c74177ce7e/crates/ruff_linter/src/message/json.rs#L81-L90)

`Applicability::DisplayOnly` doesn't seem to be used anywhere, but I guess its serialized value also follows the lowercase rule.

---

_Label `documentation` added by @AlexWaygood on 2024-08-29 10:20_

---

_@AlexWaygood reviewed on 2024-08-29 10:34_

Thanks! This all looks correct from what I can tell. I guess my only concern would be whether we can remember to keep it up to date

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-08-29 10:34_

---

_@dhruvmanila reviewed on 2024-08-29 11:22_

---

_Review comment by @dhruvmanila on `docs/linter.md`:379 on 2024-08-29 11:22_

This might be a nit but I don't think this is correct because `OneIndexed` is a custom type defined at:

https://github.com/astral-sh/ruff/blob/f99af16bdae85829a0fa9b00168e7cad23bd7b00/crates/ruff_source_file/src/line_index.rs#L287-L294

which is a range from 1 to `usize::MAX` (depends on the platform). So, it wouldn't include 0 nor negative integers.

---

_@AlexWaygood reviewed on 2024-08-29 11:24_

---

_Review comment by @AlexWaygood on `docs/linter.md`:379 on 2024-08-29 11:24_

How would you express this in Python -- via a subclass? `class OneIndexed(int): ...`?

---

_@dhruvmanila reviewed on 2024-08-29 11:27_

What's the intended use-case for this representation? I'm not exactly sure if it's useful to represent this in terms of a language. Maybe we could use a table with three columns (field name, type, description) to make it language agnostic? I agree that we should have the output format somewhere in the documentation but I'm not sure what the format should be like. I'd ideally prefer it to be not tied to a specific language.

---

_@dhruvmanila reviewed on 2024-08-29 11:29_

---

_Review comment by @dhruvmanila on `docs/linter.md`:379 on 2024-08-29 11:29_

I think I'm more interested in understanding the use-case of this representation first. I agree that we should have the output format in some form in the documentation but I'm not sure what the format should be like.

---

_@InSyncWithFoo reviewed on 2024-08-29 11:49_

---

_Review comment by @InSyncWithFoo on `docs/linter.md`:379 on 2024-08-29 11:49_

I intentionally used a type alias and not a new type/class, since this is not for use in real code but for documentation purposes only. Similar choices were also made by, among others, [the Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#baseTypes):

```typescript
/**
 * Defines an integer number in the range of -2^31 to 2^31 - 1.
 */
export type integer = number;
```

I understand that this choice might cause some confusion. I could add some descriptions if they are necessary.

---

_Comment by @InSyncWithFoo on 2024-08-29 11:57_

I need to parse Ruff's output programmatically. Since I'm using a statically typed language, a concrete schema is a must. I picked Python because readers of Ruff's documentation are expected to be familiar with it, and TypeScript for its popularity as well as flexibility and relatable syntax. If language-agnosticism is a goal, we could also pick, say, [JSON schema](https://json-schema.org/specification).

I would say tabular formats are nowhere as expressive as code snippets. If a reader cares enough about machine-readable output formats, they must also know what interfaces are, in which case they should be able to understand the snippets (in TS's case, even if they don't know the language).

---

_Comment by @dhruvmanila on 2024-08-30 08:49_

As it's only for documentation purpose, I think it might be better to use another format as I think, as a user, I would mainly be interested in the fields, their types and what the information is about along with some examples as a reference.

@charliermarsh any thoughts?

---

_Comment by @MichaReiser on 2024-09-02 06:52_

I'm worried that this gets outdated very quickly and we currently don't have a good infrastructure to generate typescript or python types from source (unlike BiomeJS for JS). 

I would prefer if we generated and committed a JSON schema instead, to which we can link, similar to what we do with the configuration schema. We already have the infrastructure for it in place, and it doesn't require manual changes to keep it up to date.

---

_Comment by @dhruvmanila on 2024-09-02 07:06_

> I'm worried that this gets outdated very quickly

I doubt that this will happen mainly because the output format doesn't really change that often. I think the last time it was changed was for the syntax error in 0.5 and before that was to add the `cell` field. I agree that there's no way to automate this though.

> I would prefer if we generated and committed a JSON schema instead, to which we can link, similar to what we do with the configuration schema. We already have the infrastructure for it in place, and it doesn't require manual changes to keep it up to date.

I think this will require adding a structure for the same as currently the code constructs the string directly:

https://github.com/astral-sh/ruff/blob/c594ebe0e4bd0ca0b33e64f31540407f35c11bb0/crates/ruff_linter/src/message/json.rs#L51-L92

Adding a struct can also be used to generate the user facing documentation like https://docs.astral.sh/ruff/settings/ instead of linking to the JSON schema.

---

_Comment by @MichaReiser on 2024-09-18 07:26_

Let me know if you plan to continue working on this and we can re-open the PR. 

---

_Closed by @MichaReiser on 2024-09-18 07:26_

---

_Comment by @InSyncWithFoo on 2024-09-18 08:17_

@MichaReiser I do plan to, but if it's preferred that the schema be automatically generated and the logic rewritten to accommodate that, then I'm fine with this PR being closed.

---
