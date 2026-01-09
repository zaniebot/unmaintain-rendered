---
number: 2792
title: Support unicode for ignore_case on possible_values
type: issue
state: closed
author: ModProg
labels:
  - C-bug
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-09-26T17:38:20Z
updated_at: 2021-10-09T13:45:12Z
url: https://github.com/clap-rs/clap/issues/2792
synced_at: 2026-01-07T13:12:19-06:00
---

# Support unicode for ignore_case on possible_values

---

_Issue opened by @ModProg on 2021-09-26 17:38_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

Possible value containing non ascii character should match with ignore_case

Currently this will fail:
```rs
#[test]
fn case_insensitive() {
    let m = App::new("pv")
        .arg(
            Arg::new("option")
                .short('o')
                .long("--option")
                .takes_value(true)
                .possible_value("ä")
                .case_insensitive(true),
        )
        .try_get_matches_from(vec!["pv", "--option", "Ä"]);

    assert!(m.is_ok());
    assert!(m
        .unwrap()
        .value_of("option")
        .is_some());
}

```

### Describe the solution you'd like

ignore_case (or a new flag called maybe ignore_unicode_case) should match on non ascii characters with the wrong case.

### Possible implementation 
Replace `eq_ignore_ascii_case` in validator with `a.to_lowercase() == b.to_lowercase()`.
There is no perfect solution for case_ignore in unicode, but this is the closest we can get IMO.


---

_Label `T: new feature` added by @ModProg on 2021-09-26 17:38_

---

_Label `C: args` added by @pksunkara on 2021-09-26 20:58_

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-09-26 20:58_

---

_Label `:money_with_wings: $10` removed by @pksunkara on 2021-09-26 20:59_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-09-26 20:59_

---

_Label `D: easy` added by @pksunkara on 2021-09-26 20:59_

---

_Label `T: bug` added by @pksunkara on 2021-09-26 20:59_

---

_Label `T: new feature` removed by @pksunkara on 2021-09-26 20:59_

---

_Comment by @epage on 2021-09-27 13:14_

I generally rely on [unicase](https://docs.rs/unicase/2.6.0/unicase/fn.eq.html) for this which seems to do case folding.  Since most programs probably do not use unicode for possible values, I assume we'd want to have this behind a feature flag, both for build-time and for runtime performance.

---

_Comment by @epage on 2021-10-04 17:18_

@pksunkara v3 adds a build feature `unicode_help`.  What if we renamed `unicode_help` to `unicode` and have this put behind it as well (since it also pulls in a new dep)?

If we go this route, I'd recommend we bring this into v3.

---

_Comment by @pksunkara on 2021-10-09 00:00_

I would recommend adding a new feature `unicode_values`. And another new feature `unicode` can enable both `unicode_values` and `unicode_help`.

And we can always do this after v3 since there's nothing breaking.

---

_Comment by @epage on 2021-10-09 10:31_

> And we can always do this after v3 since there's nothing breaking.

Only non-breaking because of adding new features :)

> I would recommend adding a new feature unicode_values. And another new feature unicode can enable both unicode_values and unicode_help.

I disagree with this:
- The likelihood of using only one of them seems low.  `unicode_values` implies `unicode_help` as-is because unicode content will be in the help.  All we are saving people is those who only want `unicode_help`.  If they are already having non-ASCII help, there is probably a good likelihood that they'll use non-ASCII possible-values. 
- The user-benefit of getting one without the other is relatively low.  It adds dependencies but not much.  It impacts performance, but not by much.
- Proliferation of features adds cognitive load and negatively impacts all features.  We need to consider the cost for each feature we add, not just look at the benefits.

---

_Comment by @pksunkara on 2021-10-09 10:34_

In that case we need to rename unicode_help feature to unicode right now.

---

_Added to milestone `3.0` by @epage on 2021-10-09 11:19_

---

_Referenced in [clap-rs/clap#2834](../../clap-rs/clap/pulls/2834.md) on 2021-10-09 11:51_

---

_Closed by @bors[bot] on 2021-10-09 13:45_

---
