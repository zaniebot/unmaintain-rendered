```yaml
number: 2748
title: "Pass `None` to `default_value_if`, or better adding `default_value_if_not_set`?"
type: issue
state: closed
author: runapp
labels: []
assignees: []
created_at: 2021-08-31T21:17:46Z
updated_at: 2021-09-04T10:13:03Z
url: https://github.com/clap-rs/clap/issues/2748
synced_at: 2026-01-12T16:14:13Z
```

# Pass `None` to `default_value_if`, or better adding `default_value_if_not_set`?

---

_@runapp_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

I found that I can't make an option that only has default value if another option (doesn't takes value) is not set. The closest way is like this:
```rust
    let matches = App::new("test")
        .arg(Arg::with_name("A").short("A"))
        .arg(Arg::with_name("B").short("B").default_value_if("A", None, "").default_value("A not set"))
        .get_matches();
    println!("A={} B={}",
        matches.value_of("A").unwrap_or("###"),
        matches.value_of("B").unwrap_or("###"),
    );
```
I was thinking of passing `None` to `default_value_if` might help emulate as if `-B` is never set (`is_present("B")==false`). But `default_value_if` only accepts `&str` rather than `Option`.

### Describe the solution you'd like

I don't know which would be better. Modifying current signature of `default_value_if` seems painful, and might bring endless `Some` to most common cases. So would add `default_value_if_not_set` be a choice?

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @runapp on 2021-08-31 21:17_

---

_Comment by @pksunkara on 2021-09-04 10:11_

This is fixed on v3/master through a PR. What search terms did you use when searching for issues like this as requested?

---

_Closed by @pksunkara on 2021-09-04 10:11_

---
