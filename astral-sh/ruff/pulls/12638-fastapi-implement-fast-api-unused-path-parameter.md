```yaml
number: 12638
title: "[`fastapi`] Implement `fast-api-unused-path-parameter` (`FAST003`)"
type: pull_request
state: merged
author: Matthieu-LAURENT39
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fastapi_unused_path_parameter
created_at: 2024-08-02T18:10:07Z
updated_at: 2024-08-16T01:55:44Z
url: https://github.com/astral-sh/ruff/pull/12638
synced_at: 2026-01-10T21:38:31Z
```

# [`fastapi`] Implement `fast-api-unused-path-parameter` (`FAST003`)

---

_Pull request opened by @Matthieu-LAURENT39 on 2024-08-02 18:10_

This adds the `fast-api-unused-path-parameter` lint rule, as described in #12632.

I'm still pretty new to rust, so the code can probably be improved, feel free to tell me if there's any changes i should make.  

Also, i needed to add the `add_parameter` edit function, not sure if it was in the scope of the PR or if i should've made another one.

---

_Renamed from "Add fastapi-unused-path-parameter" to "[fastapi] Implement fast-api-unused-path-parameter (FAST003)" by @Matthieu-LAURENT39 on 2024-08-02 23:18_

---

_Marked ready for review by @Matthieu-LAURENT39 on 2024-08-02 23:37_

---

_Comment by @github-actions[bot] on 2024-08-02 23:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:99 on 2024-08-03 07:43_

Regular expressions are fairly expensive to construct. That's why we should try to only construct them once by e.g. using Lazy

https://github.com/astral-sh/ruff/blob/fe7d965334e6299d23ffbaee9c296edd58e0f1c5/crates/ruff_python_formatter/tests/normalizer.rs#L63-L77

But I think we can do even better here and avoid using a regex all together. It seems all we need is to find a `:` and then take everything up to `}`. This can be done by first doing two `split_once` calls (first by `:`, then by `}`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:96 on 2024-08-03 07:44_

I think we can use lifetimes here to avoid allocating strings because we only return sub-slices
```suggestion
fn extract_path_params_from_route(input: &str) -> Vec<(&str, Range<usize>)> {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:154 on 2024-08-03 07:46_

I think we can avoid calling `to_string` here and instead just use the borrowed `&str`
```suggestion
        .map(|arg| &arg.parameter.name)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:146 on 2024-08-03 07:58_

It would be nice if we can avoid returning a `Vec` from `extract_path_params_from_route` to reduce the number of allocations (allocations are rather expensive). 

I think it could be possible but you probably have to write your own iterator type

```rust
fn extract_path_params_from_route(input: &str) -> PathParamsIter {
	PathParamsIter {
		rest: input,
		offset: TextSize::default()
	}
}

struct PathParamsIter<'a> {
	rest: &'a str,
	offset: TextSize,
}

impl<'a> Iterator for PathParamsIter<'a> {
	type Item = (&'a str, TextRange);

	fn next(&mut self) {
	 // TODO do we have to split off the `{`, `:` and `}` from name etc?
		let (before_curly, after_curly) = self.rest.split_once('{')?;
		let (name, after_colon) = after_curly.split_once(':')?;
		let (up_to_curly, rest) = after_colon.split_once('}')?;
		let offset = self.offset;
		let len = self.rest.text_len() - rest.text_len();

		self.offset += len;
		self.rest = rest;

		Some(name.trim(), TextRange::at(offset, len)
	}
}

impl FusedIterator for PathParamsIter<'_> {}
```

---

_@MichaReiser reviewed on 2024-08-03 07:59_

Thanks. Overall looking good. I have to review it a bit more closely but I left a few comments on how we can improve performance.

---

_Comment by @Matthieu-LAURENT39 on 2024-08-03 10:22_

Somehow VSCode didn't show the code blocks inside your reviews so i implemented that from scratch :sweat_smile: 
Works pretty well though

---

_@charliermarsh reviewed on 2024-08-03 12:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-03 12:25_

Since path parameters use the same format as f-strings, could we use our parser here instead? Just parse the path, extract the f-string segments? You can see [here](https://play.ruff.rs/8847ec0d-dcd0-417d-ba75-b372b96a3115) that its evaluated to a literal element, followed by an `FStringExpressionElement` where the `:` part is the `FStringFormatSpec`.

---

_@Matthieu-LAURENT39 reviewed on 2024-08-03 12:42_

---

_Review comment by @Matthieu-LAURENT39 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-03 12:42_

Path parameters can use invalid identifier names. For example, `/route/{path-name}`, which the f-string parser doesn't support. However, we just ignore invalid identifiers in the rule anyways since fastapi normalizes them in an undocumented way, so i don't think this is a blocker to using that parser.
Also, fastapi doesn't handle the `{name!r}` and `{name=}` special syntaxes and would normalize them as `name_r` and `name` respectively, but this is a very obscure edge-case so it doesn't matter much. We do need to at least disable the rule if there is a conversion or debug_text in the f-string to avoid complaining about the wrong name though.
If those aren't a problem, i'll switch to the f-string parser

---

_Comment by @codspeed-hq[bot] on 2024-08-03 16:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Matthieu-LAURENT39:fastapi_unused_path_parameter)

### Merging #12638 will **not alter performance**

<sub>Comparing <code>Matthieu-LAURENT39:fastapi_unused_path_parameter</code> (288bd10) with <code>main</code> (80efb86)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@MichaReiser reviewed on 2024-08-05 08:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-05 08:02_

I leave this up to you @charliermarsh but cloning the AST, then parsing it seems very heavy weight. 

---

_@charliermarsh reviewed on 2024-08-06 02:34_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-06 02:34_

Ok yeah, let's just skip the parser idea for now then.

Does FastAPI allow you to escape curly braces? Like `/foo{{bar}}/` to represent a literal `{` and `}` in the path, as with f-strings?

---

_@Matthieu-LAURENT39 reviewed on 2024-08-06 16:18_

---

_Review comment by @Matthieu-LAURENT39 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-06 16:18_

it doesn't, it will just be treated as if you had a path attribute named `bar` (without curly braces).
Although i'm pretty sure it's because of the aforementioned undocumented normalizing behavior.  

Should i revert the commits using the f-string parser?

---

_Label `rule` added by @MichaReiser on 2024-08-13 13:37_

---

_Label `preview` added by @MichaReiser on 2024-08-13 13:37_

---

_@MichaReiser reviewed on 2024-08-13 14:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:117 on 2024-08-13 14:13_

> Should i revert the commits using the f-string parser?

Yeah I think that would be great, considering that fast-api doesn't support f-strings?

---

_Comment by @Matthieu-LAURENT39 on 2024-08-13 19:40_

reverted to the old parsing system that doesn't use the f-string parser, as per the conversation

---

_Comment by @tiangolo on 2024-08-16 00:39_

Amazing! :rocket: 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-16 01:07_

---

_@charliermarsh reviewed on 2024-08-16 01:09_

Reviewing now!

---

_@charliermarsh approved on 2024-08-16 01:40_

---

_Renamed from "[fastapi] Implement fast-api-unused-path-parameter (FAST003)" to "[`fastapi`] Implement `fast-api-unused-path-parameter` (`FAST003`)" by @charliermarsh on 2024-08-16 01:41_

---

_Merged by @charliermarsh on 2024-08-16 01:46_

---

_Closed by @charliermarsh on 2024-08-16 01:46_

---
