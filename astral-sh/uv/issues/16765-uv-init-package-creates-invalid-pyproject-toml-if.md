---
number: 16765
title: "`uv init --package` creates invalid pyproject.toml if git username contains quotes"
type: issue
state: closed
author: NMertsch
labels:
  - bug
assignees: []
created_at: 2025-11-17T23:06:59Z
updated_at: 2025-11-20T19:10:13Z
url: https://github.com/astral-sh/uv/issues/16765
synced_at: 2026-01-10T01:26:09Z
---

# `uv init --package` creates invalid pyproject.toml if git username contains quotes

---

_Issue opened by @NMertsch on 2025-11-17 23:06_

### Summary
`uv init` does not escape quotes when inserting the git "user.name" config value as "project.author.name".

#### How to reproduce
Steps:

```shell
# create new directory
mkdir my-new-project
cd my-new-project

# configure git user
git init
git config user.name 'Tony "Iron Man" Stark'
git config user.email 'ironman@example.com'

# create new uv package
uv init --package
```

This creates the following `project.authors` field in pyproject.toml:

```toml
[project]
authors = [
    { name = "Tony "Iron Man" Stark", email = "ironman@example.com" }
]
```

Note the string `"Tony "Iron Man" Stark"`, which should be `"Tony \"Iron Man\" Stark"`

(This came up while testing. I don't suggest that quotes in a git username are a good idea.)

### Platform

Linux 6.17.0-6-generic x86_64 GNU/Linux

### Version

uv 0.9.10

### Python version

Python 3.13.3

---

_Label `bug` added by @NMertsch on 2025-11-17 23:07_

---

_Comment by @NMertsch on 2025-11-17 23:36_

The code is in [crates/uv/src/commands/project/init.rs](https://github.com/astral-sh/uv/blob/512c0ca5edc34f4eb932d5dd59cc86f842a8817c/crates/uv/src/commands/project/init.rs#L938-L946):

```rust
impl Author {
    fn to_toml_string(&self) -> String {
        match self {
            Self::NameEmail { name, email } => {
                format!("{{ name = \"{name}\", email = \"{email}\" }}")  // <-- here
            }
            Self::Name(name) => format!("{{ name = \"{name}\" }}"),  // <-- here
            Self::Email(email) => format!("{{ email = \"{email}\" }}"),
        }
    }
}
```

Using [`toml::to_string()`](https://docs.rs/toml/latest/toml/ser/fn.to_string.html) might be a bit much for this case. Maybe `name.replace("\"", "\\\"")` is enough?

---

_Comment by @terror on 2025-11-18 20:38_

> The code is in [crates/uv/src/commands/project/init.rs](https://github.com/astral-sh/uv/blob/512c0ca5edc34f4eb932d5dd59cc86f842a8817c/crates/uv/src/commands/project/init.rs#L938-L946):
> 
> impl Author {
>     fn to_toml_string(&self) -> String {
>         match self {
>             Self::NameEmail { name, email } => {
>                 format!("{{ name = \"{name}\", email = \"{email}\" }}")  // <-- here
>             }
>             Self::Name(name) => format!("{{ name = \"{name}\" }}"),  // <-- here
>             Self::Email(email) => format!("{{ email = \"{email}\" }}"),
>         }
>     }
> }
> Using [`toml::to_string()`](https://docs.rs/toml/latest/toml/ser/fn.to_string.html) might be a bit much for this case. Maybe `name.replace("\"", "\\\"")` is enough?

Using [`to_string`](https://docs.rs/toml/latest/toml/ser/fn.to_string.html) has the other added benefit of handling other control characters, backslashes, etc (in addition to double quotes). I think this is the 'proper' fix here.

Something like:

```rust
use toml_edit::InlineTable;

#[derive(Debug)]
enum Author {
  Name(String),
  Email(String),
  NameEmail { name: String, email: String },
}


impl Author {
  fn to_toml_string(&self) -> String {
    let mut inline = InlineTable::new();

    match self {
      Self::NameEmail { name, email } => {
        inline.get_or_insert("name", name);
        inline.get_or_insert("email", email);
      }
      Self::Name(name) => {
        inline.get_or_insert("name", name);
      }
      Self::Email(email) => {
        inline.get_or_insert("email", email);
      }
    }

    inline.to_string()
  }
}

```

---

_Comment by @konstin on 2025-11-19 09:45_

It's definitely a better idea to use `toml_edit` here than trying to use a template that doesn't do toml escaping.

---

_Referenced in [astral-sh/uv#16778](../../astral-sh/uv/pulls/16778.md) on 2025-11-19 17:29_

---

_Closed by @woodruffw on 2025-11-20 19:10_

---
