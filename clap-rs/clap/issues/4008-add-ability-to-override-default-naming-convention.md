---
number: 4008
title: Add ability to override default naming convention for ValueEnum variants
type: issue
state: closed
author: mfreeborn
labels:
  - C-enhancement
assignees: []
created_at: 2022-07-31T10:04:54Z
updated_at: 2022-08-01T20:56:37Z
url: https://github.com/clap-rs/clap/issues/4008
synced_at: 2026-01-10T01:27:50Z
---

# Add ability to override default naming convention for ValueEnum variants

---

_Issue opened by @mfreeborn on 2022-07-31 10:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.16

### Describe your use case

I'm deriving `ValueEnum` to parse values for one of the command line options into a set of discrete values. The default derive macro converts enum variants into kebab-case possible values, but I would like to override that and have them in snake_case. Here's some code:

```rust
#[derive(Clone, clap::ValueEnum)]
enum EventName {
    Sunrise,
    CivilDawn,
    CustomAM,
}

// ValueEnum provides the following string values which are allowed when passed over the command line: [sunrise, civil-dawn, custom-am]

// But I would like the values to be snake_case, so I have to implement `ValueEnum` directly:

#[derive(Clone)]
enum EventName {
    Sunrise,
    CivilDawn,
    CustomAM,
}

impl clap::ValueEnum for EventName {
    fn value_variants<'a>() -> &'a [Self] {
        &[
            Self::Sunrise,
            Self::CivilDawn,
            Self::CustomAM,
        ]
    }

    fn to_possible_value<'a>(&self) -> ::std::option::Option<clap::PossibleValue<'a>> {
        match self {
            Self::Sunrise => Some(clap::PossibleValue::new("sunrise")),
            Self::CivilDawn => Some(clap::PossibleValue::new("civil_dawn")),
            Self::CustomAM => Some(clap::PossibleValue::new("custom_am")),
        }
    }
}
```

### Describe the solution you'd like

Taking inspiration from `serde`, it should be possible to rename the variants using a macro. `serde` either lets you [annotate the whole `enum`](https://serde.rs/container-attrs.html#rename_all) and set the case e.g.:

```rust
#[derive(Clone, clap::ValueEnum)]
#[clap(rename_all = "kebab_case")]
enum EventName {
    Sunrise,
    CivilDawn,
    CustomAM,
}
```

or allows [annotating individual variants](https://serde.rs/variant-attrs.html#rename), e.g.:

```rust
#[derive(Clone, clap::ValueEnum)]
enum EventName {
    Sunrise,
    #[clap(rename = "civil_dawn")]
    CivilDawn,
    #[clap(rename = "custom_am")]
    CustomAM,
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @mfreeborn on 2022-07-31 10:04_

---

_Comment by @epage on 2022-08-01 17:06_

What about the existing [rename_all attribute](https://docs.rs/clap/latest/clap/_derive/index.html#value-enum-attributes).  I don't know if we have any examples but we do have [some tests](https://github.com/clap-rs/clap/blob/master/tests/derive/value_enum.rs#L214).

---

_Comment by @mfreeborn on 2022-08-01 17:58_

Yes that's exactly what I was looking for. I see that one can also use #[clap(name = "anything you like")] to rename individual variants. The problem was that I was looking in the wrong place, here: https://docs.rs/clap/latest/clap/trait.ValueEnum.html

---

_Comment by @epage on 2022-08-01 18:04_

There is enough overlapping derive documentation that we haven't put it all on each trait's documentation.  We do call out there being documentation for attributes though items like that can easily be missed.

---

_Comment by @mfreeborn on 2022-08-01 18:11_

Ah hah, and the contents item 2.4 on the derive reference page uses the deprecated Arg Enum naming, so the link doesn't work when you click on it. It should all be much simpler when the docs get updated to fix #4009
________________________________
From: Ed Page ***@***.***>
Sent: Monday, August 1, 2022 7:04:22 PM
To: clap-rs/clap ***@***.***>
Cc: Michael Freeborn ***@***.***>; Author ***@***.***>
Subject: Re: [clap-rs/clap] Add ability to override default naming convention for ValueEnum variants (Issue #4008)


There is enough overlapping derive documentation that we haven't put it all on each trait's documentation. We do call out there being documentation for attributes though items like that can easily be missed.

—
Reply to this email directly, view it on GitHub<https://github.com/clap-rs/clap/issues/4008#issuecomment-1201534698>, or unsubscribe<https://github.com/notifications/unsubscribe-auth/AHSVKWH4LDIK5O2U4XDFBOTVXAGSNANCNFSM55EVISLQ>.
You are receiving this because you authored the thread.Message ID: ***@***.***>


---

_Referenced in [clap-rs/clap#4015](../../clap-rs/clap/pulls/4015.md) on 2022-08-01 20:15_

---

_Closed by @epage on 2022-08-01 20:34_

---

_Referenced in [clap-rs/clap#4016](../../clap-rs/clap/pulls/4016.md) on 2022-08-01 20:43_

---

_Comment by @epage on 2022-08-01 20:56_

Good catch.  Fixed

---
