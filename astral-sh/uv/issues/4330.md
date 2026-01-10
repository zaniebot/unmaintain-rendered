```yaml
number: 4330
title: "`RegistryClientBuilder` should support creation from `BaseClientBuilder`"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - internal
assignees: []
created_at: 2024-06-14T17:18:09Z
updated_at: 2024-08-20T14:23:54Z
url: https://github.com/astral-sh/uv/issues/4330
synced_at: 2026-01-10T04:53:49Z
```

# `RegistryClientBuilder` should support creation from `BaseClientBuilder`

---

_Issue opened by @zanieb on 2024-06-14 17:18_

_No description provided._

---

_Label `good first issue` added by @zanieb on 2024-06-14 17:18_

---

_Label `internal` added by @zanieb on 2024-06-14 17:18_

---

_Comment by @danielenricocahall on 2024-06-29 21:59_

I can take this one, as I'm interested in learning and contributing!

---

_Comment by @zanieb on 2024-06-29 22:48_

Awesome! Let me know if you want some guidance on the goals. I pointed out the problem we're trying to solve over in https://github.com/astral-sh/uv/pull/4326#discussion_r1640139844 but didn't provide much detail.

---

_Comment by @danielenricocahall on 2024-06-30 04:31_

Hey @zanieb ! I think I may take you up on that 😅 . I've been pondering it - we could add another field to the struct:

```rust

    base_client_builder: Option<BaseClientBuilder<'a>>

```

then do something like the following in the `build` function:

```rust
        let mut builder = self.base_client_builder.unwrap_or_else(BaseClientBuilder::new);
        ...
        let client = builder
            .retries(self.retries.unwrap_or(builder.retries))
            .connectivity(self.connectivity.unwrap_or(builder.connectivity))
            .native_tls(self.native_tls.unwrap_or(builder.native_tls))
            .keyring(self.keyring.unwrap_or(builder.keyring))
            .build();
```

but I'm not sure if that's clunky or over-engineering for the duplication, so I would love your feedback (this is also my first time dabbling in Rust, so I may be trying to fit a square peg into a circular hole with this approach).

---

_Comment by @zanieb on 2024-06-30 06:25_

I think I'd add a `base_client_builder` field to `RegistryClientBuilder` but I wouldn't make it an `Option`. I think we'd create it immediately in `RegistryClientBuilder::new` then mutate it in the relevant helpers (e.g. when changing `native_tls`) rather than storing all that state on the `RegistryClientBuilder` directly. Does that make sense?

Next you can add a `From` implementation to make it easy to create a registry client builder from an existing base

```rust
impl From<BaseClientBuilder> for RegistryClientBuilder ...
```


---

_Comment by @danielenricocahall on 2024-06-30 14:01_

Thank you! I understand now - I think I have the updates in place I can push up and make a PR soon. One gap for adding the trait is `RegistryClientBuilder` needs a `cache` and I don't believe there is a default value to use e.g;

```rust
impl<'a> From<BaseClientBuilder<'a>> for RegistryClientBuilder<'a> {
    fn from(value: BaseClientBuilder) -> Self {
        Self {
            index_urls: IndexUrls::default(),
            index_strategy: IndexStrategy::default(),
            cache: ..., // Unsure how to approach this unless we add a separate helper function or trait
            base_client_builder: BaseClientBuilder::default(),
            client: None,
            markers: None,
            platform: None,
        }
    }
}



```

---

_Comment by @zanieb on 2024-08-20 14:23_

This happened in #4729 

---

_Closed by @zanieb on 2024-08-20 14:23_

---
