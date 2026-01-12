```yaml
number: 2759
title: Allow iterating over ArgMatches and check whether a match uses default value
type: issue
state: closed
author: romange
labels: []
assignees: []
created_at: 2021-09-09T09:38:35Z
updated_at: 2021-09-23T12:55:05Z
url: https://github.com/clap-rs/clap/issues/2759
synced_at: 2026-01-12T16:14:13Z
```

# Allow iterating over ArgMatches and check whether a match uses default value

---

_@romange_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta

### Describe your use case

Use case: I would like to merge ArgMatches into Config settings using the following rules:
1. ArgMatches have higher precedence over Config if they are explicitly set.
2. If config setting is present but ArgMatches has an argument with default value then config value is taken.
3. If config does not set the setting, then ArgMatches default value it taken.

In order to do this, I would need to iterate over ArgMatches.args and query  `MatchedArg.ty` field.  

### Describe the solution you'd like

Some sort of interface that allows me to do this.

### Alternatives, if applicable

Currently, I have to declare all fields in clap structure with `Option<>` and without default values in order to expose similar semantics. For example,

```
struct MyOpts {
    #[clap(short, long, default_value="80")]
    port: Option<u16>,
}
```

instead of 
```
struct MyOpts {
    #[clap(short, long, default_value="8080")]
    port: u16,
}
```

### Additional Context

_No response_

---

_Label `T: new feature` added by @romange on 2021-09-09 09:38_

---

_Comment by @epage on 2021-09-09 13:54_

While, in general, I think there is value in
- Allowing iterating
- exposing `ArgType` (though maybe giving it a more meaningful name, like `ArgSource`

For layered config, I'm a bit unsure on this approach.  I've split layered config talk out to [a discussion](https://github.com/clap-rs/clap/discussions/2763)

---

_Comment by @pksunkara on 2021-09-23 12:54_

This issue is a duplicate of #1206 and thus we are closing this.

I would appreciate if you could tell us more about why you were not able to find the original issue. You marked that you have searched the existing issues. What search terms did you use?

<sub>This questions is asked in the attempt to understand if there are any other contexts/cases related to the issue that we missed discussing originally.</sub>

---

_Closed by @pksunkara on 2021-09-23 12:54_

---
