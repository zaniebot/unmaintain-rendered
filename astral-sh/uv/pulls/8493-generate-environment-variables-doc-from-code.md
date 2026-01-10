```yaml
number: 8493
title: Generate environment variables doc from code
type: pull_request
state: merged
author: j178
labels:
  - documentation
assignees: []
merged: true
base: main
head: gen-env
created_at: 2024-10-23T09:09:34Z
updated_at: 2024-11-03T15:46:39Z
url: https://github.com/astral-sh/uv/pull/8493
synced_at: 2026-01-10T11:59:59Z
```

# Generate environment variables doc from code

---

_Pull request opened by @j178 on 2024-10-23 09:09_

## Summary

Resolves #8417

I've just begun learning procedural macros, so this PR is more of a proof of concept. It's still a work in progress, and I welcome any assistance or feedback.


---

_Comment by @samypr100 on 2024-10-30 01:43_

Nice! I was planning to do this at some point, thanks!

---

_@samypr100 reviewed on 2024-10-30 01:48_

---

_Review comment by @samypr100 on `crates/uv-macros/src/lib.rs`:67 on 2024-10-30 01:48_

```suggestion
pub fn attribute_env_vars_metadata(_attr: TokenStream, input: TokenStream) -> TokenStream {
```

Maybe rename this to align with the naming of the other macros and be more explicit that this is for env var metadata?

---

_Review comment by @samypr100 on `crates/uv-macros/src/lib.rs`:79 on 2024-11-01 01:09_

```suggestion
            ImplItem::Const(item)
                if !item.attrs.iter().any(|attr| attr.path().is_ident("attr_hidden")) =>
            {
                let name = item.ident.to_string();
                let doc = item
                    .attrs
                    .iter()
                    .find_map(get_doc_comment)
                    .expect("Missing doc comment");
                Some((name, doc))
            }
```

This is likely useful to allow `#[attr_hidden]` on an env var to prevent it being rendered in the docs using below.

```rust
#[proc_macro_attribute]
pub fn attr_hidden(_attr: TokenStream, item: TokenStream) -> TokenStream {
    item
}
```

Which would allow you to do

```rust
    ...
    /// Used to set the spawning/parent interpreter when using --system in the test suite.
    #[attr_hidden]
    pub const UV_INTERNAL__PARENT_INTERPRETER: &'static str = "UV_INTERNAL__PARENT_INTERPRETER";
    ...
```

---

_@samypr100 reviewed on 2024-11-01 01:09_

---

_@j178 reviewed on 2024-11-01 01:38_

---

_Review comment by @j178 on `crates/uv-macros/src/lib.rs`:79 on 2024-11-01 01:38_

Thanks!

---

_Review comment by @samypr100 on `crates/uv-macros/src/lib.rs`:80 on 2024-11-01 01:48_

Another cool idea is to handle the functions as well with return env var patterns.

```suggestion
            ImplItem::Fn(item)
                if !item.attrs.iter().any(|attr| attr.path().is_ident("attr_hidden")) =>
	            {
                // Extract the environment variable patterns
                if let Some(pattern) = get_env_var_pattern_from_attr(&item.attrs) {
                    let doc = item
                        .attrs
                        .iter()
                        .find_map(get_doc_comment)
                        .unwrap_or_else(|| "No documentation provided.".to_string());
                    Some((pattern, doc))
                } else {
                    None // Skip if pattern extraction fails
                }
            }
            _ => None,
```

Using this extractor

```rust
fn get_env_var_pattern_from_attr(attrs: &[Attribute]) -> Option<String> {
    attrs
        .iter()
        .find(|attr| attr.path().is_ident("attr_env_var_pattern"))
        .and_then(|attr| attr.parse_args::<LitStr>().ok())
        .map(|lit_str| lit_str.value())
}

#[proc_macro_attribute]
pub fn attr_env_var_pattern(_attr: TokenStream, item: TokenStream) -> TokenStream {
    item
}
```

That way, these will also now be in the docs

```rust
    #[attr_env_var_pattern("UV_INDEX_{name}_USERNAME")]
    pub fn index_username(name: &str) -> String {
        format!("UV_INDEX_{name}_USERNAME")
    }

    /// Generates the environment variable key for the HTTP Basic authentication password.
    #[attr_env_var_pattern("UV_INDEX_{name}_PASSWORD")]
    pub fn index_password(name: &str) -> String {
        format!("UV_INDEX_{name}_PASSWORD")
    }
```

---

_@samypr100 reviewed on 2024-11-01 01:48_

---

_Marked ready for review by @j178 on 2024-11-01 05:50_

---

_Renamed from "Generate environment variables doc" to "Generate environment variables doc from code" by @j178 on 2024-11-01 05:50_

---

_@samypr100 reviewed on 2024-11-02 01:08_

---

_Review comment by @samypr100 on `Cargo.toml`:153 on 2024-11-02 01:08_

```suggestion
syn = { version = "2.0.77" }
```

I don't think `features = ["full"]` is needed here?

---

_@j178 reviewed on 2024-11-02 03:14_

---

_Review comment by @j178 on `Cargo.toml`:153 on 2024-11-02 03:14_

Good catch, thanks!

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:124 on 2024-11-02 14:13_

I'm curious why all these changes? I kind of liked the docs here.

---

_@zanieb reviewed on 2024-11-02 14:13_

---

_@j178 reviewed on 2024-11-03 02:25_

---

_Review comment by @j178 on `crates/uv-static/src/env_vars.rs`:124 on 2024-11-03 02:25_

It is copied from the original `docs/configuration/enviroment.md` file. I think it's more user friendly for a document intended for end-users, rather than a developer facing docstring.

---

_@zanieb approved on 2024-11-03 14:31_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-11-03 14:31_

---

_Merged by @zanieb on 2024-11-03 14:31_

---

_Closed by @zanieb on 2024-11-03 14:31_

---

_Branch deleted on 2024-11-03 15:46_

---
