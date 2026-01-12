```yaml
number: 254
title: "Allow access to `Arg` properties"
type: issue
state: closed
author: wackywendell
labels:
  - S-waiting-on-decision
  - A-docs
assignees: []
created_at: 2015-09-13T21:22:01Z
updated_at: 2018-08-02T03:29:44Z
url: https://github.com/clap-rs/clap/issues/254
synced_at: 2026-01-12T16:14:08Z
```

# Allow access to `Arg` properties

---

_@wackywendell_

When building an `Arg`, right now one can only do so in a functional style. To be more extensible, it would help to add functions for some of the following functions:

``` rust
get_name(&self) -> &str;
set_name(&self, &str);

get_short(&mut self);
set_short(&mut self, &str);

get_long(&self) -> &str;
set_long(&mut self, &str);

get_help(&self) -> &str;
set_help(&mut self, &str);

get_required(&self) -> &bool;
set_required(&mut self, &bool);
```

...and so on. Based on my understanding of the architecture of `clap`, this should not require any serious modifications to the base architecture: adding getters and setters to `Arg` looks like it will require minimal modification of `Arg`, and since `App` takes ownership of `Arg` instances as they are added and does not allow them to be modified, this should not require any modification of `App` whatsoever.

I request this because I would like to be able to build a TOML parser alongside `clap` (a la #251), so that the TOML library can allow a user to specify a list of names and types (types for the TOML), use the names to build a `Vec` of `Arg` instances, allow the user to add options to the `clap` library, and extract out the necessary information (e.g. `get_help` and `get_long`) to more easily parse the TOML.


---

_Comment by @kbknapp on 2015-09-13 22:49_

While I'm not totally against adding this, I do wonder if it would either pollute the API namespace (or rather just clutter...pollute is the wrong word), or initially confuse consumers of the library. 

I would lean closer to simply using the struct fields directly to accomplish the same thing without adding a bunch of new methods, also being that I would imagine using `clap` in the non-functional way would be the exception and not the rule.

The fields of `Arg` are already public, they're just hidden from the documentation. Exposing them (in the docs) may be a better route to take.

Thoughts, opinions?


---

_Label `T: RFC / question` added by @kbknapp on 2015-09-13 22:49_

---

_Label `W: maybe` added by @kbknapp on 2015-09-13 22:49_

---

_Label `C: args` added by @kbknapp on 2015-09-13 22:49_

---

_Label `D: easy` added by @kbknapp on 2015-09-13 22:49_

---

_Label `E: tedious` added by @kbknapp on 2015-09-13 22:49_

---

_Renamed from "Add `get` and `set` traits to `Arg`" to "Allow access to `Arg` properties" by @wackywendell on 2015-09-14 02:01_

---

_Comment by @wackywendell on 2015-09-14 02:02_

That sounds like an excellent idea to me; I modified the title of this to match that. Thanks!


---

_Comment by @WildCryptoFox on 2015-09-14 04:10_

The functional builder pattern has a MASSIVE advantage. That is, the compiler will fold it down to just defining the built structure. Breaking the builder pattern down like this might loose this property.


---

_Label `R: conflicts` added by @WildCryptoFox on 2015-09-14 04:11_

---

_Label `R: conflicts` removed by @WildCryptoFox on 2015-09-14 04:11_

---

_Comment by @WildCryptoFox on 2015-09-14 04:11_

Sorry for the conflicts label. I was trying to add `R: needs benches`


---

_Comment by @wackywendell on 2015-09-14 14:35_

> The functional builder pattern has a MASSIVE advantage.

That's a very strong statement. Have you tested this? Does it fold it down? Does breaking it prevent folding it down? What effect does this have on memory usage / CPU usage? Is this difference significant in comparison to string parsing, such as is necessary for any CLI and/or `from_usage`? If this is a "MASSIVE" advantage, what is it MASSIVE relative to?

I think the amount of advantage it offers probably depends a great deal on what you are trying to achieve. If performance in building the CLI parser is critical, it may be an important advantage; if, however, building the CLI parser is not a particularly critical part of your code, as I know it won't be for my application (parview), then having the flexibility to do things other ways can be a significant advantage. In this particular instance, I imagine even just opening, reading, and parsing a TOML file or two may outclass the CLI parser-builder in terms of resources needed, and the flexibility would be greatly appreciated. Of course, I'm not certain, and I'm not advocating eliminating the builder pattern, simply giving users the flexibility to make their own choice depending on their needs.


---

_Comment by @WildCryptoFox on 2015-09-14 16:31_

Builder pattern -> Constant Folding through LLVM's SROA, which treats components of a struct as individual SSA variables.

See https://en.wikipedia.org/wiki/Constant_folding and https://en.wikipedia.org/wiki/Static_single_assignment_form

As I mentioned for needing benchmarks, I'm unsure how fragile this optimization is. Getters possibly before some setters may imply that the default value, or current at the execution point could be required _at those points_. If all setters are done first, and getters applied after the fact, this should not be an issue.

When I said MASSIVE, I was referring to for simply being a property of the builder itself. The optimization works by inlining and factoring down to the constants in each variable; defining them once instead of going through what the builder pattern appears with re-declaration of these struct fields.

I'm not against the idea of getters, just stated that this property needed a mention as getters _might_ influence the setters.

---

eddyb on IRC reminded that if the getters are by-ref, it already prevents mutation; so all setters must be first by Rust's borrowck rules already (unless otherwise scoped down to free up for mutation).


---

_Label `C: docs` added by @kbknapp on 2015-09-17 22:26_

---

_Comment by @kbknapp on 2015-09-17 22:27_

For the time being, let's add some solid docs to the public fields of `Arg` but with the caveat that this is not intended for normal use.


---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-19 10:37_

---

_Comment by @kbknapp on 2015-09-21 03:46_

@wackywendell Once #261 gets merged, is there something further that needs to be changed in order to accomplish what you're working on, or this issue closed?


---

_Comment by @WildCryptoFox on 2015-09-21 04:21_

@kbknapp #261 only shows the fields in the documentation. They were already public. I think the issue here is enumerating over the already in the `App` structure. This is not an issue, however, worth the mention that any handling of the `Vec<Arg>` that needs to occur; should be before it is added into the `App`. (Untested code follows)

``` rust
let args: Vec<Arg> = vec![Arg::with_name("foo")..., ...];
let keys = args.iter().map(|a| a.name).collect();
App::with_name("MyApp").args(args);
```


---

_Comment by @wackywendell on 2015-09-21 11:42_

#261 closes this issue.

@james-darkfox does make a good suggestion for my overall architecture which I have been using, although it leaves things a bit awkward for the users of my library, as they have to do something like `config.add(Arg(...).short('...'), other_option)`, where `other_option` is an option for my library. I have not yet looked carefully enough at #259 to understand what impact that will have.

Thanks!


---

_Closed by @wackywendell on 2015-09-21 11:42_

---

_Comment by @WildCryptoFox on 2015-09-21 12:06_

@wackywendell My rewrite isn't yet ready. It's essentially a tiny implementation of what clap already is. What is the effect of the `other_option`? I noticed after referencing this from my rewrite; that you can just map against the `Vec<Arg>` with the current code; hence the code in my last comment.


---
