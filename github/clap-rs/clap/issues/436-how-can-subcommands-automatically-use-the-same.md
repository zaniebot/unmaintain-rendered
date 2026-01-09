---
number: 436
title: How can subcommands automatically use the same version as the parent command?
type: issue
state: closed
author: clux
labels: []
assignees: []
created_at: 2016-03-01T10:03:59Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/436
synced_at: 2026-01-07T13:12:19-06:00
---

# How can subcommands automatically use the same version as the parent command?

---

_Issue opened by @clux on 2016-03-01 10:03_

Hi there.

I am writing an app of a form like:

``` rs
let args = App::new("blast")
                   .version("1.2.3")
                   .subcommand(SubCommand::with_name("status").help("help str"))
                   .get_matches();
```

And it appears `blast status -h` will suggest `-V` as a flag for `blast status`, but if you actually try this, it will just print the version as an empty string. I'm not sure if it necessarily needs to inherit from the app version, but it looks a little weird listing it in the subcommand's help if it is not set.

I am using:
- clap = "2.1.2"
- rustc 1.6.0 (c30b771ad 2016-01-19)

(Btw, other than this slight quirk, this library is fantastic. Thanks for your hard work.)


---

_Comment by @kbknapp on 2016-03-02 08:48_

By default subcommands don't inherit the version of the parent command, this allows them to be developed as a separate program entirely (should that ever be required).

You can, however, tell `clap` to simply propagate the version of the parent down through all child subcommands if you'd like them to all have the same version.

To do this, you use the [`AppSettings::GlobalVersion`](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#examples-3) enum variant along with the `App::setting` method.

``` rust
let args = App::new("blast")
                   .setting(AppSettings::GlobalVersion) // all subcommands have version "1.2.3" now
                   .version("1.2.3")
                   .subcommand(SubCommand::with_name("status").help("help str"))
                   .get_matches();
```


---

_Label `T: RFC / question` added by @kbknapp on 2016-03-02 08:50_

---

_Renamed from "subcommands and versions slight issue" to "How can subcommands automatically use the same version as the parent command?" by @kbknapp on 2016-03-02 08:50_

---

_Comment by @clux on 2016-03-02 10:20_

Ah, I see. That's one useful solution.

Alternatively though; is there a way to remove the version from showing up in subcommand help?

`./blast -h` and `./blast status -h` will both suggest looking at version even if the app was missing a `.version()` call. That is still misleading, and pollutes the help of small subcommands with few flags.


---

_Comment by @kbknapp on 2016-03-08 12:44_

@clux  Sorry, my free time has been non-existent lately! 

Yes, you can use the [`AppSettings::VersionlessSubcommands`](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#examples-4) enum variant to remove the version flag entirely.


---

_Comment by @clux on 2016-03-08 13:31_

Ooh, perfect. Thank you very much. I had entirely missed the documentation of that enum. Lots of useful stuff in there! I will close this now as it works perfectly.


---

_Closed by @clux on 2016-03-08 13:31_

---

_Comment by @kbknapp on 2016-03-08 13:32_

:+1: 


---
