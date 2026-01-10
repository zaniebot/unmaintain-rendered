---
number: 86
title: stackoverflow when building rather large clap command trees
type: issue
state: closed
author: Byron
labels:
  - C-enhancement
assignees: []
created_at: 2015-04-29T08:04:26Z
updated_at: 2018-08-02T03:29:38Z
url: https://github.com/clap-rs/clap/issues/86
synced_at: 2026-01-10T01:26:23Z
---

# stackoverflow when building rather large clap command trees

---

_Issue opened by @Byron on 2015-04-29 08:04_

Even though this issue is not due to a bug in `clap`, I thought I might post it here for your information only.

For more information, please have a look at the respective [question on stackoverflow](http://stackoverflow.com/questions/29937697/building-a-large-app-with-clap-causes-thread-main-has-overflowed-its-st).


---

_Closed by @Byron on 2015-04-29 08:04_

---

_Reopened by @Byron on 2015-04-29 14:24_

---

_Comment by @Byron on 2015-04-29 14:27_

I just reopen this one to send a message :): I just tried it with the latest (the one that uses Boxes internally), and was unable to validate that this fixes the issue. The point is that the Box doesn't make a difference here. After all, it seems rust tries to allocate one `App` structure on the stack per call that returns one, which is basically every method you call.

Therefore I highly recommend undoing commit e5613ec1563bfa1e6f2981fa892e5823c8184a95 as it changes the performance characteristics of the implementation, without having any positive side-effect.

