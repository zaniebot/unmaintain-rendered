---
number: 748
title: "Allow adding \"external argv\"s to be parsed alongside/before command line"
type: issue
state: open
author: casey
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2016-11-13T23:56:57Z
updated_at: 2025-03-16T22:20:10Z
url: https://github.com/clap-rs/clap/issues/748
synced_at: 2026-01-10T01:26:35Z
---

# Allow adding "external argv"s to be parsed alongside/before command line

---

_Issue opened by @casey on 2016-11-13 23:56_

Maintainer's notes
- This is related to https://github.com/clap-rs/clap/issues/3113, https://github.com/clap-rs/clap/discussions/2763
---

I wrote up a sketch of how this might look here:

https://github.com/casey/clap-config

Basically, it would be pretty cool to generate a simple config file parser from a clap argument parser. There's a longer description of some use cases in the readme, but it would be great for projects which are configurable with both the command line and a config file, and for helping projects add config file configuration in addition to command line configuration.

---

_Comment by @kbknapp on 2016-11-14 21:30_

This relates to #251 

I'm not against this at all, but I don't have the bandwidth to implement it just yet. I'll keep it on the issue tracker. I'd say supporting YAML and TOML would be fine.

The part I'd still need fleshed out is whether a user can use _both_ the command line and this config file as well? Does one override the other? What about arguments that accept multiple values, and are passed both in the command line and config files, do they get overridden or appended to?


---

_Label `C: parsing` added by @kbknapp on 2016-11-14 21:30_

---

_Label `D: intermediate` added by @kbknapp on 2016-11-14 21:30_

---

_Label `E: optional dep` added by @kbknapp on 2016-11-14 21:30_

---

_Label `P4: nice to have` added by @kbknapp on 2016-11-14 21:30_

---

_Label `T: new feature` added by @kbknapp on 2016-11-14 21:30_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-14 21:30_

---

_Comment by @casey on 2016-11-15 05:19_

I think that a `MergeType` enum would work, something like:

``` rust
enum MergeType {
  Override, // replace values with those from config file
  Defaults, // use values from config file only where not given on command line
  Conflict. // complain if any value is provided in both places
}

...

matches.merge_from_config(config, MergeType::Override);
```

For arguments that accept multiple values, I think that until someone has a specific use case for merging values from both places, it's probably best to only allow overriding or ignoring.


---

