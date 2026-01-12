```yaml
number: 239
title: Upgrade YAML test
type: issue
state: closed
author: Vinatorul
labels: []
assignees: []
created_at: 2015-09-08T13:11:45Z
updated_at: 2018-08-02T03:29:44Z
url: https://github.com/clap-rs/clap/issues/239
synced_at: 2026-01-12T16:14:08Z
```

# Upgrade YAML test

---

_@Vinatorul_

Now yaml test only check `yml` parsing, but not the the work in general.
I think we need to add:
- [ ] flags tests
- [ ] options test
- [ ] positionals test
- [ ] requirement test
- [ ] conflicts test
- [ ] override tess
- [ ] multiple args
- [ ] takes value args
- [ ] min and max values
- [ ] arg group test
- [ ] subcommand test


---

_Label `P2: need to have` added by @Vinatorul on 2015-09-08 13:11_

---

_Label `C: tests` added by @Vinatorul on 2015-09-08 13:11_

---

_Label `D: easy` added by @Vinatorul on 2015-09-08 13:11_

---

_Comment by @kbknapp on 2015-09-08 22:45_

Unless I'm not understanding the question/issue, so long as the Yaml parser spits out a valid `App` it's good. Checking that `App` is doing what it's supposed to isn't the job of the Yaml tests. Similar to the soon-to-be-new macros. We just need to verify that the Yaml parser is spitting out the intended `App` struct with all the specific settings we wrote in the `.yml`

...or maybe I'm misunderstanding :stuck_out_tongue_winking_eye: 


---

_Comment by @Vinatorul on 2015-09-09 06:26_

As I tried to explain the idea of test it macros PR - we can't claim that it work as it assumed. 
We can say - yes, it is doing something, but can't say what it is doing.
I can agree that integration test is overkill, but we have no unit test for `yaml` functions. So we cant proove them working well.


---

_Comment by @sru on 2015-09-09 15:32_

We are only testing the validity, not the correctness. Currently, we have no way to tell from tests that parsing YAML will work as intended and not explode on your face.

I agree with @kbknapp that the YAML tests shouldn't check what `App` is doing, but I believe that we should check if YAML parsing results in intended `App`.

For example:

``` rust
// Note: this is an over-simplified unit test example.
use super::{YamlLoader, App};

#[test]
fn parse_yaml_author() {
    let yml = "
name: claptests
author: Kevin K. <kbknapp@gmail.com>";

    let app = App::from_yaml(YamlLoader::load_from_str(yml).unwrap());

    assert_eq!(Some("Kevin K. <kbknapp@gmail.com>"), app.author);
}
```


---

_Comment by @WildCryptoFox on 2015-09-09 15:37_

Both macro and yaml parsers need to have their testing done by checking that they have performed the expected builder functions against the App. @sru 's `assert_eq` method should work well.

For the macro case, I manually verified the generated code - it could still do with a test case; as @Vinatorul pointed out for at least regression testing.


---

_Comment by @Vinatorul on 2015-09-09 16:57_

@sru I told about something like this, but I also think that integration test is overkill.
We need to test functions like this:
https://github.com/kbknapp/clap-rs/blob/master/src/args/group.rs#L89
or this
https://github.com/kbknapp/clap-rs/blob/master/src/args/arg.rs#L164

Both false-negative and true-positive cases.


---

_Comment by @Vinatorul on 2015-09-22 18:35_

As we can see from #266 #267 #270 #271 these tests are "must have"


---

_Label `P1: urgent` added by @Vinatorul on 2015-09-22 18:35_

---

_Label `P2: need to have` removed by @Vinatorul on 2015-09-22 18:35_

---

_Comment by @kbknapp on 2015-09-22 19:43_

#270 is by design, so that's not really a bug. But yes, we should update the tests to ensure everything is good to go. ;)


---

_Label `P3: want to have` added by @kbknapp on 2016-08-20 22:12_

---

_Label `P1: urgent` removed by @kbknapp on 2016-08-20 22:12_

---

_Closed by @kbknapp on 2017-01-30 05:13_

---