If you have any questions, please let me know.
You can also jump to the last 10 minutes of [this video](http://youtu.be/lYe--RXqNJc) to see what I was doing to come to the conclusion.


---

_Closed by @Byron on 2015-04-29 14:27_

---

_Comment by @kbknapp on 2015-04-29 15:20_

It's been un-done. Thanks for the feedback, I'll take a look and see what I can do!


---

_Comment by @Byron on 2015-04-29 15:27_

I don't think there is anything you can do except for getting rid of the builder pattern as is. Naively I'd think returning an `&mut App` would do the job, but last time I tried that I got lifetime issues. Since it was many weeks ago, it might be worth trying that again due to changed borrowchk semantics.


---

_Renamed from "stackoverflow when building rather larger clap command trees" to "stackoverflow when building rather large clap command trees" by @Byron on 2015-04-29 17:34_

---

_Comment by @Byron on 2015-04-29 17:35_

For completeness, here is _one_ way to circumvent overflows: https://github.com/Byron/google-apis-rs/blob/clap/gen/dfareporting2d1-cli/src/main.rs . It basically shifts all the work to the runtime, feeding of statically allocated, simple nested structures.


---

_Comment by @kbknapp on 2015-04-29 18:49_

The original design used `&mut App`, but there were issues because some items were owned and had to be transferred to the `ArgMatches` struct upon calling `get_matches()`...but this was also a while back. I'm going to re-look into the issue and see if things have changed (either with `clap` or Rust) since I last checked ;)


---

_Reopened by @kbknapp on 2015-04-29 18:49_

---

_Comment by @kbknapp on 2015-04-29 18:51_

Conversely, the way you ended up working around this is almost exactly what I had in mind. I'll post back with progress.


---

_Label `enhancement` added by @kbknapp on 2015-04-30 01:47_

---

_Label `postponed` added by @kbknapp on 2015-04-30 01:47_

---

_Comment by @kbknapp on 2015-04-30 01:47_

I changed to a borrowed builder pattern but that actually created more problems than it solved. The main issue stems from subcommands...so being that you do have a work around for the time being, I'm going to close this with a post-poned label until I can figure out another method.


---

_Closed by @kbknapp on 2015-04-30 01:47_

---

_Comment by @Byron on 2015-04-30 05:32_

Thanks for trying. Do you have the intermediate result around somewhere, maybe in a branch so I could have a look ? I was always wondering why the &mut builder pattern doesn't really work ... in C++ for instance returning references is the default way for example, and even the borrow checker should consider it safe.

What I see there is that one would have to store the initial App object somewhere, and then go ahead with building it:

``` Rust
// something along these lines
let mut app = App::new("program");
app.about(...).arg(...).arg(...);
```


---

_Comment by @kbknapp on 2015-04-30 12:50_

Yep that's the exact issue, because subcommands are really just apps you gave to store them separately then add them to the app, so if you have a large subcommand tree it becomes far less ergonomic. 

I've pushed patch-86 if you want to take a look.


---

_Comment by @Byron on 2015-04-30 14:38_

Thank you, I will have a look tomorrow, as I still think this should work, somehow. Hope dies last.


---

_Comment by @Byron on 2015-05-02 08:25_

I think this will work, but only so if one is willing to drastically change the 'nestability' of clap entirely.

For example, invocations like this ...

``` Rust
let matches = App::new("MyApp")
                .version("1.0")
                .author("Kevin K. <kbknapp@gmail.com>")
                .about("Does awesome things")
                .args_from_usage("-c --config=[conf] 'Sets a custom config file'
                                 [output] 'Sets an optional output file'
                                 [debug]... -d 'Turn debugging information on'")
                .subcommand(SubCommand::new("test")
                                        .about("does testing things")
                                        .arg_from_usage("[list] -l 'lists test values'"))
                .get_matches();
```

... would now look like this to work:

``` Rust
let mut app = App::new("MyApp");
app.version("1.0")
   .author("Kevin K. <kbknapp@gmail.com>")
   .about("Does awesome things")
   .args_from_usage("-c --config=[conf] 'Sets a custom config file'
                       [output] 'Sets an optional output file'
                       [debug]... -d 'Turn debugging information on'");

let mut sc = SubCommand::new("test");
sc.about("does testing things")
  .arg_from_usage("[list] -l 'lists test values'");
app.subcommand(sc);

let matches = app.get_matches();
```

I would argue that that the second form is actually more readable, and is an effective means of preventing stack-related issues. After all, one would re-use the `let mut sc` binding for all subcommands, which should lead to just one additional instance of an `App` to be held on the stack at all times.

A strong indicator against doing that would be that most people are used to the _feature complete_ builder pattern, and would thus be confused if they can't nest at first. Also the stack-size should be great enough for 99% of the programs out there, and hurting usability for the majority of people to benefit 1% seems wrong.

Considering workarounds ([via Thread](http://stackoverflow.com/questions/29937697/building-a-large-app-with-clap-causes-thread-main-has-overflowed-its-st); [data-driven builder](https://github.com/Byron/google-apis-rs/blob/d2a4e2ff8b16cb848869cc07b6c5a9107fb0a929/gen/youtube3-cli/src/main.rs#L9996)) for the remaining 1% are well-defined and easy to implement, I'd also not pursue this issue further.


---

_Comment by @kbknapp on 2015-05-03 05:09_

Yeah, because I think for the 99%, the nest-ability is easier and working I'm going to leave it as is for now. I'm going to continue looking into this though.

I'm beginning to wonder if it's more to two with highly nested subcommands, than the builder pattern itself. Reason I'm thinking that is I've done some naive testing with several hundred "builder settings" with no stack overflows. So I'm also curious where the line is. I have sit some SO issues with poor recursive calls though with relatively little in the current stack frame (compared to several instances of a `App` struct). I could be totally wrong though :)

The one method I've considered adding, and perhaps I will if I get some extra time this week, is to have a separate `App` struct which uses the borrowed builder pattern. This would allow usage similar to your example, but instead of importing `clap::App`, it'd probably be something like `clap::bb::App` There's still some portions I'd have to figure out, or move around, but if I do that, I'll let you know here so you can test it out and see if it happens to fix the SO issues for you.


---

_Comment by @Byron on 2015-05-04 13:22_

I see you want to achieve the best possible quality, and I'd be the last to not appreciate that, but: Does it make sense to invest time into something the fewest of us would encounter, or could easily workaround ? Maybe adding a section to the docs explaining the problem and showing solutions to it could already help.
Something I would be afraid of is the added complexity that comes with solving this 1% issue, which at least has potential to do more harm then good in the long run.
If I were you, I'd probably want to go ahead and implement interesting features, like maybe 'did you mean' for subcommands and flags. Actually I wanted to have a PR ready for that yesterday, but didn't even start working on it as my body decided to be affected by some virus or so :(. Of course I am still not fine, but try to get back into the workflow.


---

_Referenced in [habitat-sh/habitat#2848](../../habitat-sh/habitat/pulls/2848.md) on 2017-07-28 22:29_

---

_Referenced in [TeXitoi/structopt#203](../../TeXitoi/structopt/issues/203.md) on 2019-06-12 23:32_

---
