---
number: 96
title: "clap-rs 0.7.1: dynamic usage strings seem 'inverted'"
type: issue
state: closed
author: Byron
labels:
  - C-bug
assignees: []
created_at: 2015-05-02T07:20:28Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/96
synced_at: 2026-01-07T13:12:19-06:00
---

# clap-rs 0.7.1: dynamic usage strings seem 'inverted'

---

_Issue opened by @Byron on 2015-05-02 07:20_

With a program exhibiting the following grammar ...

``` Bash
$ groupsmigration1 archive insert --help`
groupsmigration1-archive-insert
Inserts a new mail into the archive of the Google group.

USAGE:
    groupsmigration1 archive insert <group-id>  [FLAGS] [OPTIONS] [ -u <mode> <file> ]

FLAGS:
    -h, --help       Prints help information
    -v, --version    Prints version information

OPTIONS:
    -m <mime>          The file's mime time, like 'application/octet-stream'
    -u <mode> <file>        Specify the upload protocol (simple|resumable) and the file to upload
    -o <out>           Specify the file into which to write the programs output
    -p <v>...          Set various fields of the request structure

POSITIONAL ARGUMENTS:
    group-id    The group ID
```

... I would expect a partial invocation like `groupsmigration1 archive insert` to yield a usage more along the lines of ... 

``` bash
One or more required arguments were not supplied: <group-id>, '-u <mode> <file>'
USAGE:
    groupsmigration1 archive insert <group-id> -u <mode> <file>
For more information try --help
```

... but what you get is just as follows:

``` bash
One or more required arguments were not supplied
USAGE:
    groupsmigration1 archive insert
For more information try --help
```

Another example for missing required parameters is `groupsmigration1 archive insert -u simple README.md` where `<group-id>` is missing. However, this is not actually said in the resulting usage text:

``` bash
$ groupsmigration1 archive insert -u simple README.md
One or more required arguments were not supplied
USAGE:
    groupsmigration1 archive insert    [ -u <mode> <file> ]
For more information try --help
```

What I would expect here is something like this:

``` bash
One or more required arguments were not supplied: <group-id>
USAGE:
    groupsmigration1 archive insert <group-id> -u simple README.md
For more information try --help
```

In a call like `groupsmigration1 archive insert group` one will see a usage string like this:

``` bash
One or more required arguments were not supplied
USAGE:
    groupsmigration1 archive insert  <group-id>
For more information try --help
```

and I think think it would be much better to get something like that:

``` bash
One or more required arguments were not supplied: -u <mode> <file>
USAGE:
    groupsmigration1 archive insert group -u <mode> <file>
For more information try --help
```

Once the usage is improved, `clap` will finally be top-of-the-line when it comes to usability I think, and I really want it to be there :).
Thanks for taking care.


---

_Referenced in [Byron/google-apis-rs#92](../../Byron/google-apis-rs/issues/92.md) on 2015-05-02 07:32_

---

_Referenced in [clap-rs/clap#88](../../clap-rs/clap/issues/88.md) on 2015-05-02 07:43_

---

_Comment by @kbknapp on 2015-05-02 15:27_

Thanks for the detailed report! I agree, as I was thinking about this last night, it should state which arguments were missing such as your last:

```
One or more required arguments were not supplied: -u <mode> <file>
USAGE:
    groupsmigration1 archive insert group -u <mode> <file>
For more information try --help
```

This should be a decently simple change. I'll start working on this today.

Also, as for why `<group-id>` is getting dropped from the requirements list, this could be a bug. Could you show me where the argument definitions for the `insert` subcommand are located?

One last kind of side note, the `[` and `]` around the `-u <mode> <file>` I'm on the fence about if I should take them out or not (for the usage string only, not for the new "missing required" error message). I was originally worried that if I user saw:

```
groupsmigration1 archive insert <group-id> -u <mode> <file>
```

They may mistakenly think `<file>` is not part of `-u` until they see the full help message. If a program had more than one positional argument, and some of which were optional, they could mistakenly do something like

```
groupsmigration1 archive insert <group-id> <file> -u <mode> <some_other_positional>
```

I'll post back here once I've got fixes!


---

_Label `bug` added by @kbknapp on 2015-05-02 15:27_

---

_Comment by @kbknapp on 2015-05-02 16:39_

I was able to reproduce the bug with this snippet, but if you have a link to your source I'd still like to see that to see how exactly your requirements are set up so I don't fix the bug in my test case, but not in your real case.

``` rust
let u_names = ["mode", "file"];
let matches = App::new("MyApp")
                    .arg(Arg::from_usage("-f [f]... 'some value'")
                              .value_names(&u_names)
                              .required(true))
                    .arg(Arg::from_usage("<gid> 'other value'"))
                    .get_matches();
```

Which produces grammarr:

``` sh
fake 

USAGE:
    fake <gid>  [FLAGS] [ -f <mode> <file> ] 

FLAGS:
    -h, --help       Prints help information
    -v, --version    Prints version information

OPTIONS:
    -f <mode> <file>        some value

POSITIONAL ARGUMENTS:
    gid    other value
```

And when run with incomplete `$ fake`:

``` sh
One or more required arguments were not supplied
USAGE:
    fake    
For more information try --help
```


---

_Closed by @kbknapp on 2015-05-03 04:40_

---

_Comment by @kbknapp on 2015-05-03 04:47_

Take a look at the latest version (0.7.2 on crates.io) or master here and let me know if that works for you. It now tells you exactly which arguments you're missing (if any), along some visual clean up (spaces between arguments). I still need to fix the tab alignment in the help info, but I may wait on that as it's minor right now.

Also, the requirements all should be working, i.e. you can make an argument required by default, and if it requires other arguments, those arguments will be listed in the default usage string as well...kind of "required by proxy"

One other visual thing I changed was the `[` and `]` around arguments with named values, those have been removed because with the new requirements error messages it's quite explicit about what belongs with what. And the square brackets make an argument look "optional"


---

_Comment by @Byron on 2015-05-05 07:09_

I finally got around to testing the modifications (with v0.7.5 actually), and must say that the help and usage strings now seems perfect. I'd have nothing to add here, except for saying thanks !


---
