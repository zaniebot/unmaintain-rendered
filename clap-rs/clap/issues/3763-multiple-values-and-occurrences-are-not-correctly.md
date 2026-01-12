```yaml
number: 3763
title: multiple values and occurrences are not correctly counted / grouped for positionals
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - E-hard
  - A-parsing
assignees: []
created_at: 2022-05-27T11:39:43Z
updated_at: 2022-06-08T14:35:02Z
url: https://github.com/clap-rs/clap/issues/3763
synced_at: 2026-01-12T16:14:15Z
```

# multiple values and occurrences are not correctly counted / grouped for positionals

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.1.13

### Minimal reproducible code

```rust

#[test]
fn values_per_occurrence_positional() {
    let mut a = Command::new("test").arg(
        Arg::new("pos")
            .number_of_values(2)
            .multiple_occurrences(true),
    );

    let m = a.try_get_matches_from_mut(vec!["myprog", "val1", "val2"]);
    let m = match m {
        Ok(m) => m,
        Err(err) => panic!("{}", err),
    };
    assert_eq!(
        m.get_many::<String>("pos")
            .unwrap()
            .map(|v| v.as_str())
            .collect::<Vec<_>>(),
        ["val1", "val2"]
    );
    assert_eq!(m.occurrences_of("pos"), 1);

    let m = a.try_get_matches_from_mut(vec!["myprog", "val1", "val2", "val3", "val4"]);
    let m = match m {
        Ok(m) => m,
        Err(err) => panic!("{}", err),
    };
    assert_eq!(
        m.get_many::<String>("pos")
            .unwrap()
            .map(|v| v.as_str())
            .collect::<Vec<_>>(),
        ["val1", "val2", "val3", "val4"]
    );
    assert_eq!(m.occurrences_of("pos"), 2); // Fails, we don't recognize this as two occurrences
}

#[test]
fn positional_parser_advances() {
    let m = Command::new("multiple_values")
        .arg(Arg::new("pos1").number_of_values(2))
        .arg(Arg::new("pos2").number_of_values(2))
        .try_get_matches_from(vec!["myprog", "val1", "val2", "val3", "val4"]);

    assert!(m.is_ok(), "{}", m.unwrap_err());
    let m = m.unwrap();

    assert_eq!(m.occurrences_of("pos1"), 1);
    assert_eq!(
        m.get_many::<String>("pos1")
            .unwrap()
            .map(|v| v.as_str())
            .collect::<Vec<_>>(),
        ["val1", "val2"]
    );

    assert_eq!(m.occurrences_of("pos2"), 1);
    assert_eq!(
        m.get_many::<String>("pos2")
            .unwrap()
            .map(|v| v.as_str())
            .collect::<Vec<_>>(),
        ["val3", "val4"]
    );
}

```


### Steps to reproduce the bug with the above code

Inject those tests into clap

### Actual Behaviour

The first test will report groups correctly but will also report an occurrence per value, contradicting the documentation.  In a change I'm working on, it will instead report one occurrence regardless of how many occurrences there should be

The second test is invalid because we don't stop parsing multiple values once we've hit the cap, so we disallow it in a debug assert

### Expected Behaviour

Both pass

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-05-27 11:39_

---

_Label `M-breaking-change` added by @epage on 2022-05-27 11:39_

---

_Label `E-hard` added by @epage on 2022-05-27 11:39_

---

_Label `A-parsing` added by @epage on 2022-05-27 11:39_

---

_Added to milestone `4.0` by @epage on 2022-05-27 11:39_

---

_Referenced in [clap-rs/clap#3765](../../clap-rs/clap/pulls/3765.md) on 2022-05-27 18:47_

---

_Renamed from "multiple values and occurrences are not correctly counted / grou[ed" to "multiple values and occurrences are not correctly counted / grouped" by @epage on 2022-05-27 18:48_

---

_Renamed from "multiple values and occurrences are not correctly counted / grouped" to "multiple values and occurrences are not correctly counted / grouped for positionals" by @epage on 2022-05-31 14:40_

---

_Referenced in [clap-rs/clap#3772](../../clap-rs/clap/issues/3772.md) on 2022-05-31 14:49_

---

_Comment by @epage on 2022-06-08 14:35_

Closing because of #3772

---

_Closed by @epage on 2022-06-08 14:35_

---
