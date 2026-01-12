```yaml
number: 3638
title: Explicitly doc the usage of ArgEnum
type: issue
state: closed
author: Banyc
labels:
  - C-enhancement
assignees: []
created_at: 2022-04-18T07:03:14Z
updated_at: 2022-04-20T14:04:30Z
url: https://github.com/clap-rs/clap/issues/3638
synced_at: 2026-01-12T16:14:15Z
```

# Explicitly doc the usage of ArgEnum

---

_@Banyc_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.9

### Describe your use case

Searching google for "rust clap enum example" yields no results or [outdated macro `arg_enum`](https://docs.rs/clap/2.27.1/clap/macro.arg_enum.html).

The expected answer should be:

```rust
#[derive(clap::Parser)]
struct Args {
    #[clap(arg_enum)]
    level: Level,
}

#[derive(clap::ArgEnum, Clone)]
enum Level {
    Debug,
    Info,
    Warning,
    Error,
}
```


### Describe the solution you'd like

Explicitly show the example and usage in `README.md`.


### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Banyc on 2022-04-18 07:03_

---

_Comment by @epage on 2022-04-18 12:05_

You have several routes to the documentation
- On the [arg_enum page](https://docs.rs/clap/2.27.1/clap/macro.arg_enum.html), click "Go to latest release" and show a search result for the `ArgEnum` trait which gives a derive example
- On [the docs](https://docs.rs/clap/) click on "Tutorial: .... Derive API"  and scroll down to the validation section for [enumerated values](https://github.com/clap-rs/clap/blob/v3.1.9/examples/tutorial_derive/README.md#enumerated-values)
- On [the docs](https://docs.rs/clap/) click on ["Derive Reference"](https://github.com/clap-rs/clap/blob/v3.1.9/examples/derive_ref/README.md) and you will see how to use it and what attributes are supported.

---

_Comment by @Banyc on 2022-04-19 06:58_

@epage I totally ok with the suggestions as a user who have just found the correct usage. But as for new learners stumbling across this, they might still find it a hard time looking for answers. In their (including mine) points of views, the doc is not clear and easy enough, especially comparing to [that of C#](https://github.com/commandlineparser/commandline/wiki/Enum-Flags). I am really impressed by Rust language and its safety features. That's why I want the ecosystem around the language could be more friendly to new learners. I hope the usage is only one search away, not nested in links within links imho.


---

_Comment by @epage on 2022-04-19 11:42_

Isn't that similar to [our tutorial](https://github.com/clap-rs/clap/blob/v3.1.9/examples/tutorial_derive/README.md#enumerated-values), with the main difference being that our code isn't inline?

The lack of inline code is not great but its a side effect of making our code samples testable.  If we switched to a mdbook, we could have them inline.  https://github.com/clap-rs/clap/issues/3189 is exploring the needs for our derive documentation.

What I'm trying to understand is a concrete user need that we can explore improving or a concrete solution we can evaluate applying.

---

_Comment by @Banyc on 2022-04-19 14:18_

@epage

> a concrete solution

 I fully agree with this!


---

_Comment by @epage on 2022-04-20 14:04_

Without anything actionable and the fact that the reported problem (documenting `ArgEnum`) is done, I'm going to go ahead and close this.  If you have specific concerns or specific solutions, feel free to open new issues.

---

_Closed by @epage on 2022-04-20 14:04_

---
