---
number: 568
title: "completions: Support adding additional code to complete values"
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-completion
  - S-wont-fix
assignees: []
created_at: 2016-07-04T12:48:21Z
updated_at: 2022-01-11T18:27:03Z
url: https://github.com/clap-rs/clap/issues/568
synced_at: 2026-01-10T01:26:32Z
---

# completions: Support adding additional code to complete values

---

_Issue opened by @joshtriplett on 2016-07-04 12:48_

[Closing comment](https://github.com/clap-rs/clap/issues/568#issuecomment-992733258):
> We currently provide [ValueHint](https://docs.rs/clap/3.0.0-rc.4/clap/enum.ValueHint.html) for leveraging shell logic, though it isn't customizable. For that, we have #1232.
> 
> I'm inclined to close this in favor of wrapping up this request with #1232. I'm hesitant for us to have two different solutions (a Rust driven and shell-specific snippets) and doing shell snippets would couple args to completion so they would know what shell they are completing for.
> 
> We can always expand how we are using `ValueHint` for the common cases and as a short term until we have full customization.
---
For some argument values, the bash-completions may want to include additional logic for what type of value to complete, rather than allowing arbitrary strings.  For instance, an option might accept a path to a file that exists; bash has a mechanism for that.  Or, an option might accept the name of a git ref that exists; that's something more easily implemented in the program in Rust.  Either way, it makes sense to augment clap's completion logic.

This also argues for implementing the completions by calling the program at runtime, rather than via shell; that way, arbitrary logic in Rust can determine what to complete, rather than providing a template system to feed in shell code (ick).


---

_Comment by @kbknapp on 2016-07-04 21:35_

Perhaps adding something like was discussed in in #376 where there is a "completer" function? I'm all for this, but figured it's also addable in a backwards compatible way once I had the base implementation complete.

I'm just not sure which would be the _best_ way to add this so I'm open to all ideas.


---

_Label `T: enhancement` added by @kbknapp on 2016-07-04 21:35_

---

_Label `P4: nice to have` added by @kbknapp on 2016-07-04 21:35_

---

_Label `D: intermediate` added by @kbknapp on 2016-07-04 21:35_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-04 21:35_

---

_Label `C: completion gen` added by @kbknapp on 2016-07-04 21:35_

---

_Comment by @joshtriplett on 2016-07-04 22:38_

I'm honestly not sure either.  I'm really hesitant to suggest inlining shell script snippets into Rust code as strings; I'd rather see those written in a separate shell file and included from there (not least of which to get the right filetype and highlighting).  bash (via `compgen`) and bash-completion (via functions in `/usr/share/bash-completion/bash_completion`) have some built-in helpers, and it'd be nice to support those for common cases like hostnames, users, and files (with glob patterns).  Someone might also want to write arbitrary shell code to enumerate argument values.  It'd also be nice to support using arbitrary Rust code by invoking the program.

I think what I'd suggest is that the `.completer` function should take an enum argument, where that enum has values like `User`, `File`, `FileGlob("*.ext")`, `BashFunc("__comp_function_name")`, and `RustFunc(...)`.  Those would then translate into appropriate calls to compgen, calls to the specified function, or invocations of the program to run Rust code.  (That last one would also require something like a `global_setting` to enable a `--clap-complete` option or similar.)

This is turning out to be a remarkably hairy yak.


---

_Referenced in [git-series/git-series#20](../../git-series/git-series/issues/20.md) on 2016-09-08 21:06_

---

_Referenced in [git-series/git-series#26](../../git-series/git-series/pulls/26.md) on 2016-09-29 22:42_

---

_Comment by @mathstuf on 2016-10-02 12:15_

In zsh at least, clap could generate completion function calls such as:

``` zsh
(( $+functions[_appname_clap_complete_ARG] )) || _appname_clap_complete_ARG () {
}
```

Which can then be overridden in a supplemental file included before this one (via `source` if the file exists). Bash probably has some mechanism that works similarly.


---

_Comment by @emk on 2016-10-10 15:31_

I've just converted [cage](https://github.com/faradayio/cage) to use clap, and I'm very happy with the results. Basic completion works under both bash and fish. Great code!

But cage would benefit enormously from being able to dynamically complete the names of docker services for commands like:

``` sh
cage test $SERVICE_NAME
```

If the project in the current directory has the ervices `foo` and `frontend/bar` I would like to be able to do the following:

``` sh
> cage test f<TAB>
foo
frontend/bar
```

I would be happy to add an extra argument to the app, something like:

``` sh
> cage --_complete-service f
foo
frontend/bar
```

And declare this as:

``` yaml
- SERVICE:
    value_name: "SERVICE"
    required: true
    complete_with: "_complete-service"
    help: "The name of the service in which to run a command"
```

Obviously the details could vary a bit, but we would ultimately have `--_complete-pod`, `--_complete-service`, `--_complete-pod-or-service` and `--_complete-repo-alias`, among others. Also note that many different subcommands would share each completion hook, which might mean we want these to be potentially global.


---

_Referenced in [clap-rs/clap#644](../../clap-rs/clap/issues/644.md) on 2016-10-10 19:13_

---

_Comment by @kbknapp on 2016-10-10 19:15_

@emk Your post has me thinking about this more and more, I'm thinking some sort of hybrid between what @joshtriplett listed above and what you're proposing.

My schedule is pretty busy this week, but I should be able to at least test some ideas and see the feasibility. Stay tuned to this thread for updates!


---

_Comment by @emk on 2016-10-10 23:43_

Another approach worth a quick glance might be optcomplete for Python:
http://furius.ca/optcomplete/ As far as I can tell, this uses one small,
universal shell script for each supported shell, and offloads all the
actual completion work to the application's own arg parsing machinery.

I think another Python library just uses a '--_complete' on the program
that does all the actual work, but I can't find it right now. I'll keep
Googling around when I have moment and post anything interesting I find.

Thank you so much for a great library and for looking into this!


---

_Comment by @emk on 2016-10-11 10:23_

Ah, here we go. [Some docs](https://github.com/dbarnett/python-selfcompletion) on how several Python arg parsing libraries handle `--_completion`.

> selfcompletion is a layer on top of argparse to take the fine-grained model
> argparse builds of the arguments your program accepts and automatically
> generate an extra '--_completion' argument that generates all possible
> completions given a partial command line as a string.
> 
> The '--_completion' argument in turn is used by a generic bash programmable
> completion script that tries '--_completion' on any program that doesn't have
> its own completion already available, renders the output of the program's
> built-in completion if available, and otherwise silently falls back to the
> shell default.

Here is the [generic completion function for bash](https://github.com/dbarnett/python-selfcompletion/blob/master/bash_completion.d/_selfcompletion):

``` sh
_foo()
{
    prog="$1"
    while read glob_str; do
        case $prog in
        $glob_str)
            return 1;;
        esac
    done < <( echo "$SELFCOMPLETION_EXCLUSIONS" )
    which "$prog" >/dev/null || return 1
    _COMP_OUTPUTSTR="$( $prog --_completion "${COMP_WORDS[*]}" 2>/dev/null )"
    if test $? -ne 0; then
        return 1
    fi
    readarray -t COMPREPLY < <( echo -n "$_COMP_OUTPUTSTR" )
}

complete -o default -o nospace -D -F _foo
```

The advantage of this approach is that the per-shell code can be written only once, and all the hard work can be done directly by the application itself. Obviously, there might be disadvantages as well. But I figured it was worth tracking down all the existing attempts to standardize this to see if any of them had helpful ideas. :-)


---

_Comment by @joshtriplett on 2016-10-24 21:40_

@kbknapp Any updates on this mechanism? I have someone asking after completions, and I'd love to beta-test this.


---

_Comment by @jcreekmore on 2016-10-25 17:35_

@kbknapp I would be interested in this as well. I am currently post-processing my completions to substitute in `_filedir` for filename completion, but that is less than ideal.


---

_Comment by @kbknapp on 2016-10-25 17:53_

@joshtriplett @jcreekmore 

Now that the ZSH implementation is complete I've got a better handle on this. The biggest issue I see holding this up is that completions are done differently between all three (so far) supported shells.

I'm all for some sort of enum with variants that allow things like, `Files`, `Directories`, `Globs(&str)`, `Code(&str)` or something to that effect. But some shells support those things verbatim, others only in arbitrary ways that clap doesn't use when gen'ing the completion code.

I'm just unsure of the best way to expose this. Perhaps on an `Arg::complete_with(enum)`?

I guess, first what is the shell you're trying to support, and what particular portions are you wanting to inject into the completion code?


---

_Comment by @emk on 2016-10-25 18:00_

@kbknapp For `cage`, we'd like to be able to complete custom "types" of values, such as Docker container names, "pod" names, target environments, and so on. The legal values can only be determined by asking our executable at runtime, since they vary from project to project.

This is pretty much how `git-completion` handles origin names, branch names, etc.

The problem with an `enum` is that it would limit us to just a few built-in types such as `Files`, `Directories`, etc., right?


---

_Comment by @joshtriplett on 2016-10-25 19:23_

@kbknapp I don't think you need to support embedding arbitrary shell code from Rust.  My suggestion would be to support the lowest-common-denominator bits (filenames, usernames, filenames matching one of these patterns, etc), and then have a "call this Rust function" variant that invokes the program itself with some internal hidden --clap-complete option that dispatches to that Rust function.  That makes it easy to do things like "a git ref matching this pattern", by calling a Rust function implementing that.

For those common categories like filenames or usernames, use the shell built-in mechanisms if available, or provide and use code implementing those simple completions if the shell doesn't have them.

If people want "invoke this shell code", I'd suggest adding a Rust variant to call a named shell function, and then letting people add that shell function to the resulting generated completions for any shell they support.  That seems preferable to embedding many variants of shell code directly.

``` rust
enum Completion<F: Fn(...) -> ...> {
    File,
    User,
    Hostname,
    Strings(&[str]),
    Globs(&[str]),
    ShellFunction(&str), // maybe
    RustFunction(F),
}
```


---

_Comment by @kbknapp on 2016-10-25 19:36_

@emk Yes, and no. It would be extensible, so more variants could be added. But Some of the variants could also take additional parameters, and ultimately (possibly) injecting arbitrary shell script via something like ,`Code(&str)` which of course isn't _super_ great, but perhaps a fallback if a particular variant doesn't quite fit the bill. (Or perhaps not...it could end up being massively unsafe :stuck_out_tongue_winking_eye: )

At the same time, I haven't looked into _exactly_ where this code would be injected and ultimately if it's even feasible yet. This is just straight of the top of my head right now.

Also, if the "types" are know prior to runtime ZSH already supports this just by using the `Arg::possible_values`


---

_Comment by @joshtriplett on 2016-10-25 19:54_

That's true, "one of these fixed strings" should be an option as well.  Updating the type in my previous comment.


---

_Referenced in [rust-lang/rustup#278](../../rust-lang/rustup/issues/278.md) on 2016-10-26 18:07_

---

_Referenced in [clap-rs/clap#816](../../clap-rs/clap/issues/816.md) on 2017-01-14 02:25_

---

_Comment by @kbknapp on 2017-01-14 02:25_

From @Xion in #816 

> In Python, there is a package called [argcomplete](https://pypi.python.org/pypi/argcomplete) which provides very flexible autocompletion for apps that use the standard argparse module. What it allows is to implement a custom completion provider: essentially a piece of your own code that's executed when the the binary is invoked in a Special Way (tm) by the shell-specific completion script.
> For an example, see [here](https://github.com/Xion/gisht.py/blob/master/gisht/args/autocomplete.py#L26). The code is preparing completions dynamically from the filesystem, or even from a remote API (if certain flag isn't passed (flags are partially parsed at this point)).
> Having something like this in clap would be very nice. I know this is a potentially complex subsystem so it'd be unreasonable to expect it implemented anytime, but I wanted to at least put this feature on the radar.

---

_Comment by @kbknapp on 2017-01-14 02:48_

After reading through some of the `argcomplete` python module the hardest part will be figuring out how to call a Rust function from the shell.

---

_Comment by @kbknapp on 2017-01-14 02:52_

I'm guessing what'll end up happening is some sort of double run with hidden args.

---

_Referenced in [clap-rs/clap#818](../../clap-rs/clap/issues/818.md) on 2017-01-19 01:29_

---

_Comment by @kbknapp on 2017-01-19 01:30_

Expanding on the ideas (from #818)

> The problem with implementing this is I just haven't had a good time to sit down and think about how (because of work, holidays, family, etc.). I want a way to specify this that abstracts well enough to work for all shells. The easiest way is to say, "Put your arbitrary completion shell script here inside this `Arg::complete_with(&str)`" but that feels super hacky to me, and potentially unsafe. What I'd like to do is provide a `Arg::complete_with(Fn(&str, &str)->String)` (and a `Arg::complete_with_os(Fn(&str,&OsStr)->OsString)` where an arbitrary Rust function is called...but herein lies the problem; shell completions are run before the program executes. This has led to some people using hidden args or something like, `$ prog --complete me<tab>` calling a shell completer that actually runs `$ prog --_complete_arg "complete" --_complete_prefix "me"` which generates the possible completions and returns them to the shell. I'm not against doing that, but again feels strange because you're injecting hidden args into a CLI. Although typing this out right now does make me lean towards this solution.

---

_Comment by @joshtriplett on 2017-01-19 06:21_

@kbknapp I like the idea of using parameters like `--_clap-complete` and similar, and then calling the program itself to do the completion via Rust code.  That would make it possible to (for instance) use git2-rs from Rust to complete names of things in a git repository.

Naming the argument/parameter seems sufficient to dispatch to the right function, though I'd also like to have the other arguments available to handle things like `prog --repository /path/to/.git --branch someth[tab]` (which needs the repository to complete branches from).

Also, those `complete_with` functions should return either a `Vec<OsString>` or in general an implementation of `Iterator<OsString>`, to return all completions.  (The latter would benefit from but not require `-> impl Trait` support, since `complete_with` can declare the return value as a generic while the actual function/closure might use a specific iterator type.)

And what does the first `&str` parameter of those functions refer to?

---

_Comment by @mathstuf on 2017-01-19 14:29_

I'd just like to note that for really expensive queries (imagine tab-completion for `cargo install <Tab>`), at least `zsh` supports a cache for completions; it would be nice to have a way to leverage that cache via some "can be cached" flag (cache expiry is controlled via `zstyle`).

---

_Comment by @joshtriplett on 2017-01-19 20:05_

> I'd just like to note that for really expensive queries (imagine tab-completion for `cargo install <Tab>`)

I'd expect tab-completion for `cargo install <Tab>` to list all the currently-available crates, without doing a network update first.  That would use entirely local information, and generate completions quickly.

Rather than attempting to integrate with any particular shell's completion caching, perhaps if an app considers its completions expensive to generate, it could cache the necessary information to make them fast?  Caching policies seem much easier to implement in Rust, since they could use arbitrary freshness metrics ("the underlying data hasn't changed so return the cached list").

---

_Comment by @kbknapp on 2017-01-19 21:31_

@mathstuf @joshtriplett yeah, I'd prefer not to incorporate shell specific arguments if at all possible and leave that to the Rust function of the implementer.

> And what does the first `&str` parameter of those functions refer to?

It referred to the current arg being parsed (as determined by `clap`), the second was the prefix being completed (if any). Whether it's a `Vec<String>` or `String` (including `\n`s) (or `OsString` equivalents) returned will be determined once I'm able look at all four shells and see what they're expecting.

> I'd also like to have the other arguments available

I thought about this as well, and I'm torn between providing a simple list of strings (at which point why include them at all because people could just use `std::env::args`), or some magic about "allowing failed/incomplete parses" to give off a `ArgMatches` struct which I'd assume is actually the information you'd want but is more complex to implement.

---

_Comment by @joshtriplett on 2017-01-19 22:57_

@kbknapp Parsing partial arguments (without enforcing all of the argument requirements) seems like a pain, but I'd rather not manually implement argument parsing in order to do completion.  That said, implementing this without support for parsing other arguments at first would still help in many cases.

Does the "current arg being parsed" exist to allow passing the same completion function for multiple arguments, and distinguishing them via argument?  If so, why not just do that using a closure?  You could pass `|arg| complete("foo", arg)` easily enough.  Completing multiple arguments with the same function but needing to distinguish between them by name seems sufficiently uncommon to not want to complicate the common case.

---

_Comment by @kbknapp on 2017-01-21 22:18_

>  Parsing partial arguments (without enforcing all of the argument requirements) seems like a pain

Actually I don't think it will be. `clap` enforces all the requirements lazily, so a single branch would allow a failing parse to "pass" and only in that very strict circumstance. Of course this would be it'd have to be well documented that when using the `ArgMatches` given to the completing function, those requirements haven't been enforced yet and can't be relied on. But it would at least allow one to check for the presence of args, values, etc.

> If so, why not just do that using a closure?

Actually I like that idea, I'll have to try this out when I get some time to sit down and try implementing this!

---

_Referenced in [clap-rs/clap#868](../../clap-rs/clap/issues/868.md) on 2017-02-22 03:41_

---

_Referenced in [BurntSushi/ripgrep#375](../../BurntSushi/ripgrep/issues/375.md) on 2017-02-22 04:03_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:56_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:56_

---

_Referenced in [clap-rs/clap#986](../../clap-rs/clap/issues/986.md) on 2017-06-16 15:33_

---

_Comment by @droundy on 2017-08-11 12:50_

I would suggest rather than an enum that multiple methods setting completion possibilities would be a better and more extensible API.  So rather than

    Args::complete_with(enum)

you would have

    Args::complete_with_files()

and a whole set of other `complete_with_XXX` methods.  This removes the requirement that you foresee every possibility on the first version of the API (since you can't add variants to a public enum without breaking backwards compatibility).  I think the most important one is the one that uses an auto-generated flag to call rust code, since this can then implement all the others in a shell-independent way.  Then optimizations could be made to do the others in a shell-dependent way if that seems to help.  So I would focus on something like:

    Args::complete_with_function(|matches_so_far| -> [String] { ... }) 

where it is important to provide the completed flags, since that can affect what is a valid completion for a given flag.

---

_Comment by @droundy on 2017-08-11 12:59_

Here is an example of a bash completion that simply calls command-line flags that return the possible completions for [darcs](https://github.com/kbknapp/clap-rs/files/1217871/darcs.txt).  darcs does no shell-specific munging in its Haskell code (since it is usable by multiple shells), just outputs a `'\n'`-delimited list of completions.  Then the bash code handles spaces and colons specially for bash.

---

_Comment by @kbknapp on 2017-11-06 03:25_

I'm copying some comments from @joshtriplett on gitter so the discussion is all in one place.

> I see general consensus that we should have a way to call an arbitrary Rust function through a flag like --_clap-complete. We haven't talked about what that should take, but I would suggest that it needs 1) the partial argument being completed, 2) the rest of the arguments minus --_clap-complete (up to it to pass them to clap if desired). Beyond that, since some shells have built-in support for completing specific things and would likely do so faster than launching a program, we should also support files, files matching a glob, list of fixed strings, and maybe usernames/hostnames. Any of those that a shell we're generating completions for doesn't support would be easy enough to support in Rust.

>As far as I can tell, the only items we don't have consensus on are 1) exactly what common-denominator of built-in completions do we support (e.g. usernames/hostnames?), and 2) how exactly do we support shell code. For the latter, personally I favor the "named shell function" approach, which has the advantage of being shell-independent, but I recognize that there are people who seem to want to embed full shell code. (I don't know how that could be made shell-independent, personally.)

----

Here's my responses:

> I see general consensus that we should have a way to call an arbitrary Rust function through a flag like --_clap-complete. We haven't talked about what that should take, but I would suggest that it needs 1) the partial argument being completed, 2) the rest of the arguments minus --_clap-complete (up to it to pass them to clap if desired).

Agreed. However, I'm willing to "settle" for simply passing in argument to complete sans the argv, since the Rust code could essentially see the same thing by querying `std::env::arg[_os]` just like clap does. Another option is to allow "incomplete parsing" whenever `--_clap-complete` is used which would allow sending an `ArgMatches` struct to the Rust completion code. It would result in a double-parse to be sure, but I can't imagine that'd be a performance issue for anyone except the most critical perhaps "daemon mode" CLIs which I think is at odds with using completions in the first place 😜 

> Beyond that, since some shells have built-in support for completing specific things and would likely do so faster than launching a program, we should also support files, files matching a glob, list of fixed strings, and maybe usernames/hostnames. Any of those that a shell we're generating completions for doesn't support would be easy enough to support in Rust.

Also agreed. I'm thinking I'd like to expose this as a sort of enum where the implementation allows checking the shell which we're outputting for and either using the shell builtins or augmenting the missing parts with our own code.

> 1) exactly what common-denominator of built-in completions do we support (e.g. usernames/hostnames?)

Correct. I don't think this needs to be fully hashed out though, as we can always add. For now, files is a good starting point. List of predetermined values is possible-ish today (in some shells, but we'd need to augment in the lacking shells) via `possible_values`, however that would probably be another easy win for us to support a list of predefined values determined *at completion time*. For users/hostnames I'm fine adding it, however I don't have strong feelings about if it should be included as a first implementation or added later.

> 2) how exactly do we support shell code. For the latter, personally I favor the "named shell function" approach, which has the advantage of being shell-independent, but I recognize that there are people who seem to want to embed full shell code. (I don't know how that could be made shell-independent, personally.)

The ways I see are allowing the user to send shell specific code up front a la `BashShellCode("blah blah blah"), FishShellCode("bam bam bam")` etc, or perhaps with a Rust function which we send a `clap::Shell` variant to and they are responsible for sending back valid code such as `ShellCode(Fn(clap::Shell)->String)`.

-------

More generally, my goal is to pull the completion script generation *out* of clap proper and into a `clap_completions` crate at 3.x. I had made quite a few changes on the 3.x branch which would make implementing all this far easier and more correct, but it looks like I'm going to have to *partially* scrap the current 3.x branch due to some recent changes, and ideas I'd rather incorporate moving forward.

What that means is I don't want to get too into the weeds *implementing* ideas we reach here on the current 2.x branch only to have massive changes in 3.x. I *do* want to reach a consensus though, or agree upon a foundation API which could be implemented on the 3.x branch.

Having said that, if someone puts time into actual implementation on the 2.x branch I'd be more than happy to include it. I just don't personally have the time to pour into separate 2.x and 3.x implementations.

---

_Referenced in [sharkdp/fd#189](../../sharkdp/fd/issues/189.md) on 2018-01-01 11:42_

---

_Comment by @kpcyrd on 2018-01-03 04:02_

I think it's important that the function that is called by `.get_matches()` needs to act like a 2nd main. There are some usecases where initialization is needed even in this case, for example to make sure the sandbox is active even in those cases. I don't think this is going to be an issue, just my 2 cents. :)

I'm about to add completion to two of my programs, mostly because the values that the user is usually trying to complete are hard to type and would require looking them up manually.

I think a minimal invasive 2.0 compatible solution that would already work for most of us would be something along the lines of:
```rust
.arg(Arg::with_name("foo")
    .complete_with_cmd(&["myprog", "internal", "something"])
)
```
This would instruct the shell to execute `myprog internal something abc` when the user types `abc<TAB>` for that argument. The output would be a `\n`-delimited list as @droundy suggested.

For now, this would require writing additional subcommands, but that code can be re-used for a more advanced solution that requires breaking changes to clap.

If this is acceptable I would try to prepare a patch, completion is crucial for one of those programs and I would either have to maintain the tab completion code on my own or fallback to post-processing as well.
  

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:29_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:29_

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2018-03-30 01:22_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-03-30 01:23_

---

_Referenced in [clap-rs/clap#1269](../../clap-rs/clap/issues/1269.md) on 2018-06-05 01:22_

---

_Comment by @softprops on 2018-06-05 03:20_

Here's some inspiration from how golang kingpin package handles dynamic tab completion using the bash [COMPREPLY bash protocol](https://www.gnu.org/software/bash/manual/html_node/A-Programmable-Completion-Example.html)

Kingpin supports dynamic completions via it's [HintAction interface](https://github.com/alecthomas/kingpin/blob/master/README.md#additional-api)

I imagine this should be possible providing a fn interface to clap's Arg type for dynamic completions

Under the covers kingpin forks program control when in [completion mode](https://github.com/alecthomas/kingpin/blob/master/app.go#L205) to invoke that completion func then exits the process.

It switches modes of operation based on a [flag](https://github.com/alecthomas/kingpin/blob/master/app.go#L66) that the completion script [passes](https://github.com/alecthomas/kingpin/blob/master/templates.go#L240).









---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 00:50_

---

_Referenced in [matthias-t/workspace#17](../../matthias-t/workspace/issues/17.md) on 2018-07-31 13:23_

---

_Comment by @adamtulinius on 2018-09-06 09:19_

> Here's some inspiration from how golang kingpin package handles [..]

Please not that kingpin currently can't complete file paths, which need something like https://github.com/alecthomas/kingpin/compare/master...DBCDK:hint-files to fix. I'm just mentioning this, because it required work on the compreply stuff, and might be useful here as well.

---

_Referenced in [sharkdp/bat#372](../../sharkdp/bat/issues/372.md) on 2018-10-21 20:29_

---

_Comment by @NotBad4U on 2018-11-21 11:51_

Hello everyone :smile: 
Any plans to progress on this feature? I could give it a try, but I would need a bit of mentoring / pointing to the right places.

---

_Comment by @joshtriplett on 2018-11-21 19:46_

@NotBad4U 
@kbknapp is interested in seeing that as clap gets closer to 3.0, and we're likely to brainstorm how it'll work in the next few months. We'll ping this bug when doing so.

---

_Comment by @IssueHuntBot on 2018-12-03 08:21_

@issuehuntfest has funded $50.00 to this issue. [See it on IssueHunt](https://issuehunt.io/repos/31315121/issues/568)

---

_Comment by @Dylan-DPC-zz on 2019-03-23 23:54_

@NotBad4U are you still interested in working on this issue? If yes, then we are happy to mentor you

---

_Comment by @NotBad4U on 2019-03-26 16:26_

yes :smile:  I'm still interested @Dylan-DPC 

---

_Comment by @IssueHuntBot on 2019-03-27 09:35_

@cben has funded $10.00 to this issue.

---
- Submit pull request via [IssueHunt](https://issuehunt.io/repos/31315121/issues/568) to receive this reward.
- Want to contribute? Chip in to this issue via [IssueHunt](https://issuehunt.io/repos/31315121/issues/568).
- Checkout the [IssueHunt Issue Explorer](https://issuehunt.io/issues) to see more funded issues.
- Need help from developers? [Add your repository](https://issuehunt.io/users/273688/repositories) on IssueHunt to raise funds.

---

_Comment by @Dylan-DPC-zz on 2019-03-27 10:27_

@NotBad4U you can start working on it. if you need any help you can ask us in #wg-cli channel on discord. 

---

_Comment by @zx8 on 2019-06-10 16:53_

https://github.com/posener/complete is a library dedicated to dynamic completion written in Go, if you're looking for some ideas/inspiration. Despite the repo's description, it supports bash, zsh & fish.

---

_Removed from milestone `3.0` by @pksunkara on 2020-02-14 02:46_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-14 02:46_

---

_Removed from milestone `3.1` by @pksunkara on 2020-03-03 12:42_

---

_Comment by @benjumanji on 2020-04-04 11:20_

Here is a proposal that I think would be relatively simple. I haven't checked how zsh completion is done, but it's my understanding that it understands bash so you could just re-use it?

The generated scripts for both fish and bash _already_ support dynamic completions. Both compgen for bash and complete for fish honor parameter expansion, including shell substitution and word split. That is to say if you throw `$(cat words)` for bash or `(cat words)` for `possible_value` into an arg it totally works! Right up until clap validation kicks in and says it's not a valid value.

Proposal: add `dynamic` to arg which adds the shell code and disables value validation. I can't justify working on this _right_ now, but if this is an acceptable proposal I might be able to circle back around to it in a few weeks when I might need it.

---

_Comment by @8573 on 2020-04-04 16:14_

> I haven't checked how zsh completion is done, but it's my understanding that it understands bash so you could just re-use it?

I've forgotten almost all I once knew about the Zsh completion system, but I recall that it's much more powerful/expressive(/fancy) than Bash's such that, if I recall correctly, telling it to use Bash completion, while the easy way out, could provide a needlessly suboptimal user experience. That said, I would think that such a project could start with telling Zsh to use Bash completion and come back and write native Zsh completion later.

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 06:46_

---

_Comment by @pksunkara on 2020-04-09 06:51_

#1793 is an attempt at fixing this in zsh.

---

_Referenced in [vn971/rua#121](../../vn971/rua/issues/121.md) on 2020-04-27 06:43_

---

_Referenced in [clap-rs/clap#1880](../../clap-rs/clap/issues/1880.md) on 2020-04-29 15:38_

---

_Referenced in [clap-rs/clap#1910](../../clap-rs/clap/issues/1910.md) on 2020-05-06 12:31_

---

_Referenced in [lsd-rs/lsd#385](../../lsd-rs/lsd/issues/385.md) on 2021-02-05 05:15_

---

_Referenced in [pimalaya/himalaya#101](../../pimalaya/himalaya/issues/101.md) on 2021-05-03 22:35_

---

_Referenced in [denoland/deno#10166](../../denoland/deno/issues/10166.md) on 2021-06-08 16:19_

---

_Comment by @epage on 2021-07-22 14:37_

Are #1232 and this now dupes?  Should we close one in favor of the other to make it easier to browse the backlog?

---

_Removed from milestone `3.0` by @pksunkara on 2021-07-26 22:15_

---

_Comment by @pksunkara on 2021-07-26 22:16_

I kept them separate because this issue is more concentrated on leveraging shell completions while the other is for a full fledged solution.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [tranzystorekk/pakr#1](../../tranzystorekk/pakr/issues/1.md) on 2021-08-23 14:13_

---

_Comment by @Milo123459 on 2021-09-13 21:04_

I'm looking to try and implement this. Is there an API in specific that should be used?

---

_Referenced in [epage/clapng#65](../../epage/clapng/issues/65.md) on 2021-12-06 16:28_

---

_Referenced in [epage/clapng#157](../../epage/clapng/issues/157.md) on 2021-12-06 20:15_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 16:55_

---

_Label `D: medium` removed by @epage on 2021-12-09 16:55_

---

_Label `S-blocked` added by @epage on 2021-12-09 16:55_

---

_Comment by @epage on 2021-12-13 18:04_

We currently provide [ValueHint](https://docs.rs/clap/3.0.0-rc.4/clap/enum.ValueHint.html) for leveraging shell logic, though it isn't customizable.  For that, we have #1232.

I'm inclined to close this in favor of wrapping up this request with #1232.  I'm hesitant for us to have two different solutions (a Rust driven and shell-specific snippets) and doing shell snippets would couple args to completion so they would know what shell they are completing for.

We can always expand how we are using `ValueHint` for the common cases and as a short term until we have full customization.

If there is any concern with closing this, please let us know!

---

_Closed by @epage on 2021-12-13 18:04_

---

_Label `S-blocked` removed by @epage on 2022-01-11 18:27_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:27_

---