_Referenced in [clap-rs/clap#734](../../clap-rs/clap/pulls/734.md) on 2016-11-15 22:08_

---

_Comment by @ssokolow on 2016-11-23 22:56_

This is actually something I'm going to have to deal with in a project soon. (In Python, I tend to dodge the issue by hard-coding my defaults at the top of the only (or top-level) `.py` file and telling users to just edit them there).

While I'm not familiar with clap internals, I don't see why it would be necessary to over-complicate things. Let's start by separating what clap needs out from a more generic "config file for my program" solution.


1. **Standard format:** So people can write frontends easily, use something like JSON or TOML. (Ideally, a lightweight parser, since clap already results in my experimental "smallest possible libstd-using Rust binaries" tests hitting a wall at 156KiB (static/musl) or 129KiB (dynamic/glibc) with nothing but clap and some stub argument definitions... and it'd be nice to include config file support.)
2. **String-oriented key-value store:** As I see it, the only use clap has for a configuration file is as an alternative representation of the command-line arguments, so...
    1. the only need to support data types beyond key="string" pairs is to allow things like key=1 as more intuitive alternatives to key="1". (Being able to have syntax highlighting for data types correspond to the data's actual/conceptual/intuitive type is also a nice thing to have when editing manually.)
    2. It's only necessary to support non-flat data structures as a comfortable shorthand for things clap has built-in support for producing from key=value pairs, like a list equivalent to `--append=this --append=that`.
    3. Positional arguments generally aren't explicitly supported in configuration files since the situations where giving them defaults are so uncommon and esoteric. (eg. Allowing "target_dir" to be optional in `fancy_cp <file> [...] [target_dir]` would be an example where it'd actually make sense but be esoteric.)
3. **Simple precedence:** I've never actually seen a situation where `MergeType` is worth the trouble. Generally, a much better solution is to just use simple linear precedence:
    1. Make parsing a command-line and parsing a file have the same API so you can call either first and then merge the other. (This also has the benefit that it makes it really easy to build precedence chains for config files like the "system-wide for vendor, system-wide for application, user-specific for vendor, user-specific for application" one that Qt's QSettings API implements.)
    1. The only situation I can see where `Override` wouldn't be frustrating and/or confusing is when a `--foo-from-file=` argument comes after a `--foo=` argument in the command-line... so just add "all from file" and "specific field from file" as argument types and call it a day. (ie. `--foo=A --foo=B_trumps_A --foo-from-file=contents_trump_B --foo=winner`.)
    2. The only situation where I've seen `Conflict` making sense is within the same source (ie. Complaining about specifying two values within the same config file *or on the same command line*.), so supporting it in an intuitive way is orthogonal to supporting config files. (The key being to understand what purpose `Conflict` actually serves. Forcing people to modify config files for a one-off run is *never* a good design (it encourages human error), so `Conflict` only really makes practical sense as a way to say that a single source is internally contradictory.)

With all of that said, what I'd suggest is that it's not really the config file parsing you need to focus on, but providing a clearly documented API that allows something like `(human_readable_source_identifier, parsed_data_structure)` to be fed into clap so we can feed in JSON/TOML/whatever at our leisure. (The human-readable source identifier would be used for error messages.)

That would also allow clap defaults to be embedded into larger JSON/TOML/etc. configuration files without complicating clap.

Heck, with a little thought, the "from command-line arguments" functions you already have could just be parsers on the same tier as JSON/TOML/etc. which just feed into the API I'm proposing.

---

_Comment by @BurntSushi on 2016-11-24 03:59_

> The only situation where I've seen Conflict making sense is within the same source (ie. Complaining about specifying two values within the same config file or on the same command line.), so supporting it in an intuitive way is orthogonal to supporting config files.

I'm not sure I really buy this. If I have a flag --foo that takes a value N, then it seems reasonable for me to want to assign a default value to it in a config file, but retain the right to want to override it on the actual command line. Depending on how the --foo flag was defined, this might not be allowed in the form of `--foo X --foo Y`, so a config file might need special handling?

---

_Comment by @ssokolow on 2016-11-24 05:36_

> I'm not sure I really buy this. If I have a flag --foo that takes a value N, then it seems reasonable for me to want to assign a default value to it in a config file, but retain the right to want to override it on the actual command line. Depending on how the --foo flag was defined, this might not be allowed in the form of --foo X --foo Y, so a config file might need special handling?

You misunderstand. I made only two claims and neither was that `Conflict` should be mandatory within a single source. Those two claims are:

1. `Conflict` never produces a worthwhile behaviour when applied to two pieces of data from different sources.
2. Regardless of how `MergeType` behaviour is handled, it is entirely internal to the parsing of each individual source.

What I'm arguing is that:

1. If we're doing fine without `MergeType` now, there's nothing about adding support for configuration files which adds it to the list of requirements.
2. If we need `MergeType` urgently and implement it before config file support, no beneficial functionality can be gained by expanding that "within a single source" implementation to also operate between multiple sources.

Hence, orthogonal.

**First,** let's discuss the "never produces worthwhile behaviour" point, starting with a few examples:

1. If `~/.user_default` can't override `/etc/global_default`, then that's bad.
2. If `--one-time-setting` can't override either of those, then that's bad.
3. If `~/.defaults_a` and `~/.defaults_b` don't have a defined precedence order, you're forcing users to manage pointless busywork in the best case and, in the worst case, you wind up with a "FAT table copies 1 and 2 disagree? Which one did you intend?" situation.

This derives from two issues:

1. If at all possible, you want to have a single, authoritative source for settings to limit the potential for human error.
2. Multiple sources of configuration are introduced specifically to take advantage of defined-precedence overriding to allow customizing in progressively narrower scopes. (eg. site, user, project, task)

Or, let's come at it from the other direction and address `Override`, `Defaults`, and `Conflict` as options.

`Defaults` needs no justification. When applied to reconcile data from two different sources, it implements the behaviour configuration files serve in every case I've ever encountered.

`Conflict` is the hardest to justify because:

* If you're the administrator, it just cripples the utility of the config file if you can't put anything into it which you might want to override on a task-by-task basis. (And then people end up implementing *another* layer of default handing using a wrapper which passes in `--foo=false` unless you call it with `--foo=true`.)
* If you're not the administrator, it means "You can't specify --foo=true because `/etc/config_defaults` specified `foo=false`".

The former case is counter-productive and the latter sounds like trying to kitchen-sink some kind of user permissions system into clap when that's a job for code which runs after clap is done.

Finally, `Override` is just `Defaults` with the ordering swapped, so it's only justifiable in the one case where you can't swap the order: The command-line arguments, which must always be the last in the chain.

The problem is that, by their very nature, command-line arguments are also the most suitable for one-off overrides, which means that there is no justifiable reason to use `Override` to silently ignore what the user specified on the command-line because some config file somewhere disagrees.

Therefore, it's much better to just make defined-precedence overriding an inherent, unavoidable feature of how conflicts between multiple sources are resolved. It's the de facto standard way to handle things and it's the only solution which addresses all of the concerns I brought up.

...it's also easy for people to conceptualize. For example, in Python (since it's pseudocode-like):
```python
matches = dict()
matches.update(parse_global_config())
matches.update(parse_local_config())
matches.update(parse_command_line_args())
```

**Therefore,** the concept of `MergeType` is only meaningful within the process of parsing a single source, because merging input from multiple sources should always `Defaults`.

We may indeed need to discuss the `MergeType` idea more, but the conclusion we come to has no bearing on a well-designed multi-source implementation.

1. Parse command-line input
2. Parse one or more config files (optionally with a source file specified in step 1 via `--config-file` or the like)
3. Merge the results using a simple "When in doubt, the last to speak wins" algorithm.

The only potential kink I see is to at least warn users that step 3 might reset `config_file` so it's not the actual value used if the config file itself contains a `config_file` value.

**The beauty of this approach** is that it's made of very small, very extensible pieces and you can get a lot of utility early on, then amend it later. Here are some example steps:

1. Split the `argv` parsing out from the rest so that anyone can implement the connecting interface on top of a JSON/TOML/etc. parser. (Now, even if they have to manually hook up the config parser and manually merge things, users don't have to reinvent clap's "apply the validation" code in between those two steps.)
2. Add a simple function for merging two parse results. (Now users don't have to manually merge entry-by-entry anymore.)
3. Anything else you think is necessary.

What I'm envisioning is that the `argv`-parsing side of the interface would take a reference it could use to query the schema-validation side to disambiguate things that would be inherent in the framing of some formats but not others. For example:

* Please resolve this key name (so `argv` can resolve command-line shorthands like `-s` without complaint while config files can print a deprecation message or exit with a failure message asking the user if they meant `sidechannel_data=...` in `config.cfg`)
* What type of value does this key take? (so parsers can perform any additional transformations needed, like `argv` deciding whether `--foo --bar` actually means `--foo=--bar`)
* Does this key accept a list of values? (so `argv` can use `--foo=this --foo=that --foo=those` while the internal representation it outputs can be closer to the `{"foo": ["this", "that", "those"]}` that JSON would use.)

---

_Referenced in [BurntSushi/ripgrep#196](../../BurntSushi/ripgrep/issues/196.md) on 2016-11-24 22:49_

---

_Comment by @BurntSushi on 2016-11-24 22:51_

@ssokolow Frankly, I'm having a really hard time following any of what you're saying. In particular, I find it hard to tie what you're saying to a concrete user experience. I'm probably missing some important context. In any case, I discussed some of the issues I personally see with config files: https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-262853109

---

_Comment by @ssokolow on 2016-11-25 00:41_

Ugh. That's the worst kind of response because it's a perfectly fair one, yet it's the hardest to formulate a meaningful answer to. Give me an hour or two to do "morning routine" stuff and I'll see what I can do to come at it from a different angle and simplify it so that, at the very least, maybe we can figure out where the disconnect is.

---

_Comment by @ssokolow on 2016-11-25 01:29_

OK. From a user-experience standpoint, the most intuitive solution is to have later arguments always override earlier ones and to treat the fallback chain from command-line to user defaults to global defaults the same way.

That gets you 99% of the way to supporting all behaviours I've seen in the wild.

For example:

1. `/etc/global_defaults` could set `foo=bar`
2. `~/.user_defaults` could override it with `foo=baz`
3. `alias mycmd="mycmd --foo=quux"` could override it with `foo=quux`
4. `mycmd --foo=glorp` could resolve to `mycmd --foo=quux --foo=glorp`

The end result clap should return is `foo=glorp` in that circumstance, rather than erroring out because some prior default the user had forgotten about is in conflict with what they want to do this one time.

(ie. They shouldn't have to type `command mycmd --foo=glorp` to explicitly bypass the shell `alias` or edit configuration files and then edit them back.)

What I'm arguing is that it's very simple to accomplish this as a collection of small, easily-composed pieces:

1. Make the `--foo=bar --foo=baz` resolution mentioned above for non-appending arguments into the default. (Having it error out is, at best, useful only in niche situations thanks to the `alias` shell built-in and wrapper scripts which don't carry around their own argument parsers.)
2. Split clap's current `get_matches` and friends into two pieces:
    1. Something which goes from a raw `argv` string to a structured internal representation. (With access to query the schema to disambiguate things like "Does `--foo` take a value?" and "Does `---bar` append, count, or override when specified multiple times?".)
    2. Something which applies the schema to that structured internal representation
3. Provide documentation for how to write JSON/TOML/etc. adapters which produce the same structured internal representation `argv` now produces.
4. Write a convenience function which takes a bunch of parsed outputs (what you currently get from `get_matches()`) and merges them in a "last in the list wins" manner.

Getting paths to things like XDG configuration directories is external to clap, because we don't want to preclude parsing config files stored elsewhere or tie clap to a specific implementation.

---

_Comment by @kbknapp on 2016-11-25 02:21_

I've read all the comments and have some thoughts on the matter but I'm on mobile right now so I'll try to write up my thoughts early to tomorrow. It boils down to I'd like command line to override config files or env vars, but those two extra sources to be added in a first come first serve manner. I'd also like to limit claps responsibility to parsing arguments from a *source*. Not determining source order or source validity.

I'll type up my full thoughts soon.

---

_Comment by @ssokolow on 2016-11-25 02:37_

> It boils down to I'd like command line to override config files or env vars, but those two extra sources to be added in a first come first serve manner. I'd also like to limit claps responsibility to parsing arguments from a source. Not determining source order or source validity.

I fully agree. Hence my idea limiting clap to:
1. Parse `argv` to structured representation (more akin to what JSON can represent)
2. Process structured representation according to schema
3. Provide a function which can be called to merge the output of multiple parsings according to application-specified precedence order. (As opposed to each application's developer manually reinventing the boilerplate which does this.)

It then becomes easy for the clap-using application or 3rd-party crates to implement things like:

1. Looking up the path to a config file using an XDG paths crate
2. Using the parsed output from the command-line args to determine the path for the config files. (Parse the command line, then parse any config file it specifies, then merge the results with the command line winning on conflict.)
3. Parsing a JSON/TOML/etc. config file into clap's structured representation.
4. Using a crate equivalent to Python's `shlex` to tokenize an environment variable's contents into what `get_matches_from` expects.
5. Deciding the precedence order to request of clap's merging function and when in the process to perform the merging.

As long as you provide examples of how to accomplish all of this with minimal boilerplate using 3rd-party crates, you can indefinitely defer the question of whether anything else belongs in clap itself.

> I'll type up my full thoughts soon.

I look forward to it.

---

_Comment by @kbknapp on 2016-11-26 21:58_

Ok so it took me a little longer to get to this than I'd planned, but here's where I stand on the issue.

First, I'm not terribly interested in clap handling the file I/O part of this, i.e. the reading files, handling permissions, errors that come from this, precedence, etc. I intend for this to all happen in the consumer code. The consumer would then pass in the deserialized config representation, for now I'm only imagining TOML/YAML, but others could be added.

The goal is ultimately to get clap the info it needs to do it's job, i.e. a normalized structure of an argv. In clap's case simply an ordered vector of strings (meaning anything from OsStr, String, etc.). I'm OK with giving clap either a `Toml` or `Yaml` object, and having clap normalize it down to just the ordered `Vec`.

The details would probably be something like defining a trait `Normalize` (aside, all names in this proposal are 🚲 🏠-able) that does the normalizing, and as a start implementing that trait on `Yaml`, `Toml`, `String`, and `OsString` (the latter two in order to support env vars). The `App` struct would then accept any number of "external argv" sources via something like `fn external_argv<T: Normalize>(argv: T)`, and save these in a first come first serve basis. I.e. the pseudocode below is the same to clap, and totally up to the consumer to decide (which allows us to side step all this precedence discussion and allow clap to stay more focused on a single purpose):

```rust
let some_env_var = env::var("SOME_ENV_VAR").ok();
let global_cfg = load_toml("/etc/myconfig.toml");
let user_cfg = load_toml("~/myconfig.toml");
let m = App::new("test")
    .external_argv(some_env_var)
    .external_argv(global_cfg)
    .external_argv(user_cfg)
    .get_matches();
```

If the consumer wants `global_cfg` to take precedence over `user_cfg` for whatever reason, they'd swap those two lines.

This makes parsing very simple because internally clap just parses them in reverse order, and if it reaches an arg that arleady exists in the matches, it just skips it. 

There are two outstanding issues that this would present though. One is if you have an arg that accepts multiple values, and has a value in some external argv, should a later value in either another external argv or via the explicit command line *add to* these values, or entirely override? The proposed system above entirely overrides, which I'm more of a fan of. If a consumer wants to provide a global default and allow users to add to those values, I'd be easiest to simply tell the user to include that default value in their own "overrides" or just re-add that value back after clap is done with it's parsing.

The second issue where this MergeType came into play. Since clap allows two types of conflicts, POSIX style overrides and hard conflicts, a MergeType would allow consumers to effectively convert from hard conflicts to POSIX style overrides *only* in the case of external argv conflicts. Basically it gives the choice of whether they want hard conflicts or overrides.

This situation, in my estimation, only happens in user defined configs. I.e. a user wants to specify a default that conflicts with other options, but yet may want to override that behaviour at some point. I can't imagine why I consumer would put a conflicting argument into a default config and actually want a hard conflict. Think of unix style aliases, `ls="ls -l"` yet due to POSIX style overrides, using values that conflict with `-l` is perfectly fine, so long as they come *afterwards*. However, as the developer/consumer it's sometimes difficult to decide what should be a hard conflict and what should be POSIX overridable because overrides sometimes seem confusing at runtime, whereas with a hard conflict, the user knows exactly what is going on and how to fix it.

I'm of the thought that *all* conflicts arising from external argvs should be treated as overridable, and it should just be documented well. I can't think of a concrete example where I'd actually *want* a hard conflict because I explicitly set something via the commandline.

This may sound like I'm in favor of a MergeType, but actually the more I think about it, the less I am. As I stated earlier, I'd prefer to treat all things as overridable, and disallow adding values at the commandline to values defined in the configs.

The only thing left to determine is how to represent free/positional arguments in these configs. Another 🚲 🏠 for sure, but options, and flags are simple. I'd suggest simply using a single "args" key and assigning the values in sequence such as `args="foo bar baz"`. 

Thoughts?

---

_Comment by @kbknapp on 2016-11-26 22:39_

I wasn't clear about the positional args part, we could equally as easy use the key to individualize them, but I kind of like that they're forced to be in order with only a single key to keep from any confusion by accidentally putting them in the wrong order

---

_Comment by @ssokolow on 2016-11-27 05:04_

We basically agree on the design aside from whether the merging should be declarative or procedural.

Your declarative approach is definitely nicer to look at, but it's puts more onus on clap to support edge-case features (or constrain users by refusing to), as I'll answer in reply to one of your outstanding issues...

> There are two outstanding issues that this would present though. One is if you have an arg that accepts multiple values, and has a value in some external argv, should a later value in either another external argv or via the explicit command line add to these values, or entirely override? The proposed system above entirely overrides, which I'm more of a fan of. If a consumer wants to provide a global default and allow users to add to those values, I'd be easiest to simply tell the user to include that default value in their own "overrides" or just re-add that value back after clap is done with it's parsing.

That's part of the reason I wanted the merge to be a later step. It allows these two cases to be implemented in the consumer:

1. Supporting things like `--config-file`. With your solution, one of the following has to happen:

    a. Clap needs explicit support for a new argument type
    b. Users need to call `get_matches`, then `external_argv(load_toml(matches.value_of('config_file').unwrap()))`, then `get_matches` again.

    With my solution, they just call `get_matches`, then call something in the vein of `get_matches_via_normalize(load_toml(matches.value_of('config_file').unwrap()))`, then call a clap-provided merging function like `matches1.update(matches2)`.

   No special "look ahead, then dynamically inject an `external_arg`" or "re-parse from the beginning with changed parameters" necessary.

2. Allowing users to control behaviour of multiple arguments.

    With your solution, clap dictates how it works. With my solution, users can easily extract the values which should add together before doing the merging and then add them together manually.

It also has the benefit that there's less uncertainty about whether clap will allow users to reuse the same `App` to parse multiple sets of merged inputs. That is, I'd be worried that `external_argv` might invalidate earlier references, requiring me to throw out `App` and create it anew every time when I need to do something like this:

```rust
let m = App::new("test")

let matches1 = m
    .external_argv(some_env_var1)
    .get_matches_from(args1);

let matches2 = m
    .external_argv(some_env_var2)
    .external_argv(user_cfg2)
    .get_matches_from(args2);

let matches3 = m
    .external_argv(some_env_var3)
    .external_argv(global_cfg3)
    .external_argv(user_cfg3)
    .get_matches_from(&[]);
```

> The only thing left to determine is how to represent free/positional arguments in these configs. Another :bike: :house: for sure, but options, and flags are simple. I'd suggest simply using a single "args" key and assigning the values in sequence such as args="foo bar baz".

> I wasn't clear about the positional args part, we could equally as easy use the key to individualize them, but I kind of like that they're forced to be in order with only a single key to keep from any confusion by accidentally putting them in the wrong order

At the very least, you'll want it to be `args=["foo", "bar", "baz"]` to avoid reinventing quoting/escaping.

With that said, this is definitely a tricky thing to address because:

1. Mapping one key to multiple schema entries feels like undesirable magic behaviour and makes `external_argv` more than merely a merging, trait-enabled version of `get_matches_from`, which also feels conceptually wrong.

2. If clap doesn't enforce the "list of positional arguments is never sparse" invariant, mapping them to individual keys in config files could introduce potential for logic bugs.

3. If clap *does* enforce non-sparseness for positional arguments, it could frustrate users when `cmd foo bar` keeps turning into "cmd foo bar baz` yet the shell is adamant that it's not meddling with it. 

(Wrapper scripts and shell functions are **the** standard, accepted way to augment or meddle with positional arguments. As someone who cares about user experience, I might go as far as slipping my own filter in between `load_toml` and clap if it didn't provide a way to opt out of positional arguments into the config file... and then we're right back to reinventing the config file schema because clap's implementation is too inflexible.)

I'd just treat positional arguments in config files as a validation failure and wait to see if anyone complains, since it can be relaxed without breaking anything. (I've never seen a config file or environment variable that allows specifying positional arguments in 10 years of using DOS/Windows command-line applications and another 16 of using Linux ones. It's always been wrapper scripts or shell features like `alias` and `function done() { command mv "$@" ~/done/; }`)

...or, in the case of DOS and Windows, wrapper scripts as `.BAT` files would looke like these:

```batch
REM DOS
move %1 %2 %3 %4 %5 %6 %7 %8 %9 %HOME%\DONE\
REM Windows
move %* %HOME%\Done\
```

**Finally,** since you didn't mention them either way, here are a couple of other points:

1. I have no problem with `Normalize` as long as it's implemented such that, if `.external_argv` accepts the output of `load_toml` directly, there's also an easy way to use only a specified subtree, so the file can contain multiple sections with only one of them being for clap.

2. You'll need `Normalize` implementations to have access to the schema because, otherwise, the only attainable normalized form is to flatten structured data from TOML/JSON/YAML/etc. into an `argv` Vec, just so they can be re-structured again once access to the schema *is* available... and that feels very unpleasant to think about.

(Without access to the schema, tell me whether `--foo --bar` is `{"foo": true, "bar": true}` or `{"foo": "--bar"}`... or `{'foo': 1 + (-1)}` for that matter, if you're dealing with options like `--verbose` and `--quiet`.)

---

_Comment by @kbknapp on 2016-11-28 18:14_

> Supporting things like --config-file. [...]

IMO, that's a little bit of a niche use-case. I'm not saying it's uncommon, but I think it's FAR more common to simply provide these "defaults" in a predetermined location. Unless you're using aliases, typing `$ myprog --config-file foo.toml` is basically the same as providing those defaults in the first place.

Due to how the internals of clap work, you can't just have a `matches.update(load_yaml!("foo.toml"))`, at a minimum you'd have to provide the `App` instance too because that's where the schema is defined. So it would end up being something long the lines of `app.update_matches_from(load_yaml!("foo.yaml"), &mut matches)`

Also parsing the arguments is very fast, it's building the `App` instance that takes more of the time. So calling `get_matches` more than once shouldn't be a real issue in practice, especially if it's only for a small percentage of uses cases (such as `--config-file <file>`). I'm not saying this is the best solution, but it's definitely the easiest considering I'll have to support this solution 😉 

> there's less uncertainty about whether clap will allow users to reuse the same App to parse multiple sets of merged inputs.

There shouldn't be any issue with re-using the same `App` instance to update the matches. This may just need to be documented better in this case. As for the example with `matches{1,2,3}`, it would depend on *how* you're trying to do this, but I'd need to see a real use case. The example you posted looks like it would work via this `app.update_matches_from()`, but not work with an `app.external_argv()`. Because the `app.external_argv()` would always allow adding to the `App` instance, but not taking away from it upon additional parses.

>  have no problem with Normalize as long as it's implemented such that, if .external_argv accepts the output of load_toml directly, there's also an easy way to use only a specified subtree, so the file can contain multiple sections with only one of them being for clap

That's the plan. As far as using a subtree, that should work, depending on the deserialization framework. I.e. this `external_argv` would simply take a TomlTable or YamlTree (not reall objects, just abstract ideas), whether thats an entire file's table/tree or just a subset doesn't really matter. If the consumer only wants to provide a subset, they just give that subset to the `external_argv` and not the entire deserialized file.

> You'll need Normalize implementations to have access to the schema because, otherwise, the only attainable normalized form is to flatten structured data from TOML/JSON/YAML/etc. into an argv Vec, just so they can be re-structured again once access to the schema is available... and that feels very unpleasant to think about. (Without access to the schema, tell me whether --foo --bar is {"foo": true, "bar": true} or {"foo": "--bar"}... or {'foo': 1 + (-1)} for that matter, if you're dealing with options like --verbose and --quiet.)

The `App` instance contains the schema. And parsing a flat argv is exactly what clap does already, so that's not an issue at all. In fact, the `Normalize` trait would simply flatten the Yaml/Toml/JSON/whatever into what clap is already good at parsing, a flat `Vec`.

> At the very least, you'll want it to be args=["foo", "bar", "baz"] to avoid reinventing quoting/escaping.

Good point. @nabijaczleweli has a branch with a parser which does all the whitespace/quotation handling that we could use/adapt. Although, the more I think about it, the more I'd rather do one of the following

1. Outlaw providing positional args in these configs (because you're right it could be very confusing to users, and I don't see a huge benefit so if someone does, please let me know)
2. *If* we do allow positional args, they could be in the simple `1="foo", 2="bar"` form where `2` requires `1`...but I still don't really like this because a user that types `$ myprog` may not realize what he's really doing is `$ myprog foo bar`, but then typing `$ myprog baz` does what...`$ myprog baz bar`? It just seems like extra magic that could go very wrong. 

Edit: updated Option 2 about the positional args

---

_Referenced in [clap-rs/clap#814](../../clap-rs/clap/issues/814.md) on 2017-01-11 02:10_

---

_Referenced in [clap-rs/clap#815](../../clap-rs/clap/issues/815.md) on 2017-01-14 02:17_

---

_Renamed from "Feature request – allow parsing options from a config file instead of command line arguments" to "Allow parsing options from a config file instead of command line arguments" by @kbknapp on 2017-01-30 04:52_

---

_Comment by @TruputiGalvotas on 2017-03-21 11:04_

I don't know whether anyone knows about this here: https://docs.rs/preferences/1.1.0/preferences/

But it seems you could just use this as an optional dependency instead of a separate implementation within clap to achieve the same thing. The clap would still be clap instead of swiss army knife of being a configuration file as well as command line parser.

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:57_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:57_

---

_Referenced in [TeXitoi/structopt#32](../../TeXitoi/structopt/issues/32.md) on 2017-11-24 18:13_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:29_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:29_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-05 15:29_

---

_Renamed from "Allow parsing options from a config file instead of command line arguments" to "Allow adding "external argv"s to be parsed alongside/before command line" by @kbknapp on 2018-02-05 15:30_

---

_Comment by @kbknapp on 2018-02-05 15:31_

This will be implemented as adding `App::argv` which clap will parse prior to the CLI, and the CLI will override options found in any of the `App::argv`s.

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:51_

---

_Comment by @lez on 2018-03-12 09:00_

Hi, I would like to work on this issue. Do you see any chance that it can be integrated to the 2.x branch, too, before 3.x comes out?

---

_Comment by @kbknapp on 2018-03-19 20:03_

@lez of course I can't stop people from working for free on this project, and all contributions are very welcome! :)

My only caution to people implementing things on 2.x branch is that all of it needs to be "back/forward ported" to 3.x branch. So if the changes are large, it's more difficult to port and thus ends up getting re-implemented anyways and increases the duplication of effort...or just doesn't make it to v3.

With that caveat aside, this particular change if limited to the base case needed should actually be pretty minimal and thus could be directly ported to v3 ;)

The base case of this being:

* Allow an App to specify an external argv which is just an iterator of items that are/can be converted to `OsStrings` (as that's what the internal parses iterates over).
* If multiple argv's are allowed, they must remain in order
* No merging strategy is defined as that is handled by the parsing logic, these argv's are simply concatenated in a FIFO manner (see below).

Imagine we had 2 external argvs, and the command line:

```
argv1 = ["a", "b", "c"]
argv2 = ["d", "e"]
cli = ["x", "y", "z"]

what_clap_parses = ["a", "b", "c", "d", "e", "x", "y", "z"]
```

---

_Removed from milestone `v3-alpha.1` by @kbknapp on 2018-07-22 01:00_

---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 01:00_

---

_Comment by @davidMcneil on 2019-11-26 12:38_

[This](https://github.com/davidMcneil/unified-dynamic-rust-app-config) is proof-of-concept work for integrating clap with a config file.

---

_Referenced in [entropic-dev/ds#25](../../entropic-dev/ds/pulls/25.md) on 2019-11-27 06:48_

---

_Comment by @pksunkara on 2020-03-03 13:07_

@CreepySkeleton Please read through this issue in relation to #1693 and how we can combine both of them

---

_Comment by @CreepySkeleton on 2020-03-03 16:41_

This is not the first time the "external argv" theme pops up, and we have also got few similar requests in structopt: [one](https://github.com/TeXitoi/structopt/issues/72), [two](https://github.com/TeXitoi/structopt/issues/197). I have actually read through this issue back in November or so, few rogue thoughts:

* We don't want to backport it to 2.x. Manpower is limited and, frankly, scarce. Let's focus on 3.x.
* Citing @kbknapp 
  >  I'd also like to limit claps responsibility to parsing arguments from a source. Not determining source order or source validity.
  > [...]
  > First, I'm not terribly interested in clap handling the file I/O part of this, i.e. the reading files, handling permissions, errors that come from this, precedence, etc. I intend for this to all happen in the consumer code. 

  I agree. Otherwise, we risk to stitch yet another kind of Frankenstein's monster, capable of everything but extremely hard to maintain/use.

  I like https://github.com/clap-rs/clap/issues/748#issuecomment-263348300 , specifically, the `update_matches()` part. I also agree that positional args are exempt from configs (i.e they are allowed only in the last update).

  https://github.com/clap-rs/clap/issues/748#issuecomment-374352572 is a simpler option which looks fine too. I vote for it as the bare workable minimum.

* I don't think this is related to #1693 . They can coexist. 

---

_Referenced in [clap-rs/clap#251](../../clap-rs/clap/issues/251.md) on 2020-03-27 10:22_

---

_Referenced in [clap-rs/clap#1837](../../clap-rs/clap/issues/1837.md) on 2020-04-17 09:41_

---

_Comment by @Kixunil on 2020-04-28 13:33_

If anyone needs this feature more than some other fancies like native subcommand support, you might be interested in my crate [`configure_me`](https://github.com/Kixunil/configure_me). From my experience, this is very common if you write long-running services instead of command line tools.

The whole crate took a lot different approach than clap. Mainly it's declarative-first, no stringly manipulation, no forcing into UTF-8 strings. (Disclaimer: maybe `clap` has these properties too now or I misunderstood them at the time.) I implemented it as build script instead of proc macro at the time mainly because it was (or seemed to be) simpler. However it turned out to be [a better approach](https://github.com/Kixunil/configure_me/blob/master/docs/why_not_derive.md) anyway. At least for now.

People here raised some interesting issues, so here are my answers - how I solved them in `configure_me`:

* First, generally I try to do what other Unix tools do in order to avoid surprising behavior (this led to for support of even some crazy things like `--arg=val` being same as `--arg val`, coalesced flags...)
* There's a definite ordering on sources, the value in the last source wins, unless custom merging is used
* The order is: config files (defined by the crate user, but generally `/etc...`, `~/...`), environment variables, arguments
* What if a user gives `--config FILE` argument? The values in that file override appropriate values that were parsed so far, including command-line values. This is consistent, so shouldn't be too surprising.
* Missing configs are ignored, but all other errors (permissions etc) are treated as fatal
* Including a file from within another file is currently unsupported
* Only Toml format is supported
* Custom merge function is entirely in hands of the user, but I'd recommend to not do anything crazy with it. I needed it in two cases only so far: merging maps and repeating arguments (~~which don't have a native support yet~~ they do now)
* Interestingly, `configure_me` can read contents of a directory too - very handy to implement `conf.d` - style configuration

I think the most important thing about this approach is: it works. Over past years there weren't *major* complaints about it. Some missing features/bugs were fixed as needed. Feel free to open issues if you find something not working for you.

---

_Comment by @kbknapp on 2020-04-28 14:08_

> I don't think this is related to #1693 . They can coexist.

Agreed.

I think we *could* combine the two, but I see them as different things. This issue (#748) is about allowing the consumer code to essentially make their own argfile impl if they choose. It's about making it possible to update matches from multiple argv sources (those sources are handled by user code).

I may have already said it elsewhere, but I tend to see this issue being more of just adding either `App::argv` which accepts an iterator over `OsStrings` (or could be a generic `Into<Iterator<Item=OsString>>`), and can be used multiple times (in the builder sense) and all those argv's are concatenated together, and the CLI (`std::env::args_os`) is concatenated at the end, and then parsing happens like normal (minus perhaps the exception that instead of conflicts, we use overrides).

#1693 essentially adds support for special casing a particular type of argument as an argv. That issue could perhaps use the impl of this issue internally.

---

_Comment by @kbknapp on 2020-04-28 14:10_

Also, just throwing my two cents into the ring; if I had to pick between this issue and #1693 I'd pick this issue. As implementing this issue would allow users who want an argfile to do so in thier own code, but the other way around isn't as easy.

---

_Comment by @pksunkara on 2020-04-28 14:13_

Yup, #1693 can use the implementation here. And then this issue could use the implementation for `replace` #1697. Which is why I said they are related.

---

_Referenced in [paritytech/substrate#6856](../../paritytech/substrate/issues/6856.md) on 2020-08-11 08:50_

---

_Removed from milestone `3.0` by @pksunkara on 2021-06-16 01:59_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Comment by @cmcqueen on 2021-11-12 06:54_

I reckon [ConfigArgParse](https://pypi.org/project/ConfigArgParse/) package for Python is a terrific reference for a good useful and featureful implementation.

---

_Referenced in [epage/clapng#66](../../epage/clapng/issues/66.md) on 2021-12-06 16:29_

---

_Label `E: optional dep` removed by @epage on 2021-12-08 20:35_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:18_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:18_

---

_Referenced in [clap-rs/clap#3113](../../clap-rs/clap/issues/3113.md) on 2021-12-09 16:53_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 16:54_

---

_Label `D: medium` removed by @epage on 2021-12-09 16:54_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 16:54_

---

_Comment by @jaskij on 2021-12-13 22:49_

While looking into solutions to this issue I came across the [`merge`](https://docs.rs/merge/latest/merge/) crate. Haven't tested it yet, but it seems that `#[derive(Deserialize, StructOpt, Merge)]` is a solution

Tthe only fault being the amount of boilerplate needed for defaults - serdes expects to use the Default trait, while StructOpt wants those as strings in field attributes.

The code sample in [structopt#150](https://github.com/TeXitoi/structopt/issues/150) shows how to have a single authority for those defaults in `impl Default`.

---

_Comment by @Kixunil on 2021-12-13 23:06_

Actually, `serde` allows you to specify custom default function. If `StructOpt` could use functions too, you could reference the same function.

---

_Comment by @epage on 2021-12-14 00:36_

> Tthe only fault being the amount of boilerplate needed for defaults - serdes expects to use the Default trait, while StructOpt wants those as strings in field attributes.

`StructOpt` supports `Default` via `#[structopt(default_value)]` (no parameter).  We've renamed this in `clap_derive` to `#[clap(default_value_t)]` because it also supports a typed value, rather than a string value.

The limitation to both is the type needs to implement `Display`.

---

_Comment by @jaskij on 2021-12-14 06:50_

> StructOpt supports Default via #[structopt(default_value)] (no parameter). We've renamed this in clap_derive to #[clap(default_value_t)] because it also supports a typed value, rather than a string value.

I must have mistunderstood how StructOpt works then, because to me it seemed as if the no-param `default_value` took the default value from the type of the field, not the config struct's `Default` impl.

Edit:

I do not want to be argumentative, just stating my observations - a seven value config has ballooned to over fifty lines of code using the method I mentioned.

@Kixunil: I indeed overlooked serde's option of using functions, but I don't think this helps here, especially with the difference in types. `clap_derive` seems to fix that - although it's hard to tell from the current docs whether it expects a callable or a value.

---

_Comment by @epage on 2021-12-14 14:38_

> I must have mistunderstood how StructOpt works then, because to me it seemed as if the no-param default_value took the default value from the type of the field, not the config struct's Default impl.

Yes, I thought you had meant `impl Default` for the field, rather than the container.  We have #3116 for using the container's `Default::default`.

> clap_derive seems to fix that - although it's hard to tell from the current docs whether it expects a callable or a value.

We support any expression.  In general, we need to loosen up our attributes to allow any expression so there isn't a question of what you can or can't do (https://github.com/clap-rs/clap/issues/3173)

---

_Comment by @jaskij on 2021-12-14 15:03_

@epage seems like clap V3 will be indeed a big change in a good direction!

I'd still love to see file handling too, but understand that you're reluctant to include it. I've given it some thought myself and it indeed is a tricky thing.

---

_Comment by @epage on 2021-12-14 15:09_

@jaskij note there is also a discussion on layered config at https://github.com/clap-rs/clap/discussions/2763, exploring different options.  I also include the [option I've landed on for now](https://github.com/clap-rs/clap/discussions/2763#discussioncomment-1301698)

---

_Referenced in [clap-rs/clap#3846](../../clap-rs/clap/issues/3846.md) on 2022-07-13 23:24_

---

_Referenced in [mintlayer/mintlayer-core#277](../../mintlayer/mintlayer-core/pulls/277.md) on 2022-07-14 20:55_

---

_Comment by @jettero on 2022-08-30 20:54_

Came here looking for the ConfigArgParser functionality as well. I think it's fine to leave the config parsing as an exercise for an external package... The problem I'm having is that if I really use clap and it's handling the defaults and things; then there's no interfaces available for me to complete the merge with commandline overwriting config if specified.

Ideally, I'd like to use --name value if it's there, and fall back on config name if it's there and finally try clap --name default if there wasn't anything else to use.

The problem is I don't see any obvious way to test the matches to see if the user specified the value or if clap fell back on the specified default so there's no way to complete such a merge.

Another strategy I've used in the past is to just read the config from the usual places (and give up on the idea of a --configfile flag) and use anything from the config files as the defaults for the switches and things. But I don't think you can do that at all with either the builder (using arg!()) or with especially with the derived macro interface things.

I'm pretty new to rust, but this seems like a bit of a showstopper on using clap for my current little project...  which really bums me out because clap is so neat and there seem to be very few alternatives (perhaps none). 

---

_Comment by @epage on 2022-08-30 20:58_

> The problem is I don't see any obvious way to test the matches to see if the user specified the value or if clap fell back on the specified default so there's no way to complete such a merge.

[`matches.value_source("arg")`](https://docs.rs/clap/latest/clap/parser/struct.ArgMatches.html#method.value_source) will let you know what set the value.

btw if you want to read up on more thoughts on layering clap with config, check out https://github.com/clap-rs/clap/discussions/2763

---

_Comment by @mzagrabe on 2022-08-30 21:20_

On Tue, Aug 30, 2022 at 3:58 PM Ed Page ***@***.***> wrote:

> The problem is I don't see any obvious way to test the matches to see if
> the user specified the value or if clap fell back on the specified default
> so there's no way to complete such a merge.
>
> matches.value_source("arg")
> <https://docs.rs/clap/latest/clap/parser/struct.ArgMatches.html#method.value_source>
> will let you know what set the value.
>

@epage, value_source looks pretty slick. Thanks for mentioning it.


> btw if you want to read up on more thoughts on layering clap with config,
> check out #2763 <https://github.com/clap-rs/clap/discussions/2763>
>

Looking at #2763 it is hard for me to tell if the (eventual) plan for clap
is to incorporate some sort of ConfigArgParse functionality. Is that
something that clap will eventually provide, or will it always be left up
to the application to parse options from a config file?

Thanks for the help and answers... and code!

-mMessage ID: ***@***.***>

>


---

_Comment by @epage on 2022-08-31 02:46_

> Looking at [#2763](https://github.com/clap-rs/clap/discussions/2763) it is hard for me to tell if the (eventual) plan for clap
is to incorporate some sort of ConfigArgParse functionality. Is that
something that clap will eventually provide, or will it always be left up
to the application to parse options from a config file?

The recommended route for layering configs is still TBD.  There are multiple strategies people can use now and some improvements that we are making for v4 that will unblock other strategies.

---

_Comment by @jettero on 2022-08-31 05:44_

Oh, fantastic. Thanks very much for the tips. I feel like there's ways out of the trap now and I can stick with clap (which I like very much). Game on.

I'm daydreaming about coming up with an external crate that solves the problems I want regards to ConfigArgParse behavior. I like the idea that rust packages can be "finished" and to avoid feature creep, this new functionality should perhaps be added in another package. I'm just not the right guy for the job yet as I'm still very new to rust.

I'm encouraged that the stubs/api-thingies exist to let this happen in another crate.

[edit] it also looks like others have already done the things I was thinking about but I didn't know what to search for. (e.g., clap_conf looks promising). Hrm, that seems tied to ^2.33.0 ... oh well, some day.

---

_Comment by @mzagrabe on 2022-09-28 18:48_

> > Looking at [#2763](https://github.com/clap-rs/clap/discussions/2763) it is hard for me to tell if the (eventual) plan for clap
> > is to incorporate some sort of ConfigArgParse functionality. Is that
> > something that clap will eventually provide, or will it always be left up
> > to the application to parse options from a config file?
> 
> The recommended route for layering configs is still TBD. There are multiple strategies people can use now and some improvements that we are making for v4 that will unblock other strategies.

@epage, would you be willing to mention the different "multiple strategies" and also what improvements to v4 that have been made to unblock other strategies, and what those "other strategies" would be?

Thanks!

---

_Comment by @epage on 2022-09-28 19:33_

https://github.com/clap-rs/clap/discussions/2763 explores the different options.

clap v4 added `ArgMatches::ids` so you can iterate over what arguments are present.  Generic laying was a common motivation for people requesting that feature.

---

_Referenced in [rust-cli/book#212](../../rust-cli/book/issues/212.md) on 2022-12-20 20:30_

---

_Referenced in [restatedev/restate#8](../../restatedev/restate/pulls/8.md) on 2023-01-13 13:46_

---

_Comment by @ghenry on 2025-03-16 22:20_

Hi,

Was searching issues for musl lib and found this. I can't use clap parse on Alpine, so am switching to parse_from and converting argv to a vec.

Unless this was solved here?

Here's why https://users.rust-lang.org/t/binary-wont-run-ffi-alpine-musl-and-c/127012/13 

Thanks!

---
