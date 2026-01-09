---
number: 3096
title: Too much whitespace in output
type: issue
state: closed
author: epage
labels:
  - C-bug
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2021-12-08T21:57:46Z
updated_at: 2021-12-15T16:47:48Z
url: https://github.com/clap-rs/clap/issues/3096
synced_at: 2026-01-07T13:12:19-06:00
---

# Too much whitespace in output

---

_Issue opened by @epage on 2021-12-08 21:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-rc.0

### Minimal reproducible code

```rust
use structopt::StructOpt;

#[derive(StructOpt, Debug)]
#[structopt(author, about)]
struct Opt {}

fn main() {
    let opt = Opt::from_args();
    dbg!(opt);
}

```


### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

```
test-clap 0.1.0      
                                                                         
Me                                                                       
                                    
From Cargo.toml
                                                                         
USAGE:                                                                   
    test-clap                       
                                                                         
OPTIONS:             
    -h, --help       Print help information                              
    -V, --version    Print version information                
```

### Expected Behaviour

Note quite but similar to clap2 which is
```
test-clap 0.1.0                                                          
Me                   
From Cargo.toml
                                                                         
USAGE:                                                                   
    test-clap                       
                                                                         
FLAGS:               
    -h, --help       Prints help information                             
    -V, --version    Prints version information
```

Maybe
```
test-clap 0.1.0                                                          
Me                   

From Cargo.toml
                                                                         
USAGE:                                                                   
    test-clap                       
                                                                         
FLAGS:               
    -h, --help       Prints help information                             
    -V, --version    Prints version information
```

or
```
test-clap 0.1.0 - From Cargo.toml 
Me                   
                                                                         
USAGE:                                                                   
    test-clap                       
                                                                         
FLAGS:               
    -h, --help       Prints help information                             
    -V, --version    Prints version information
```


### Additional Context

