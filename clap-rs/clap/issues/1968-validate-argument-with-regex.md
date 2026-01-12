```yaml
number: 1968
title: Validate argument with regex
type: issue
state: closed
author: catleeball
labels:
  - A-validators
  - E-easy
assignees: []
created_at: 2020-06-09T01:33:27Z
updated_at: 2022-05-23T20:23:00Z
url: https://github.com/clap-rs/clap/issues/1968
synced_at: 2026-01-12T16:14:12Z
```

# Validate argument with regex

---

_@catleeball_

### Describe your use case

It would be nice to have a clap feature for argument validation via a regex. This would be handy and flexible for users, allowing them custom validations without writing a [custom validator](https://github.com/clap-rs/clap/blob/master/examples/15_custom_validator.rs) to do so, and hopefully without requiring too large of overhead for implementation and testing in clap itself.

### Describe the solution you'd like

For example, using clap yaml format, if we want an argument whose value wants only digit values 1 through 100:

```
args:
  - jpg-quality:
    about: Quality of jpg image to write. Must be a number 1-100.
    takes_value: true
    regex: "^[1-9][0-9]?$|^100$"
```

This could expand to complex regex use cases, like matching if an arg value is a URL:

```
args:
  - url:
    about: URL to load.
    index: 1
    required: true
    # Regex stolen from: https://stackoverflow.com/a/3809435
    regex: "[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)"
```

Thanks for this very nice arg parsing library!

---

_Label `T: new feature` added by @catleeball on 2020-06-09 01:33_

---

_Renamed from "[FR] - Validate argument with regex" to "Validate argument with regex" by @catleeball on 2020-06-09 02:24_

---

_Comment by @CreepySkeleton on 2020-06-09 05:22_

My first thought was: "This is pretty trivial with normal validators!". And indeed:
```rust
lazy_static! {
    static ref RE: Regex = Regex::new("\\d+").unwrap();
}

app.validator(|val| {
    if RE.is_match(val)) {
        Ok(())
    } else {
        Err(format!("value '{}' doesn't match the `\\d+` regex", val))
    }
})
```

But on the other side, validators cannot be deserialized from YAML, and "matches a regex" does sound common enough to warrant a special case. 

I say we might gate it behind `regex` feature.

Unresolved questions: what about error messages? I have doubts that "value <val> doesn't match <re> regex" is good user-facing message. Perhaps the API should look like:
```rust
fn regex_validator("-?\\d+", "expected an integer");

// rendered as
"Invalid value `foo`: expected an integer"
```
@pksunkara @kbknapp Any thoughts?

---

_Label `C: validators` added by @CreepySkeleton on 2020-06-09 05:24_

---

_Label `D: easy` added by @CreepySkeleton on 2020-06-09 05:24_

---

_Label `E: optional dep` added by @CreepySkeleton on 2020-06-09 05:24_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-06-09 05:24_

---

_Label `Z: good first issue` added by @CreepySkeleton on 2020-06-09 05:24_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-06-09 05:24_

---

_Comment by @catleeball on 2020-06-09 06:47_

Agreed on your point about needing a more helpful error, @CreepySkeleton . I personally like the example function you provided where the user supplies an error string.

---

_Comment by @loewenheim on 2020-06-18 15:02_

How would the error message be written in a YAML file?

---

_Comment by @CreepySkeleton on 2020-06-18 15:36_

Just as any other tuple would
```yaml
regex_validator: 
  - r"[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)"
  - error message

# or if regex and error are short enough
regex_validator: ["-?\\d+", "expected an integer"]

```



---

_Comment by @tmankita on 2020-07-23 11:14_

Hi,
I would like to pick up this issue if no one is working on it yet.


---

_Referenced in [clap-rs/clap#2073](../../clap-rs/clap/pulls/2073.md) on 2020-08-14 21:24_

---

_Closed by @bors[bot] on 2020-08-28 17:39_

---

_Referenced in [clap-rs/clap#3743](../../clap-rs/clap/issues/3743.md) on 2022-05-23 20:21_

---

_Comment by @epage on 2022-05-23 20:23_

FYI I am considering removing `validator_regex`, see #3743

---
