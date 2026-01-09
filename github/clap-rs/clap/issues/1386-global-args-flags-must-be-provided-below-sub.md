---
number: 1386
title: Global args/flags must be provided below sub-command
type: issue
state: closed
author: ctron
labels: []
assignees: []
created_at: 2018-11-19T14:52:22Z
updated_at: 2022-09-02T15:05:32Z
url: https://github.com/clap-rs/clap/issues/1386
synced_at: 2026-01-07T13:12:19-06:00
---

# Global args/flags must be provided below sub-command

---

_Issue opened by @ctron on 2018-11-19 14:52_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

~~~
rustc 1.30.1 (1433507eb 2018-11-07)
~~~

### Affected Version of clap

~~~
2.30.0
~~~

### Bug or Feature Request Summary

Bug: Configuring a global argument/flag on a subcommand requires to pass in the arg/flag after specifying the sub-command on the command line.

### Expected Behavior Summary

It should be the possible to specify the arg/flag at any position on the command line.

### Actual Behavior Summary

It is not.

### Steps to Reproduce the issue

See source code.

### Sample Code or Link to Sample Code

~~~rust
fn main() {
    let app = App::new("foo")

        .subcommand(SubCommand::with_name("sub1")

            .arg(Arg::with_name("arg1")
                .long("arg1")
                .takes_value(true)
                .global(true)
            )

            .subcommand(SubCommand::with_name("sub1a")
            )
        )
    ;

    let m1 = app.get_matches_from(&["foo",  "--arg1", "v1", "sub1", "sub1a"]); // fails

    println!("{:#?}", m1);

    let v1 = m1.subcommand_matches("sub1").unwrap().subcommand_matches("sub1a").unwrap().value_of("arg1").unwrap();
    println!("{}", v1);
}
~~~

The output will be:

~~~
error: Found argument '--arg1' which wasn't expected, or isn't valid in this context
	Did you mean to put '--arg1' after the subcommand 'sub1'?

USAGE:
    foo [SUBCOMMAND]

For more information try --help
~~~

### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2", features = ["debug"] }
```

<details>
<summary> Debug Output </summary>
<pre>
<code>

    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap1`
DEBUG:clap:Parser::add_subcommand: term_w=None, name=sub1a
DEBUG:clap:Parser::add_subcommand: term_w=None, name=sub1
DEBUG:clap:Parser::propagate_settings: self=foo, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=sub1, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=sub1, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=sub1a, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=sub1a, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--arg1"' ([45, 45, 97, 114, 103, 49])
DEBUG:clap:Parser::is_new_arg:"--arg1":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--arg1"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Didn't match anything
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=foo [SUBCOMMAND]
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
error: Found argument '--arg1' which wasn't expected, or isn't valid in this context
	Did you mean to put '--arg1' after the subcommand 'sub1'?

USAGE:
    foo [SUBCOMMAND]

For more information try --help

Process finished with exit code 1


</code>
</pre>
</details>


---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 15:27_

---

_Comment by @CreepySkeleton on 2020-02-06 09:31_

From the doc
> Global arguments only propagate down, **not up**

This is the correct way to do it:
```rust
let m = App::new("foo")
        .arg(Arg::with_name("arg1")
            .long("arg1")
            .takes_value(true)
            .global(true)
        )
        .subcommand(SubCommand::with_name("sub1")
            .subcommand(SubCommand::with_name("sub1a"))
        ).get_matches();

    println!("{:#?}", m);
    println!("{:?}", m.subcommand_matches("sub1").unwrap().subcommand_matches("sub1a").unwrap().value_of("arg1"));
```

---

_Closed by @CreepySkeleton on 2020-02-06 09:31_

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 09:31_

---

_Comment by @yyin-dev on 2022-09-02 15:01_

Is it possible to do similar things (global argument) using derive syntax?

---

_Comment by @epage on 2022-09-02 15:05_

Any builder method can be used as a "raw attribute".  See https://docs.rs/clap/latest/clap/_derive/index.html#terminology

---