This came from [reddit](https://www.reddit.com/r/rust/comments/rbyi41/ann_clap_300rc0/hnri0lw/).

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2021-12-08 21:57_

---

_Label `A-help` added by @epage on 2021-12-08 21:57_

---

_Comment by @pksunkara on 2021-12-09 01:28_

This is intentional IIRC because of https://github.com/clap-rs/clap/issues/1432

---

_Comment by @epage on 2021-12-09 01:51_

Thanks for the context; I had assumed it was done for a reason but hadn't looked yet.

I still think this is worth having as an issue as it is a regression.  We are balancing competing needs and maybe we can find a third way (without just adding a flag)

Granted, once we get proper man generation, it'll lower the reliance on help2man

---

_Comment by @joshtriplett on 2021-12-09 07:14_

This seems like a regression to me as well. Vertical space is precious in help output, especially for commands with lots of options or subcommands.

---

_Comment by @epage on 2021-12-09 12:31_

Here is an example of what help2man parses
```
$ foo --version
GNU foo 1.1

Copyright (C) 2011 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Written by A. Programmer.
$ foo --help
GNU `foo' does nothing interesting except serve as an example for
`help2man'.

Usage: foo [OPTION]...

Options:
  -a, --option      an option
  -b, --another-option[=VALUE]
                    another option

      --help        display this help and exit
      --version     output version information and exit

Examples:
  foo               do nothing
  foo --option      the same thing, giving `--option'

Report bugs to <bug-gnu-utils@gnu.org>.
```
From https://www.gnu.org/software/help2man/

---

_Referenced in [clap-rs/clap#3108](../../clap-rs/clap/issues/3108.md) on 2021-12-09 12:37_

---

_Referenced in [clap-rs/clap#1431](../../clap-rs/clap/issues/1431.md) on 2021-12-10 17:06_

---

_Comment by @I60R on 2021-12-12 11:46_

Looks ugly enough to stay with the current version of `structopt` instead.

Also, what is the point in having man page that completely mirrors --help output? 

---

_Comment by @epage on 2021-12-13 15:27_

> Also, what is the point in having man page that completely mirrors --help output?

I don't think the intent is to be exactly `--help` but to be a superset.  help2man supports [including additional content](https://www.gnu.org/software/help2man/#Including-text).   Its fairly standard for man pages to include a `--help` like output, so its reasonable for wanting to generate that to keep it up to date just like people generate `--help` output.

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-13 22:36_

---

_Added to milestone `3.0` by @epage on 2021-12-13 22:36_

---

_Comment by @epage on 2021-12-14 21:59_

Random notes as I research `help2man` to see if there is any way to reconcile the two needs

[help2man's docs](https://www.gnu.org/software/help2man/) sound like they want the organization to be
1. usage
2. about
3. args
4. additional help text (our `after_long_help`)
5. examples
6. contact or bug reporting

I'm also seeing this pattern in `docker`.  `vim` shows `user-facing name - version` first with no about.  `git` doesn't seem to provide a name or about.  Trying to think of what other programs to look **at.**

I wonder what we can do to encourage examples

From [help2man's docs](https://www.gnu.org/software/help2man/)
> Use argv[0] for the program name in these synopses, just as it is, with no directory stripping. This is in contrast to the canonical (constant) name of the program which is used in --version. 

Interesting.  They want the usage to use `argv[0]` but everywhere else to the use actual name.  

For us, we show the bin name as the application name (see https://github.com/clap-rs/clap/issues/992).  Our bin name is `argv[0].file_name()` though with https://github.com/clap-rs/clap/issues/992 I am considering changing it to `argv[0].file_stem()` or similar.


From [help2man's docs](https://www.gnu.org/software/help2man/)
> It is usually best to alphabetise (by short option name first, then long) within each section, or the entire list if there are no sections. 

Interesting input for #2808 

Now in running `help2man`

clap2 output
```roff
.\" DO NOT MODIFY THIS FILE!  It was generated by help2man 1.47.13.
.TH TEST-CLAP "1" "December 2021" "test-clap 0.1.0" "User Commands"      
.SH NAME
test-clap \- manual page for test-clap 0.1.0
.SH DESCRIPTION                  
test\-clap 0.1.0                                                         
Me                                                                       
From Cargo.toml                     
.SS "USAGE:"                                                             
.IP                                                                      
test\-clap                                                               
.SS "FLAGS:"                       
.TP                                 
\fB\-h\fR, \fB\-\-help\fR
Prints help information
.TP
\fB\-V\fR, \fB\-\-version\fR
Prints version information
.SH "SEE ALSO"
The full documentation for
.B test-clap
is maintained as a Texinfo manual.  If the
.B info
and
.B test-clap
programs are properly installed at your site, the command
.IP
.B info test-clap
.PP
should give you access to the complete manual.
```
clap3 output
```roff
.\" DO NOT MODIFY THIS FILE!  It was generated by help2man 1.47.13.
.TH TEST-CLAP "1" "December 2021" "test-clap 0.1.0" "User Commands"
.SH NAME
test-clap \- manual page for test-clap 0.1.0
.SH DESCRIPTION
test\-clap 0.1.0
.PP
Me
.PP
From Cargo.toml
.SS "USAGE:"
.IP
test\-clap
.SS "OPTIONS:"
.TP
\fB\-h\fR, \fB\-\-help\fR
Print help information
.TP
\fB\-V\fR, \fB\-\-version\fR
Print version information
.SH "SEE ALSO"
The full documentation for
.B test-clap
is maintained as a Texinfo manual.  If the
.B info
and
.B test-clap
programs are properly installed at your site, the command
.IP
.B info test-clap
.PP
should give you access to the complete manual.
```

The only difference is the insertion of `.PP`.  It looks like this is just about having paragraph marks between distinct data so it doesn't merge the lines.

Between people being able to override the help template (including when just code-genning their man page) and #3174, I'm inclined to revert the template change in #2369, so we go back to the clap2 output.

That said, I am all for ideas for overall improvement of the help output!  

---

_Referenced in [clap-rs/clap#1474](../../clap-rs/clap/issues/1474.md) on 2021-12-14 22:02_

---

_Referenced in [clap-rs/clap#3179](../../clap-rs/clap/pulls/3179.md) on 2021-12-15 16:37_

---

_Closed by @epage on 2021-12-15 16:47_

---
