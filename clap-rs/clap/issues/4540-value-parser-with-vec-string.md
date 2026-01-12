```yaml
number: 4540
title: "value_parser with Vec<String>"
type: issue
state: closed
author: gleich
labels:
  - C-bug
assignees: []
created_at: 2022-12-04T03:15:40Z
updated_at: 2022-12-04T18:14:29Z
url: https://github.com/clap-rs/clap/issues/4540
synced_at: 2026-01-12T16:14:16Z
```

# value_parser with Vec<String>

---

_@gleich_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.67.0-nightly (32e613bba 2022-12-02)

### Clap Version

4.0.2

### Minimal reproducible code

```rust
fn main() {
	let values = vec![String::from("test")];
	Command::new("foo")
		.arg(Arg::new("bar").value_parser(values));
}
```


### Steps to reproduce the bug with the above code

`cargo run`, code fails to compile due to error: `the trait bound `ValueParser: From<Vec<std::string::String>>` is not satisfied`

### Actual Behaviour

when I pass value_parser a Vec<String> it fails to compile which I feel like shouldn't happen

### Expected Behaviour

I should be able to pass a Vec<String> just like I am able to pass Vec<&str>.

### Additional Context

I would just run an iterator with a map over the strings to convert them to &str but I run into lifetime/ownership issues.

### Debug Output

N/A, doesn't compile

---

_Label `C-bug` added by @gleich on 2022-12-04 03:15_

---

_Comment by @epage on 2022-12-04 03:44_

Run `cargo add clap@4.0.27 -F string`
- 4.0.27 added a `From` for `Vec<impl Into<PossibleValue>>` (you mentioned being on 4.0.2)
- The `string` feature is needed to allow clap to work with dynamically generated strings.

---

_Comment by @gleich on 2022-12-04 18:14_

ah ok, I was missing the string feature. Thank you so much for the quick response!

---

_Closed by @gleich on 2022-12-04 18:14_

---
