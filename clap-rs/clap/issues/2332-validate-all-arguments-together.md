```yaml
number: 2332
title: Validate all arguments together
type: issue
state: closed
author: giggio
labels:
  - C-enhancement
  - A-parsing
  - A-validators
  - S-duplicate
assignees: []
created_at: 2021-02-06T22:12:19Z
updated_at: 2022-01-11T18:21:50Z
url: https://github.com/clap-rs/clap/issues/2332
synced_at: 2026-01-12T16:14:13Z
```

# Validate all arguments together

---

_@giggio_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I'd like to have a validation function that would validate the arguments together.

### Describe the solution you'd like

I'm thinking something like this:

````rust
App::new("x")
    .validate(|args| /* some logic and... */ Err(("myarg", "Why invalid"));
````

(I'm not married to this example, it is just so we can have an idea)

In this case, argument `myarg` is invalid, after I've gone through some logic that would go through all arguments already supplied (and validated individually).

### Alternatives, if applicable

Validate after parsing using `ArgMatches`.

### Additional context

Already discussed at #1913 and #1206, and I think a solution with a closure after on the app context could make sense.


---

_Label `T: new feature` added by @giggio on 2021-02-06 22:12_

---

_Label `C: validators` added by @pksunkara on 2021-02-06 22:17_

---

_Label `C: parsing` added by @pksunkara on 2021-02-06 22:17_

---

_Referenced in [epage/clapng#178](../../epage/clapng/issues/178.md) on 2021-12-06 21:12_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:11_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:11_

---

_Comment by @epage on 2021-12-13 16:22_

Note that in clap3 we added `App::error` for being able to create custom validation with `ArgMatches` that looks as if it was from a validator.

#3008 is our other generalization of validation.

Closing this in favor of `App::error` and #3008.  If there is any concern with this, feel free to let us know!

---

_Closed by @epage on 2021-12-13 16:22_

---

_Comment by @ctrlcctrlv on 2022-01-09 04:02_

MFEK's fork can do this and was rejected in #3029. If you need a solution right now like we did you could use it. We are waiting for a resolution to #3008 to be able to drop our fork and track upstream again.

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:21_

---
