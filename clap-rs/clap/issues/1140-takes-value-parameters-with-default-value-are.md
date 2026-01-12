```yaml
number: 1140
title: "`takes_value()` parameters with `default_value()` are shown in USAGE like mandatory"
type: issue
state: closed
author: albel727
labels:
  - C-enhancement
  - A-help
  - E-medium
assignees: []
created_at: 2018-01-05T22:04:12Z
updated_at: 2022-02-08T18:39:16Z
url: https://github.com/clap-rs/clap/issues/1140
synced_at: 2026-01-12T16:14:10Z
```

# `takes_value()` parameters with `default_value()` are shown in USAGE like mandatory

---

_@albel727_

### Affected Version of clap
clap 2.29.0

### Expected Behavior Summary
When command line parsing fails, a USAGE section is shown. This usage section should NOT contain non-required parameters with default values. Suppose there are two parameters, "required" and "defaulted", and the user didn't specify the required one. The output should be as follows

```
error: The following required arguments were not provided:
    <required>

USAGE:
    prog [OPTIONS] <required>

For more information try --help
```

### Actual Behavior Summary
Defaulted option parameters are mentioned in parse error help for no particular reason, confusingly looking like they're mandatory, even if they're `required(false)`.

```
error: The following required arguments were not provided:
    <required>

USAGE:
    prog <required> --defaulted <defaulted>

For more information try --help
```

### Steps to Reproduce the issue
Utilize the following code. Note that if one comments out ".default_value()", the parse error no longer contains the mention of the defaulted parameter, as expected.

### Sample Code or Link to Sample Code
```rust
    App::new("Test")
        .arg(
            Arg::with_name("required")
                .required(true)
        )
        .arg(
            Arg::with_name("defaulted")
                .takes_value(true)
                .short("d")
                .long("defaulted")
                .default_value("1")
        ).get_matches();
```


---

_Renamed from "`takes_value()` parameters with `default_value()` are shown in USAGE like required" to "`takes_value()` parameters with `default_value()` are shown in USAGE like mandatory" by @albel727 on 2018-01-05 22:07_

---

_Comment by @albel727 on 2018-01-05 22:28_

It's particularly annoying that this shadows all other flags. So

```rust
    App::new("Test")
        .arg(
            Arg::with_name("required")
                .required(true)
        )
        .arg(
            Arg::with_name("defaulted")
                .takes_value(true)
                .short("d")
                .long("defaulted")
                .default_value("1")
        )
        .arg(
            Arg::with_name("another")
        )
        .get_matches_from(vec!["prog"]);
```

produces

```
USAGE:
    prog <required> --defaulted <defaulted>
```
instead of
```
USAGE:
    prog [OPTIONS] <required> [another]
```

---

_Comment by @kbknapp on 2018-01-09 18:08_

The error messages show "used arguments" i.e. "this possibly what the command you just tried running *should* have been run"

But yes, defaults get thrown in there too. I can do some work to remove default values from error messages. This wouldn't however "turn off" the shadowing. With that fix

```
$ prog another
error: missing required [..snip]

USAGE:
    prog <required> --defaulted <defaulted> [another]
```

Would become

```
$ prog another
error: missing required [..snip]

USAGE:
    prog <required> [another]
```

I'm open to adding an option to *turn off* this "smart usage" so that the error would become just the default:


```
$ prog another
error: missing required [..snip]

USAGE:
    prog [FLAGS] [OPTIONS] <required>
```

(you can already *kind of* do this [by just supplying your own usage string](https://docs.rs/clap/2.29.0/clap/struct.App.html#method.usage), which will get used no matter what.)




---

_Comment by @kbknapp on 2018-01-09 18:08_

If someone wants to add this setting to disable the "smart usage" I'd be happy to mentor it. Removing the defaults from the usage string is probably more complicated than just adding this setting.

---

_Comment by @albel727 on 2018-01-10 20:33_

I see. I'll consider the possible ways to proceed from here, after I take a look at the clap sources when I'm done with other things. For the time being, I have another alternative to posit.

How hard would it be to, at least, make the defaulted but non-required arguments be printed in square brackets, so as to convey their non-mandatory nature? I.e. something like

```
USAGE:
    prog <required> [--defaulted <defaulted>] [another]
```

Or do you think that this would violate user's expectations, making them believe that they have to put "--defaulted" in square brackets in order to use it?

---

_Comment by @kbknapp on 2018-01-15 19:36_

> I.e. something like

It would be fine, but almost as much work as simply not printing the defaults (which is a *slightly* better solution IMO). 

[In the source](https://github.com/kbknapp/clap-rs/blob/e078a5893b9cdc367fe1621d529dfbbcc93a9095/src/app/usage.rs#L22-L45) for either of these two fixes, when clap is printing the usage string, it doesn't know if something is a default value or not, it just sees an argument was used (used by user, or used by default). So it would require re-looping through the used arguments to see if they have a default, and if so if the value that was used was in fact the default.

---

_Label `T: enhancement` added by @kbknapp on 2018-07-22 03:01_

---

_Label `P4: nice to have` added by @kbknapp on 2018-07-22 03:01_

---

_Label `D: hard` added by @kbknapp on 2018-07-22 03:01_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 03:01_

---

_Label `C: usage strings` added by @kbknapp on 2018-07-22 03:01_

---

_Referenced in [arusahni/git-req#41](../../arusahni/git-req/pulls/41.md) on 2019-12-30 01:44_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:17_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#86](../../epage/clapng/issues/86.md) on 2021-12-06 16:39_

---

_Comment by @epage on 2021-12-08 20:03_

In some ways, this reminds me of #3076 

---

_Label `A-help` added by @epage on 2021-12-08 20:04_

---

_Label `C: usage strings` removed by @epage on 2021-12-08 20:04_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 18:03_

---

_Label `E-hard` removed by @epage on 2021-12-09 18:03_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 18:03_

---

_Label `E-medium` added by @epage on 2021-12-09 18:04_

---

_Added to milestone `3.x` by @epage on 2022-01-04 13:48_

---

_Removed from milestone `3.x` by @epage on 2022-02-02 19:50_

---

_Added to milestone `3.1` by @epage on 2022-02-02 19:50_

---

_Comment by @epage on 2022-02-08 18:39_

This was fixed by https://github.com/clap-rs/clap/pull/2609

---

_Closed by @epage on 2022-02-08 18:39_

---
