```yaml
number: 1729
title: "Any tools that can \"convert\" python-argparse code to rust-clap code?"
type: issue
state: closed
author: kaiakz
labels: []
assignees: []
created_at: 2020-03-07T15:03:15Z
updated_at: 2020-06-30T23:10:28Z
url: https://github.com/clap-rs/clap/issues/1729
synced_at: 2026-01-12T16:14:11Z
```

# Any tools that can "convert" python-argparse code to rust-clap code?

---

_@kaiakz_

I am rewriting a python project in rust now. Its cmd parser uses `argparse`. 
I would like to ask if there are any tools that can generate the related `clap` code?
I found many similarities between the two libraries. I think I can write a script to do this. :smile: 

---

_Label `T: RFC / question` added by @kaiakz on 2020-03-07 15:03_

---

_Comment by @CreepySkeleton on 2020-03-07 20:21_

They are similar indeed, but the devil in the detail.

Python and Rust are very different in the sense that Python is dynamically typed while Rust is statically typed (thanks, Cap!). As a consequence, the values you get from argparse have the type you specified via `type=fn`, but clap is "stringly typed", you take a *string* from it, further conversion is up to you. Well, this is not hard limitation, you can generate the "converting post-step" as well as the app.

This is pretty much doable otherwise. 

I'm not aware of such existing implementations and I don't think I'll ever get to writing a script like that on my own (I haven't been a heavy user of python lately); but, if somebody feels like taking this on, I might be able to provide some help. Consider it mentored.

---

_Label `help wanted` added by @CreepySkeleton on 2020-03-07 20:21_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-03-07 20:21_

---

_Label `Z: mentored` added by @CreepySkeleton on 2020-03-07 20:21_

---

_Comment by @pickfire on 2020-03-14 00:27_

Do we have any existing codegen tools to generate and convert between clap inputs (macro, struct, yaml, procedural)?

---

_Comment by @CreepySkeleton on 2020-03-14 00:29_

> macro, struct, yaml, procedural

Mind explaining what you mean?

---

_Comment by @pickfire on 2020-03-14 01:26_

Say we want to generate clap code for this, but which output should we generate as? If we generate procedural code such as `App::new()`, people may want `#[derive(Clap)]`, maybe we only generate to one type from python argparse then have a converter to convert to the other types?

---

_Comment by @CreepySkeleton on 2020-03-14 18:17_

In my understanding, such a conversion script (very likely written in Python) will have the following structure:
* Take the `ArgumentParser` object
* Make some sort of intermediate clap-like representation from it
* Generate the rust code based on the IR

The last step can be modified to emit rust code in different formats (derive, yaml, raw-app, etc..), no problem with that. 

---

_Comment by @pickfire on 2020-03-16 01:29_

@CreepySkeleton Okay, can I help out with this? But for the first step, `ArgumentParser` just take from Python right?

---

_Comment by @pksunkara on 2020-03-16 03:39_

I would vote to close this issue since it is out of scope for clap project. If anyone wants to create such tool, please feel free to do it.

---

_Comment by @kaiakz on 2020-03-16 05:48_

I am trying to parse the python code by `ast`, and then convert them to the related clap-rs(Builder Pattern).
Just like:
```
ArgumentParser(description="Does awesome things") --> App::new().about("Does awesome things")
argparser.add_argument("-q", "--quiet", dest="quiet", action="count", default=0,
                        help="Silences one level of message, least-important first.") -->            .arg(Arg::with_name("quiet").short("q").long("quiet").multiple(true).help("Silences one level of message, least-important first."))
add_subparsers().add_parser() --->  .subcommand()
```
I'm curious what argparse can do but clap can't. 
I will create a new project to do so :) Thanks for your reply!

---

_Comment by @pickfire on 2020-03-16 13:45_

@kaiakz Why not just run python to get the `ArgumentParser` and then only use rust to interface with the python `ArgumentParser` object?

---

_Comment by @CreepySkeleton on 2020-03-17 21:24_

@pickfire  @kaiakz You people seem to be thinking about writing the converter in Rust, possibly employing a Python interpreter. I think it would be easier to write it in Python: a function that takes the `ArgumentParser` instance and returns the Rust source code as plain text (or writes it into a file).



> I would vote to close this issue since it is out of scope for clap project. If anyone wants to create such tool, please feel free to do it.

It is out of scope indeed, but such a project is closely related to clap nonetheless. In fact, we already have [the issue](https://github.com/clap-rs/clap/issues/1337) which is not directly related to clap itself, it can be implemented by a third party just fine; yet, kept open because some of the members are interested in watching it over. 

We may or may not want to maintain this project by ourselves, but it would be useful to mention it existence in the readme - if or when this project is ready to use. I can also provide some sort of indirect help to anyone who's willing to get it done.

---

_Comment by @pickfire on 2020-03-18 07:56_

@CreepySkeleton I am thinking of both, using python to parse and evaluate python code. Then use rust to take that python object into rust code (since existing rust formatter and code generation is available), that combines the best of both world.

---

_Comment by @CreepySkeleton on 2020-03-18 08:10_

> Then use rust to take that python object into rust code

I don't see the point, but, of course, you are free to do as you see fit.

---

_Comment by @CreepySkeleton on 2020-06-30 10:12_

Closing as inactive. Nobody showed any interest.

---

_Closed by @CreepySkeleton on 2020-06-30 10:12_

---

_Label `Z: mentored` removed by @pksunkara on 2020-06-30 23:10_

---

_Label `help wanted` removed by @pksunkara on 2020-06-30 23:10_

---
