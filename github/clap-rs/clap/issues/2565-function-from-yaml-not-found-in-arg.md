---
number: 2565
title: "function `from_yaml` not found in `Arg<'_>`"
type: issue
state: closed
author: dwisiswant0
labels:
  - C-bug
assignees: []
created_at: 2021-06-29T15:01:02Z
updated_at: 2021-06-29T15:07:14Z
url: https://github.com/clap-rs/clap/issues/2565
synced_at: 2026-01-07T13:12:19-06:00
---

# function `from_yaml` not found in `Arg<'_>`

---

_Issue opened by @dwisiswant0 on 2021-06-29 15:01_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use {
	clap::{
		App, Arg,
		load_yaml,
		crate_authors,
		crate_description,
		crate_name,
		crate_version
	},
};

#[async_std::main]
async fn main() {
	let yaml = load_yaml!("cli.yaml");
	let args = Arg::from_yaml(yaml);
	let matches = App::new(crate_name!())
		.author(crate_authors!())
		.about(crate_description!())
		.name(crate_name!())
		.version(crate_version!())
		.arg(args)
		.get_matches();
...
```


### Steps to reproduce the bug with the above code

cargo run --

### Actual Behaviour

Can't call `from_yaml` method.

### Expected Behaviour

Should be able to use (according to documentation).

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @dwisiswant0 on 2021-06-29 15:01_

---

_Closed by @pksunkara on 2021-06-29 15:07_

---

_Locked by @clap-rs on 2021-06-29 15:07_

---
