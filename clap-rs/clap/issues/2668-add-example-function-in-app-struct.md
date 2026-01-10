---
number: 2668
title: Add example function in App struct
type: issue
state: closed
author: Ebedthan
labels: []
assignees: []
created_at: 2021-08-07T16:56:21Z
updated_at: 2021-08-09T02:02:14Z
url: https://github.com/clap-rs/clap/issues/2668
synced_at: 2026-01-10T01:27:22Z
---

# Add example function in App struct

---

_Issue opened by @Ebedthan on 2021-08-07 16:56_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33

### Describe your use case

Hi,
It would be great to have a function in the Arg struct that facilitates the addition of command-line examples when the help is printed. This is very often cited as a best practice in designing command-line tools.

### Describe the solution you'd like

I believe that something like this will be really great
```
let m = App::new("prog")
    .author("Me, me@mail.com")
    .version("1.0.2")
    .about("Explains in brief what the program does")
    .examples("prog -i myfile -o myout")
    .get_matches();
```

### Alternatives, if applicable

Actually, as an alternative, I add examples into the `about()` function:

```
let m = App::new("prog")
    .author("Me, me@mail.com")
    .version("1.0.2")
    .about(
          "Explains in brief what the program does\n\
           EXAMPLES:\n\
           \tprog -i myfile -o myout"
    )
    .get_matches();
```
But this does not come with the default clap color pattern for help.

### Additional Context

_No response_

---

_Label `T: new feature` added by @Ebedthan on 2021-08-07 16:56_

---

_Comment by @pksunkara on 2021-08-07 16:57_

> But this does not come with the default clap color pattern for help.

Can you please explain that?

---

_Comment by @Ebedthan on 2021-08-07 17:03_

In the help text printed by clap, heading like USAGE, ARGS, FLAGS are colored in magenta/yellow, but not for the workaround I currently use. This can be added using color crates but I think directly implementing it in clap would be a really nice feature. 

---

_Comment by @pksunkara on 2021-08-09 02:02_

That would be solved by #1790 

---

_Closed by @pksunkara on 2021-08-09 02:02_

---
