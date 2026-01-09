---
number: 1104
title: "Allow Enum Variants as Arg|ArgGroup|SubCommand keys"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-builder
  - S-wont-fix
assignees: []
created_at: 2017-11-12T20:11:18Z
updated_at: 2022-05-03T14:13:27Z
url: https://github.com/clap-rs/clap/issues/1104
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow Enum Variants as Arg|ArgGroup|SubCommand keys

---

_Issue opened by @kbknapp on 2017-11-12 20:11_

I.e. remove the "stringly typed" nature of clap

How this is done internally is up for debate and the reason for this issue. See the related topic #1041 

Basically we want to Hash whatever key is given and store that instead of a `&str`/`String`/`Cow<_>`.

---

_Comment by @kbknapp on 2017-11-12 20:13_

Even without a derivable component we'll need to iron out the constraints that are part of clap which `ArgMatches::value_of` accepts instead of `&str`

---

_Label `C: matches` added by @kbknapp on 2017-11-12 20:16_

---

_Label `D: intermediate` added by @kbknapp on 2017-11-12 20:16_

---

_Label `P3: want to have` added by @kbknapp on 2017-11-12 20:16_

---

_Label `T: enhancement` added by @kbknapp on 2017-11-12 20:16_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-11-12 20:16_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-13 02:18_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:27_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:27_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-05 15:27_

---

