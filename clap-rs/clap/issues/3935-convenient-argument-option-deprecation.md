---
number: 3935
title: convenient argument/option deprecation
type: issue
state: closed
author: uepoch
labels:
  - C-enhancement
assignees: []
created_at: 2022-07-14T11:25:01Z
updated_at: 2022-07-14T12:56:08Z
url: https://github.com/clap-rs/clap/issues/3935
synced_at: 2026-01-10T01:27:49Z
---

# convenient argument/option deprecation

---

_Issue opened by @uepoch on 2022-07-14 11:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.8

### Describe your use case

I'm maintaining a CLI with a decent install-base in corporate environment (in the few 10ks of installs).
The CLI itself is a clap-derived collection of many subcommand doing various actions, but it's not relevant for this discussion.

We have many dev collaborating on it, from various teams and rust background and they often operate with some level of autonomy when it comes to the specifics of each command.

As time passes, we inevitably have to deprecate some arguments, but we can't really just remove them because they may be included in bash scripts, aliases, or any other weirdish setup.

Right now, I didn't find a satisfying way to handle deprecation of arguments in a standardised way for all the subcommands, and I think it may be something that clap could help doing.

I wish I could have a way to mark an attribute as deprecated, with various options for formatting a helpful warning (they are random ideas, but I want to collect thoughts on the high-level goal):
- Since when is it deprecated ?
    - Opt: support a arbitrary string interpreted as a version, or a date, doesn't really have to be validated
- At which point will it be retired (same as above)
- Is it just a warning or a proper usage error (with a message like "This field has been marked as retired since X, use <insertworkaround> to ignore this message"

As more projects grow older with clap, I suspect this will become something interesting to add to the whole clap-experience 

I've read #3321 and while I share some of the thoughts, my use-case is mostly about providing a standard user experience for deprecation 

### Describe the solution you'd like

At the moment I have a command like this:
```
#[derive(Parser)]
#[clap(version = "1.2.0")]
struct FooCLI {
    #[clap(short, long)]
    foo: Option<String>,
    
    /// Bar
    #[clap(short, deprecated(since="0.9", error="1.1", message="Use foo instead")]
    garbage: i32,
    /// Foo
    #[clap(short, deprecated)]
    desc: String,
}
```

If I run `foo-cli -g 10 -d "yolo"` I expect something in the likes of:
```
warn: The following deprecated argument was provided: desc. 
error: The following deprecated argument was provided: garbage. It has been deprecated since 0.9 and is retired since 1.1. Use foo instead.

USAGE:
    foo-cli  <FOO>

For more information try --help
```
I think theses macro attributes could be somewhat easily translated into builder API methods, but I may be missing important bits here

Short help would hide by default the deprecated flags, long help would display them anyway with:
```
USAGE:
    foo-cli [OPTIONS] [FOO]

ARGS:
    <FOO>    

OPTIONS:
    -d <DESC>           Foo. Deprecated
    -g <GARBAGE>        Bar. Deprecated: Use foo instead
    -h, --help          Print help information
    -V, --version       Print version information
```

I understand this would be harder to implement since clap default's to short help if no long-help exists, and that would require another layer of filtering, maybe adding them anyway would be enough

### Alternatives, if applicable

Nothing prevents us from technically doing it ourselves, but there's a lot of wheel reinvention:
- Have each command do its own game of parsing, manually handling erroring etc
  - Technically works, but I feel there's a room for it in clap so the user benefit from the same output style and devs have an easy way to mark deprecation in their struct
  
- Instead of passing an attribute to a specific field:

```
#[derive(DeprecatedArgs)]
 struct OldArgs {
    #[deprecate(since="1.0", error)]
    garbage: i32
}

#[derive(DeprecatedArgs)]
 struct OldEnum {
    #[deprecate(error)]
    OldFoo,
    OldBar,
}


#[derive(Parser)]
#[clap(version = "1.0.0")]
struct FooCLI {
    foo: String,
    /// Foo
    #[clap(deprecated, flattened)]
    old: OldArgs,
    #[clap(deprecated, flattened)]
    old_enum: OldEnum,
}
```
It forces the maintainer to move theses in specific constructs, maybe making it more obvious in the long term what's deprecated or not




### Additional Context

If it is something you would like to add, I'm happy to work on the implementation for the PR

---

_Label `C-enhancement` added by @uepoch on 2022-07-14 11:25_

---

_Comment by @epage on 2022-07-14 12:10_

Since #3321 and this bother cover deprecations, could you expand on why this is a separate issue rather than providing feedback on that with your use case and requirements?  Currently, I lean towards closing this as a duplicate.

#3321 is about the high level need and includes a proposed solution.  Other solutions would fit within that conversation.

---

_Comment by @uepoch on 2022-07-14 12:33_

What I though differs from #3321 is that this only focuses on deprecation rather than instability, and it sounded from your answer on #3321 that you wanted something more generic for custom user validation. So I went and proposed a UX solution for deprecation only, explaining with a use-case (without going in details about custom validation) before going in implementation details.

I'm happy to close/ get this marked as a duplicate and rewrite it as comments if you feel this belongs in the other issue and want to group both theses things, sorry for the inconvenience :)

---

_Comment by @epage on 2022-07-14 12:43_

My comment is one possible solution.  We should focus issues on the high level problem and not just the solution discussed for that problem.  If we want to split the conversation between the two topics, we should do that consciously.

Going to go ahead and close this as a duplicate.

---

_Closed by @epage on 2022-07-14 12:43_

---

_Comment by @uepoch on 2022-07-14 12:56_

Fair 👍 

---
