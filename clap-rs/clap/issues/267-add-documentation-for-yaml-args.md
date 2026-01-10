---
number: 267
title: Add documentation for YAML args.
type: issue
state: closed
author: XAMPPRocky
labels:
  - A-docs
assignees: []
created_at: 2015-09-22T15:38:09Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/267
synced_at: 2026-01-10T01:26:26Z
---

# Add documentation for YAML args.

---

_Issue opened by @XAMPPRocky on 2015-09-22 15:38_

I can't really find anything about how to do certain commands using YAML.

Example:
`subcommands_negate_reqs` I tried adding it to the YAML file, and got the following error.

```
thread '<main>' panicked at 'Unknown Arg setting 'subcommands_negate_reqs' in YAML file for arg 'languages'', /Users/aaron/.multirust/toolchains/stable/cargo/registry/src/github.com-0a35038f75765ae4/clap-1.4.0/src/args/arg.rs:227
```


---

_Label `P2: need to have` added by @Vinatorul on 2015-09-22 15:42_

---

_Label `C: docs` added by @Vinatorul on 2015-09-22 15:42_

---

_Label `D: easy` added by @Vinatorul on 2015-09-22 15:42_

---

_Referenced in [XAMPPRocky/tokei#9](../../XAMPPRocky/tokei/issues/9.md) on 2015-09-22 15:54_

---

_Comment by @sru on 2015-09-22 16:31_

@Aaronepower Did you try `subcommandsNegateReqs`? For now, you can use `examples/17_yaml.rs` and `examples/17_yaml.yml` as reference.


---

_Comment by @XAMPPRocky on 2015-09-22 16:32_

No, but why would it be camelCase? Every other command like takes_value is snake_case?

On Tue, Sep 22, 2015 at 5:31 PM, SungRim Huh notifications@github.com
wrote:

> ## @Aaronepower Did you try `subcommandsNegateReqs`? For now, you can use `examples/17_yaml.rs` and `examples/17_yaml.yml` as reference.
> 
> Reply to this email directly or view it on GitHub:
> https://github.com/kbknapp/clap-rs/issues/267#issuecomment-142341618


---

_Comment by @sru on 2015-09-22 16:46_

@Aaronepower Or, `SubcommandsNegateReqs`. It is case insensitive. I am not sure about the exact decision.

`SubcommandsNegateReqs` is an `AppSetting`, **not** `Arg` setting, so you cannot set this for individual arguments. I've briefly looked at your yaml file. I think this is not what you want because flags are not subcommands. You probably want [`conflicts_with`](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.conflicts_with), or in yaml:

``` yaml
conflicts_with:
  - input
```


---

_Comment by @kbknapp on 2015-09-22 17:04_

@Aaronepower It's camel case as part of the `settings` only because it's a bi-product of how [AppSettings](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html) works, so actually case-insensitive should work too (just not snake case).

See [here](https://github.com/kbknapp/clap-rs/blob/313d46abeba76067ac93556c5979a40be2292b6a/tests/app.yml#L5L6) for an example.

EDIT: Also you can see the [17_yaml.rs](https://github.com/kbknapp/clap-rs/blob/313d46abeba76067ac93556c5979a40be2292b6a/examples/17_yaml.rs) and [17_yaml.yml](https://github.com/kbknapp/clap-rs/blob/313d46abeba76067ac93556c5979a40be2292b6a/examples/17_yaml.yml#L7-L8) for some other examples.


---

_Referenced in [clap-rs/clap#271](../../clap-rs/clap/issues/271.md) on 2015-09-22 18:29_

---

_Referenced in [clap-rs/clap#239](../../clap-rs/clap/issues/239.md) on 2015-09-22 18:35_

---

_Comment by @sru on 2015-09-23 16:39_

@kbknapp documentation has not been updated for ~2 weeks. I couldn't find the `from_yaml` function....


---

_Comment by @Vinatorul on 2015-09-23 16:45_

@sru it is updating with every Travis run on master
`from_yaml` does not present because docs are compiling without `--features` flag 

**UPD**: I'll fix it in seconds


---

_Comment by @WildCryptoFox on 2015-09-23 17:08_

@sru don't forget `grep` is you friend. :wink:


---

_Comment by @Vinatorul on 2015-09-23 17:14_

@james-darkfox only on master branch


---

_Comment by @WildCryptoFox on 2015-09-23 17:14_

@Vinatorul I just noticed you said "run on master" and just edited away that section of my comment. Thanks for confirming. :-) 


---

_Comment by @Vinatorul on 2015-09-23 18:02_

I think #276 closes this issue. All `yaml` docs are on `gh-pages` 
http://kbknapp.github.io/clap-rs/clap/index.html


---

_Closed by @Vinatorul on 2015-10-01 05:11_

---