_Referenced in [clap-rs/clap#459](../../clap-rs/clap/issues/459.md) on 2018-07-22 02:41_

---

_Comment by @jacob-greenfield on 2019-05-30 06:04_

Is this still being considered? I really like this approach for a number of reasons. I saw the [recent blog post](https://clap.rs/2019/03/08/clap-v3-update-no-more-strings/) didn't mention it which is why I am asking.

I'm not sure if this is possible, and I think it this would rely on rust-lang/rfcs#2593. Anyway I imagine the following scenario. The user defines an enum. Each variant represents an argument. You call `ArgMatches::get_value<Variant: Args>() -> &str` where `Variant` is *the variant type itself* (see the linked RFC). That function gives you an instance of *T*, and you can get information from the associated value.

For example (pseudocode):

```rust
// in Clap:

struct Arg<Variant> {
    // everything here remains the same
}

// here `Args` is the enum itself
struct App<Args> {
    ...
    fn arg<T: Into<Arg<Self::Args>>>(self, a: T) -> Self
    fn get_matches(self) -> ArgMatches<Self::Args>
}

// here `Args` is the enum itself
impl ArgMatches<Args> {
    ...
    fn get_value<Variant: Self::Args> -> &str {
        ...
    }
	fn get_values ...
}

// user code (no argument name strings!):

enum MyArgs {
    Username,
}

let matches = App<MyArgs>::new("foo")
    .arg(
        Arg<MyArgs::Username>::new().short("u")
    )
    .get_matches();

let username = matches.get_value::<MyArgs::Username>();
```

Alternatively, if the Rust team ends up deciding to allow you to restrict enum variants more (not currently planned AFAIK):

```rust
// in Clap:

// here `Args` refers to a variant of an enum
struct Arg<Variant: Args> {
    fn new_single() { ... } // Only allowed if the variant has an associated string
    fn new_multiple() { ... } // Only allowed if the variant has an associated iterator
    fn new_boolean() { ... } // Only allowed if the variant has an associated boolean
}

// here `Args` is the enum itself
impl ArgMatches<Args> {
    ...
    fn get_value_automatic<Variant: Self::Args> -> Variant {
        ...
    }
}

// user code (no argument name strings!):

enum MyArgs {
    Usernames(IntoIterator<String>),
}

let matches = App<MyArgs>::new("foo")
    .arg(
        Arg<MyArgs::Usernames>::new_multiple().short("u")
    )
    .get_matches();

let MyArgs::Usernames(usernames) = matches.get_value();
```

This way, there is a single function `get_value` that returns the appropriate return type for any variant you give it. That means for example you can't pass a multi-argument to `ArgMatches::value_of` like you can today.

Anyway this is all still up in the air anyway since it seems like the Rust team hasn't settled on exactly what features they plan to add to enums. What do you think?

---

_Removed from milestone `v3-alpha.2` by @pksunkara on 2020-02-01 07:45_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:45_

---

_Referenced in [clap-rs/clap#1663](../../clap-rs/clap/issues/1663.md) on 2020-02-03 09:32_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 10:27_

---

_Label `C: arg groups` added by @pksunkara on 2020-04-12 10:27_

---

_Label `C: args` added by @pksunkara on 2020-04-12 10:27_

---

_Label `C: subcommands` added by @pksunkara on 2020-04-12 10:27_

---

_Comment by @magistau on 2021-01-14 17:37_

Is there a workaround to this?

---

_Referenced in [drogue-iot/drg#4](../../drogue-iot/drg/pulls/4.md) on 2021-03-17 16:37_

---

_Comment by @epage on 2021-07-19 18:56_

I tried to read up on past effort for enums but [the blog post was removed](https://clap.rs/2019/03/08/clap-v3-update-no-more-strings/) as part of the new site and for some reason [the wayback machine doesn't have that one article](https://web.archive.org/web/20190509121951/https://clap.rs/)

@kbknapp happen to have this hanging around somewhere?

---

_Comment by @kbknapp on 2021-07-19 19:09_

I believe I do. I'm on mobile at the moment and will look it up shortly once I get to a computer.

---

_Comment by @kbknapp on 2021-07-19 21:07_

I found it.

<details>
<summary>Here is the post content </summary>
# Issue: Removing Strings

One of the biggest complaints about clap is the “stringly typed” nature of arguments. You give them a string name, and retrieve their value by string. Where this goes wrong is if you typo a string name, you won’t know until you hit that code path. Combined with clap goes to decent lengths to ensure the clap::ArgMatches struct keeps memory usage low which means stripping out any arguments which weren’t used at runtime. This means the clap::ArgMatches struct doesn’t know if you typo’ed an argument value, or it simply wasn’t used at runtime.

The solution to above problem would be keeping a list of all arguments in the clap::ArgMatches struct. However, for CLIs that have hundreds of arguments, the memory usage adds up quickly.

So there are two (actually three) problems being conflated here:

* Users can typo argument names with no compiler help catching this (the can actually typo names in a number of places, definition *and* each individual access)
* clap::ArgMatches has no knowledge of defined arguments
* During parsing, clap must store, iterate, and compare all these strings many, many times which would be much more efficient with other types.

## Challenge: Typo’ing Argument Names

The solution to this would be to use enum variants. This way the compiler can tell you, no such enum variant exists if you typo it.

The question then is how to store such a value in each Argument (and clap) for parsing to be able to compare.

Another constraint placed on myself, is I don’t want existing clap users to have to upgrade en masse.  If you’re happy using Strings, or you have a CLI that works, why spend the resources to change it.  That means the requirements are:

* Allow enum variants to be used
* The solution must also allow for strings to provide an easy upgrade path

Before I talk about the solutions I tried, and ultimately chose let me speak about the other challenges.

## Challenge: clap::ArgMatches has no knowledge of defined arguments

Ok, so now we have to come up with a solution which also:

* Is efficient enough, we can afford to store all defined arguments in a clap::ArgMatches

If we quickly look at the current naive solution of storing Strings, so we know our upper bound, it would look something like (note: the actual types are nested and shown here as essentially primitives for brevity): 

```rust
struct ArgMatchesV2 {
    args: Vec<(String, Vec<OsString>)>,
    subcommand: Box<ArgMatches>
    ... 
}
struct ArgMatchesV3 {
    args: Vec<(String, Option<Vec<OsString>>)>,
    subcommand: Vec<(String, Box<ArgMatches>)>
    ... 
}
```

I put the `subcomamnd` field in this example, because it’s important to keep in mind this math can explode if multiple levels of sucommands are used.

That doesn’t look different! You’re right. However, if one used two arguments at runtime (out of a possible valid 100 for instance) `args` will only consist of two elements. Whereas in v3 it’ll consist of the full 100 elements.

Since a String is essentially a Vec<u8>, and a Vec<T> is a pointer plus length (usize) plus capacity (usize). That means it’s an additional 98 * (usize*3) *plus* whatever the actual String bytes are that are allocated, which if we just say an average of five bytes per String for simple math, and on my system a pointer/usize is 8 bytes, the difference between the above ArgMatchesV2 and ArgMatchesV3 is:

* V2 = 2*((8*3)+(5*5)) = 2*49 = 98 bytes.
* V3 = 100*49 = 4,900 bytes.

That’s just args. Look at subcommands:

Same math as before, but with the way subcommand work you can only ever have a single subcommand per “level”. However, now we’d need to store all possible subcommands, which even if Box::new doesn’t allocate for all the subcommands we didn’t use it’s still an additional String (49 bytes on average) plus a pointer (8 bytes). Now subcommand numbers *usually* don’t  go as high as args, possibly 30-50 at most. Let’s just say 25 for easy math:

* V2 = 1*8 = 8 bytes
* V3 = 25*(49+8) = 1,425 bytes

Giving us a grand total of:

* V2 = 106 bytes
* V3 = 6,325 bytes

Ok we have our baseline.

## Challenge: Iterating and comparing strings is slow

Ok, whatever solution we go with, iterating Arg struct (and thus whatever this new field is) should be faster than a bunch of Strings. Since we’ve already gone over the math and breakdown of a String, we know it involves a heap lookup (Since Strings are a essentially Vec<u8>), so if we can avoid touching the heap that’d be gravy.

## Solution 1: Generics

My first attempt at solving this was to use a generic type bound on PartialEq. While implementing this, I had an icky feeling though. All my type signatures were changing to clap::Arg<T: PartialEq> which expands out to clap::ArgMatches<T: PartialEq>. This has a few unfortunate side affects:

* People would have to annotate their types, making upgrading to clap v3 a pain
* T might be huge, which doesn’t fix the clap::ArgMatches low memory requirement
* clap codegen booms because you might have multiple T’s throughout your CLI (this is problem for one of the later challenges)

So this doesn’t seem like a good route. Next I tried Trait Objects

## Solution 2: Trait Objects

Ok, so what if instead of a type T: ParitalEq I just stored &’a PartialEq (or Box<PartialEq>)? This fixes the problem of T being huge, now T is just a pointer (8 bytes). It also fixes the type signature as people still would just use clap::Arg. However, I’m still touching the heap each time I want to compare or find an argument.

I put this on the back burner as a *maybe*.

## Solution 3: Hashing

Ok, so what if I simply hash T and get a consistent output (I picked u64, or 8 bytes)?

Sure, I’ll have to hash all 100 T’s at definition time, but clap iterates and compares each argument *far* more than once per argument. So it seems a small price to pay, especially if I only need a fast hashing algorithm instead of a super secure one.

So I picked a FNV hash implementation which is insanely fast. Hashing a T using FNV is only a few ns for all the types of T I tested. Using this, clap::Arg can now store an `id: u64` (8 bytes vs the 24 bytes of a String), iterating and comparing a bunch of u64’s is *significantly* faster than a bunch of Strings.

Without going through all the math again, here are grand totals using u64 instead of String (not to mention u64 doesn’t allocate any additional bytes for it’s content):

* V2 = args(2*8)+subcommands(8) = 24 bytes
* V3 = args(100*8)+subcommands(25*8) = 1,000 bytes

Much improved. The jury is still out on whether I’ll provide the ability to know if an argument was used or not. 1,000 bytes (nearly 1KB) is still a good bit, compared to the 24 it could be. Granted, this is for a large CLI (100+ args and 25+ subcommands at a single level), so actual uses should be lower. We’ll see, although 1,000 bytes is much easier to swallow than over six times that.

The biggest downside is each access now must be hashed. The reason I’m OK with this, is access is much less frequent, and typically only happens once. So assuming 100 arguments, we’re comparing 200 hashes with that of storing, iterating, and comparing a larger heap based type for more than 200 times. I haven’t counted lately, but due to how parsing and validation occurs the number of times an argument gets compared is in the order of (NUM_ARGS * NUM_USED_ARGS)*~3ish. Concretely that means, if you use an average of 5 arguments per run, out of a possible valid 100, there are roughly 1,500 comparisons of equality. For CLIs like ripgrep which can have hundreds or thousands of arguments per run, this number can dwarf 200 hashes easily.

## Status

I’m in the middle of implementing this change. It’s somewhat conflated with re-working how the parsing happens in order to be more efficient. This is the last major change prior to releasing v3-beta1 which I’m hoping will happen in the coming weeks/month.


</details>

---

_Comment by @epage on 2021-07-20 14:01_

@pksunkara I propose this issue and #1663 be deferred to the clap4 milestone.
- This issue is from 2017 and the work required for it was decided to be deferred in the 2019 blog post quoted above
- Adding clap-derive reduces some of the value of this
- This is a big implementation change, with design work, that seems like it could unnecessarily bloat the remaining clap3 work.
- This is somewhat similar to #1041 which we already deferred.

---

_Comment by @epage on 2021-07-20 15:14_

With that out of the way, some quick thoughts:

Clap has several nearly distinct APIs
- The raw API
- YAML
- From example
- Macro
- Derive

In considering our care abouts, we need to consider each API and what the expected role of it is.  

For example, if the expectation is someone will probably use the derive, unless they want to get rid of the overhead of proc macros, then derives for custom keys (#1663) seems contradictory.

Similarly, the derive API prevents the problems with stringly typed APIs.  On a related note, I'm unsure if we can generate the needed enums for the derive API to use anything but strings.  We can't see through to other enums and structs to generate a global list.  We'd need to support a composable enum and then somehow allow all of the typing for `App` with subcommands to magically work.  This would also bloat the binary because we'd have multiple copies of App and ArgMatches being compiled.

As for performance, while keys use less memory, `&'static str` are fast to compare by equality.  I suspect Rust is short-circuiting with a pointer check.  We just can't use it because *sometimes* people use generated strings.

We might be able to resolve the generics problem via type aliases (e.g. `pub type App = BaseApp<&'static str>`).

One problem with enums is that we default `value_name` to be the `name`.  We can't do this with enums unless we require a `Display` or `as_str()` on it.

Regarding the hash work in the above blog post, one concern brought up at the time was collisions.  I can't find the discussions to see how that was resolved.

I propose we rename `App` et all and make them generic.  We then provide type aliases that set the generic parameter to `&'static str`.  This will require those who need allocations to go through more hoops, though they could choose to use `kstring::KString` (#1041).

See [playground for example code](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=4fac2959cc5f6c40d8810c80e68ac367)
- The main problem with this is the arg match lookups require an allocation for the string case.  We'd need to play with it more to allow borrows.
- I tried to design it so the current hashing system from the above blog post can be abstracted with it

Benefits
- clap-derive gets an optimal solution (`&'static str` for avoiding allocations and for fast lookups)
- The clap easy path is the fast API but users can step up to a more powerful API (solves #1041)
  - Users can choose enums or dynamic strings for us with clap if they want (solves this, #1104)
- We can more easily compare potential solutions (KString vs String vs `&'static str` vs hash vs enum, etc) and choose what works best
- We keep the code simple compared to other options
  - Only 1 type to be generic over rather than several
  - The types in use don't have to implement custom types
- We can reject #1663 

Downsides
- Still might be messy to get all of this working
- Time to implement all of this that could be spent on other work

---

_Comment by @epage on 2021-07-20 17:22_

Just wanted to call out https://github.com/clap-rs/clap/issues/604 is the issue for "error on typo"

---

_Removed from milestone `3.0` by @pksunkara on 2021-07-25 14:06_

---

_Added to milestone `4.0` by @pksunkara on 2021-07-25 14:06_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#82](../../epage/clapng/issues/82.md) on 2021-12-06 16:37_

---

_Referenced in [epage/clapng#135](../../epage/clapng/issues/135.md) on 2021-12-06 19:11_

---

_Label `A-derive` added by @epage on 2021-12-08 19:54_

---

_Label `C: args` removed by @epage on 2021-12-08 19:55_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 19:55_

---

_Label `C: matches` removed by @epage on 2021-12-08 19:55_

---

_Label `C: arg groups` removed by @epage on 2021-12-08 19:55_

---

_Label `A-derive` removed by @epage on 2021-12-08 19:55_

---

_Label `C: arg enums` removed by @epage on 2021-12-08 19:55_

---

_Label `A-builder` added by @epage on 2021-12-08 19:55_

---

_Comment by @epage on 2021-12-09 17:18_

In thinking on this more, I propose we reject this
- The `enum` would need several traits to be implemented to work.  For maintenance sake, this is pushing people to use a derive.  At that point, why not use `clap_derive` to get everything checked at compile time?
  - A trait for the actual hash
  - To keep the implicit `value_name` would require the enum have a `Display`
  - To populate subcommands in help and then convert an arg to the enum would require `Display` and `FromStr` implementations
- Enums do not compose.  You can't pull in `clap_verbosity_flag` if you are using enums
- We've improved the situation with asserting on unrecognized names
- This will be pushing generics onto any code that deals with clap, like the completion generators, which will make them messier / harder to use

@pksunkara what are your thoughts?

---

_Label `D: medium` removed by @epage on 2021-12-09 17:51_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 17:51_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-09 17:51_

---

_Comment by @pksunkara on 2021-12-10 02:09_

Those are good reasons for me to agree with. But I think we should let @kbknapp weigh in too.

---

_Comment by @ta32 on 2021-12-10 02:59_

Hi 

So far the work around I have been using is using strum to convert enums to str slices.

```rust
#[derive(EnumString, Display, AsRefStr)]
pub enum Args {
    User
}

fn main() {
    let app = clap::App::new("app");
    let matches = app
        .version("0.1.0")
        .usage("app -user")
        .arg(
            clap::Arg::with_name(Args::User.as_ref())
                .short("u")
                .takes_value(true)
        )
        .get_matches_safe();

    match matches {
        Ok(matches) => {
            let user = matches.value_of(Args::User.as_ref()).unwrap();
            app(user );
        }
        Err(e) => {
            println!("{}", e);
        }
    }
}
```
https://github.com/Peternator7/strum
Is this kind of thing users want? 
Strum has implementations that will derive a lot of things for enums like, display, fromStr, enumIter ... ect I have found it very useful. 

Maybe something like this could be made more ergonomic, with additional attributes.






---

_Comment by @epage on 2021-12-10 18:02_

@ta32 Yes, strum or something like `ArgEnum` can help get the rest of the needed features and solves one part of the whole.  I'm just looking at the whole and wondering whether its worth it. 

Let's start with the remaining API changes.  We'd either need to
1. Add a generic type of `T: Key + Display + FromStr`
2. Add a generic type of `T: Key`, scaling back to only Args and not Subcommands and remove our defaulting of `value_name` with `name`
3. No generic type by scaling back to only Args and not Subcommands and just require that the `Key` hash of `T` equal the hash of `T::to_string()`, which removes the compile-time benefits and is the equivalent of creating `const FOO_NAME: &str = "foo";`

Each of the options using a generic type would only work with clap code that is also generic on that type, limiting code sharing.

We then have to deal with the recursive nature of this across subcommands.  Most likely, we will end up with users creating a single `enum` for all subcommands.  This then loses some of the compile-time checking benefits (the `pest` parser does this and it is one of the things that makes me not want to use it).

Then from a users perspective, they would be most likely needing to use a proc macro to help them manage this enum, like strum.  The question for me to understand is the overlap between users who want to use the builder API but are still ok with proc macros (one reason to avoid the derive API is to save on compile times).

On the other hand, today without this feature, users can do the workaround you provided which is similar to my Option 3.   If we decide against this feature, we'd probably want to make an example out of your code snippet.

So in balancing out the different aspects, it makes me wonder if the derive API and workarounds like yours are sufficient.

---

_Referenced in [clap-rs/clap#1041](../../clap-rs/clap/issues/1041.md) on 2022-02-02 20:55_

---

_Comment by @epage on 2022-02-02 20:56_

With the above reasoning, I'm going to go ahead and close this.  This was an ambitious idea but unfortunately, it doesn't seem like its going to pay off.

I did note the workarounds discussed in this thread in https://github.com/clap-rs/clap/issues/1041 so we can make sure we continue to support them.

---

_Closed by @epage on 2022-02-02 20:56_

---

_Label `S-waiting-on-decision` removed by @epage on 2022-05-03 14:13_

---

_Label `S-wont-fix` added by @epage on 2022-05-03 14:13_

---
