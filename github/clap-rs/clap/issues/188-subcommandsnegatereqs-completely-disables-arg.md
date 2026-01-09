---
number: 188
title: SubcommandsNegateReqs completely disables Arg.required
type: issue
state: closed
author: wenLiangcan
labels:
  - C-bug
  - A-builder
assignees: []
created_at: 2015-08-24T14:00:46Z
updated_at: 2015-08-24T18:48:20Z
url: https://github.com/clap-rs/clap/issues/188
synced_at: 2026-01-07T13:12:19-06:00
---

# SubcommandsNegateReqs completely disables Arg.required

---

_Issue opened by @wenLiangcan on 2015-08-24 14:00_

``` rust
App::new("my_app")
                .setting(AppSettings::SubcommandsNegateReqs)
                .arg(Arg::with_name("test")
                       .required(true)
                       .index(1))
                .subcommand(SubCommand::with_name("sub1"))
                .subcommand(SubCommand::with_name("sub1"))
```

While running:

``` shell
$ my_app
```

`.required(true)` has no effect.


---

_Comment by @Vinatorul on 2015-08-24 14:11_

Hello, as you can read in documentation about `AppSettings::SubcommandsNegateReqs`.

> Allows subcommands to override all requirements of the parent (this command). For example if you had a subcommand or even top level application which had a required arguments that are only required as long as there is no subcommand present.
> 
> **NOTE**: This defaults to false (using subcommand does not negate requirements)

So it is expected behavior.
If you don't want for subcommands to override requires - just delete `.setting(AppSettings::SubcommandsNegateReqs)` call.

Please let me know if you still have problems or not.


---

_Label `T: RFC / question` added by @Vinatorul on 2015-08-24 14:24_

---

_Label `C: args` added by @Vinatorul on 2015-08-24 14:24_

---

_Comment by @wenLiangcan on 2015-08-24 14:37_

But in my opinion, I think `<test>` should be required when running `my_app` without `sub1` and `sub2`.   

I have no idea why we need `SubcommandsNegateReqs` if it should behave as you described, because we can simply omit the declaration of the parent's requirements or declare the parent without the subcommands.


---

_Comment by @Vinatorul on 2015-08-24 14:38_

Oh, I'm sorry, you are right, it's a bug.


---

_Label `T: bug` added by @Vinatorul on 2015-08-24 14:38_

---

_Label `P1: urgent` added by @Vinatorul on 2015-08-24 14:38_

---

_Label `T: RFC / question` removed by @Vinatorul on 2015-08-24 14:38_

---

_Comment by @kbknapp on 2015-08-24 14:43_

@wenLiangcan Thanks for finding and taking the time to file this issue! As @Vinatorul said, it's in fact a bug. We'll get this patched and put out v1.2.3 shortly :+1: 


---

_Label `D: easy` added by @kbknapp on 2015-08-24 14:43_

---

_Comment by @Vinatorul on 2015-08-24 14:44_

@kbknapp I can do it, when come home in about one hour.


---

_Comment by @kbknapp on 2015-08-24 14:44_

Sounds good thanks!


---

_Assigned to @Vinatorul by @kbknapp on 2015-08-24 14:44_

---

_Comment by @wenLiangcan on 2015-08-24 14:46_

Thanks for the quick responses, @Vinatorul , @kbknapp :smile: 


---

_Label `C: app` added by @Vinatorul on 2015-08-24 17:17_

---

_Referenced in [clap-rs/clap#189](../../clap-rs/clap/pulls/189.md) on 2015-08-24 17:24_

---

_Closed by @kbknapp on 2015-08-24 18:29_

---

_Comment by @kbknapp on 2015-08-24 18:35_

@wenLiangcan the new version (v1.2.3) has been published to crates.io thanks to @Vinatorul 

Let us know if you have any other questions or issues! :smile: 


---

_Comment by @wenLiangcan on 2015-08-24 18:45_

@kbknapp Thank you, I tried the new version and it works well now! 

Thanks for your great job, @Vinatorul :clap: 


---

_Comment by @Vinatorul on 2015-08-24 18:48_

At your service ;)


---
