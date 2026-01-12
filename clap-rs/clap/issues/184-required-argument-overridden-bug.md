```yaml
number: 184
title: Required argument overridden bug
type: issue
state: closed
author: Vinatorul
labels:
  - C-bug
assignees: []
created_at: 2015-08-22T14:22:38Z
updated_at: 2018-08-02T03:29:42Z
url: https://github.com/clap-rs/clap/issues/184
synced_at: 2026-01-12T16:14:08Z
```

# Required argument overridden bug

---

_@Vinatorul_

```
let m = App::new("req_overridden")
            .arg(Arg::from_usage("-f, --flag 'some flag'")
                .requires("color"))
            .arg(Arg::from_usage("-c, --color 'some other flag'")
                .mutually_overrides_with("debug"))
            .arg(Arg::from_usage("-d, --debug 'another other flag'"))
            .get_matches_from(vec!["", "--flag", "-c", "-d"]);
```

This sample should throw

```
error: The following required arguments were not supplied:
    '--color'

USAGE:
    req_overridden --color --flag --debug

For more information try --help

```

unless it did not


---

_Label `T: bug` added by @Vinatorul on 2015-08-22 14:23_

---

_Label `P2: need to have` added by @Vinatorul on 2015-08-22 14:23_

---

_Label `C: args` added by @Vinatorul on 2015-08-22 14:23_

---

_Label `D: intermediate` added by @Vinatorul on 2015-08-22 14:23_

---

_Referenced in [clap-rs/clap#182](../../clap-rs/clap/pulls/182.md) on 2015-08-22 14:31_

---

_Comment by @kbknapp on 2015-08-22 15:01_

I think we need something like [this line](https://github.com/kbknapp/clap-rs/blob/a7e6e00a11c20cadbde0c457aafa777864374eac/src/app.rs#L2343) only for requirements. Those two lines do some validation before the next subcommand (if any) gets parsed...so this would be the probable point to validate all requirements as well.


---

_Referenced in [clap-rs/clap#185](../../clap-rs/clap/issues/185.md) on 2015-08-22 15:13_

---

_Comment by @Vinatorul on 2015-08-22 15:18_

Good idea, it will work :+1: 


---

_Comment by @kbknapp on 2015-08-22 20:17_

It removes the requirements of args that were overrriden, not ones that still exist.

I.e. if A requires B and B mutually overrides C. Then the user passes in `A B C` which after overrides take effect is `A C`. This should fail because A's requirements aren't met.

Conversely, if A requires B and A mutually overrrides with C. Then the user passes in, `A C`, this should pass. Because A's requirements of B were removed when it got overrriden by C. Effectively its as if the user just passed in `C`.


---

_Referenced in [clap-rs/clap#187](../../clap-rs/clap/pulls/187.md) on 2015-08-22 21:26_

---

_Closed by @Vinatorul on 2015-08-23 07:34_

---
