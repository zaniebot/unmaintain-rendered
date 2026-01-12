```yaml
number: 809
title: add --include/--exclude flags, with correct precedence
type: issue
state: open
author: pikeas
labels:
  - enhancement
assignees: []
created_at: 2018-02-16T01:00:18Z
updated_at: 2025-07-17T14:08:06Z
url: https://github.com/BurntSushi/ripgrep/issues/809
synced_at: 2026-01-12T16:13:22Z
```

# add --include/--exclude flags, with correct precedence

---

_@pikeas_

On rg 0.8.0 / OS X 10.12.6

When using `rg -Tfiletype -tfiletype foo`, the exclude takes precedence over the include. Is this intentional behavior?

My use case is a `.ripgreprc` which excludes minified files by default, which I do occasionally need to search. I can do `rg --type-add=minified:*.min.{css,js} -tminified --no-config foo` in this case, but this is pretty clunky. I would much rather do `rg -tminified foo` and have my include clobber my config file's default exclude.

---

_Comment by @BurntSushi on 2018-02-16 01:07_

This is a limitation of ripgrep's argv parser. It is the same reason why ripgrep doesn't have traditional --include and --exclude flags for example.

I would love to fix this, but I don't know the optimal route.

---

_Comment by @kbknapp on 2018-02-16 02:54_

> It is the same reason why ripgrep doesn't have traditional --include and --exclude flags for example

Could you expand on that, as it might be in my sphere of influence to change? 

The way I understand it, it's currently set up so that excludes *always* override includes, no? The way I imagine in the simple case is just using normal overrides in whichever flag is last "wins" (or overrides the prior). However, I'm assuming in ripgrep's case this is complicated by having multiple includes/excludes?

i.e. what do I mean:

```
$cat ripgreprc
--type-add 'foo: *.foo' 
--type-add 'bar: *.bar'
-Tfoo
-Tbar

$ rg -tfoo
    # should ripgrep search 'bar' files?
```

In ripgrep's help message for `--type` states, `"**Only** search files matching TYPE"` (emphasis mine).  In which case, traditional overrides work perfectly here. However, the "do what I expect not what I say" in me says, "I want to search everything, including 'foo' files, but I still want 'bar' files to be skipped." (more traditional `--include`/`--exclude`).

Edit: So keeping the current functionality as is (perhaps just adding overrides?) seems fine, with the ability to also add more traditional include/excludes. As there may be times I *do* want to be able to say, "ripgrep, ONLY search this one file type" which wouldn't be possible with traditional includes/excludes. So I see them (current type system with traditional include/exclude) as not mutually exclusive ideas.

From a implementation standpoint traditional include/excludes are much harder to implement than just, "`--type` and `--type-not` override each other" as it would have to resolve *only matching types* in an overriding fashion.

If what I've said is true so far, there are a few ways I could actually tackle this on my end. The easiest would be to provide something like `Arg::overrides_eq_vals_with` which only overrides specific values with another arg provided both values match. Because clap already has all this information about what comes in what orders, it'd be quite simple to add.

i.e. assume we had an argv (after config parse) that was roughly `$ rg --exclude=*.foo --exclude=*.bar --include=*.foo 'test'` After `overrides_eq_vals_with` it would become `$ rg --exclude=*.bar --include=*.foo 'test'`.

The other way would involve something I've been thinking about, which is to provide companions to the validators, namely the ability to provide functions to `Filter/Skip`, `Map`, and `Action` arguments mid parse. These would do things such as do something potentially impure (`Action`), allow one to filter/skip values (`Filter/Skip` haven't decide on naming yet), or transform actual values themselves (`Map`) to be saved in the matches struct. These would all have access to the matches struct as it exists at that time, in order to perform decisions or possibly even mutate. I should say though that this method is further off and would not be available until v3 at the earliest (if ever, pending design and implementation, etc.).

----

Apologies for the length! As I began typing it was mainly me talking myself through the problem at hand 😜 

Edit: Added some words about current `--type` vs `--include`/`--exclude`

---

_Comment by @BurntSushi on 2018-02-16 12:19_

> However, I'm assuming in ripgrep's case this is complicated by having multiple includes/excludes?

Exactly. Basically, you want `--exclude A --include B --exclude C` to retain its ordering in the same way that `-g '!A' -g B -g '!C'` does. This is in fact partially why the `-g/--glob` flag provides the facilities to both whitelist and blacklist (via a `!` prefix on the glob) in the same flag, because then I can easily retain the order of the flag values given.

> In ripgrep's help message for --type states, "**Only** search files matching TYPE" (emphasis mine). In which case, traditional overrides work perfectly here. However, the "do what I expect not what I say" in me says, "I want to search everything, including 'foo' files, but I still want 'bar' files to be skipped." (more traditional --include/--exclude).

I think whatever the docs say today was my attempt at documenting ripgrep's behavior, and not necessarily the ideal behavior. I'm pretty sure I was aware of this issue when I added the `-t/--type` and `-T/--type-not` flags, and it in particular manifests itself here:

https://github.com/BurntSushi/ripgrep/blob/361698b90a6b31feac78ac2f16082342f750a1e1/src/args.rs#L873-L878

You can see for example that we add all positive selections followed by all negative selections. This means negative selections will always override positive selections, regardless of where they come. This is likely the source of this specific bug reported. We could swap the positive/negative and that would fix this particular bug, but it would just create another one (negative overrides that came after positive overrides would not override the positives).

The docs on the `-t/--type` and `-T/--type-not` flags are quite ambiguous and don't mention any precedence rules between them. I wonder if I did that on purpose. \~\_\~

> As there may be times I do want to be able to say, "ripgrep, ONLY search this one file type" which wouldn't be possible with traditional includes/excludes. So I see them (current type system with traditional include/exclude) as not mutually exclusive ideas.

That is interesting. I might argue that this would be better solve by dropping your default ignore rules into a `.ignore` file, and reserving the CLI for overrides.

> From a implementation standpoint traditional include/excludes are much harder to implement than just, "--type and --type-not override each other" as it would have to resolve only matching types in an overriding fashion.

I'm not sure I quite grok this, but from ripgrep's perspective, basically what I'd need is the ability to couple two or more flags together, and then have a way to ask clap for the list of values given to any of those flags in the order they were received. Each value I get would then need to be tagged with its corresponding flag (whether that's an associative list or corresponding lists or whatever doesn't really matter). That would be enough information on my end to load them into globsets, which will handle the actual precedence rules.

> i.e. assume we had an argv (after config parse) that was roughly $ rg --exclude=*.foo --exclude=*.bar --include=*.foo 'test' After overrides_eq_vals_with it would become $ rg --exclude=*.bar --include=*.foo 'test'.

> The other way would involve something I've been thinking about, which is to provide companions to the validators, namely the ability to provide functions to Filter/Skip, Map, and Action arguments mid parse. These would do things such as do something potentially impure (Action), allow one to filter/skip values (Filter/Skip haven't decide on naming yet), or transform actual values themselves (Map) to be saved in the matches struct. These would all have access to the matches struct as it exists at that time, in order to perform decisions or possibly even mutate. I should say though that this method is further off and would not be available until v3 at the earliest (if ever, pending design and implementation, etc.).

What do you think of my idea above in lieu of this? Is it simpler to implement? It definitely implies new API items to `ArgMatches` though.

---

_Comment by @BurntSushi on 2018-02-16 12:21_

@pikeas As a note, you can work around this by not putting `--type` or `--type-not` flags in your config file. Instead, put them in a `.ignore` file and load it with the `--ignore-file` flag (or drop it in your `$HOME` directory).

---

_Comment by @BurntSushi on 2018-02-16 12:26_

Also, @kbknapp, I admire your zeal. :) ripgrep has had this bug since day 1 (even in the docopt days), so there is no real urgency to get this fixed, and there are usually pretty easy workarounds on the user's end although it is somewhat exacerbated by the config file feature.

With enough effort, I could probably fix this on ripgrep's end with some light parsing of the argv itself. I would only need to look at flags I think. For example, given:

```
--include A --exclude B --exclude C --include D
```

I could pretty easily get a list of `[--include, --exclude, --exclude, --include]` from argv itself. If I combine that with the lists that clap will give me, `[A, D]` and `[B, C]`, then it seems like it should be straight-forward to produce the desired list which is `[A, B, C, D]`. Does that make sense?

---

_Comment by @kbknapp on 2018-02-16 21:44_

I actually wrote up two versions of my comment above, and deleted the original because I thought the suggestion was too much work on your end (which I'm still slightly inclined to believe), but it entailed being able to query new information from clap about the position an argument came in. 

At first glance it appeared to me like it'd be a quick win and easy to implement this feature (include/exclude) with this new information. And since clap already knows all this info, its super simple for me to add on that side of things. It's also quite general purpose because it doesn't assume a particular use case (like `Arg::overrides_eq_vals_with` would), so I like that aspect of it. But the more I dug into it, the problem is it still requires a lot of "jumping through hoops" to solve the problem (on the end application side).

An example would look something like this:

* Assume after config files we have an argv something like: `rg --include A --exclude B --exclude C --include B`

Which clap reports as `include=["A", "B"], exclude=["B", "C"]`. However, from only clap's reporting it could still actually be any of these permutations (I'm using `i` for include and `e` for exclude for brevity) that was really used:

| argv | Actually Searched |
| --- | --- |
| `iA iB eB eC` | A |
| `eB eC iA iB` | A, B |
| `iA eB eC iB` | A, B |
| `iA eB iB eC` | A, B |
| `eB iA iB eC` | A, B | 
| `eB iA eC iB` | A, B |

With just two overlapping values this is simple, but even so based off just what clap reports we can't actually know what the user wanted because there's two truths.

So back to including position information which would look something like this (using that original argv as an example).

```rust
matches.positions_of("include"); // [1, 4]
matches.positions_of("exclude"); // [2, 3]

matches.position_of("include"); // [1]
matches.position_of("exclude"); // [2]

matches.values_of("include").unwrap().with_positions(); // An iterator that provides [(1, A), (4, B)]
matches.values_of("exclude").unwrap().with_positions(); // An iterator that provides [(2, B), (3, C)]
```

That could be used to get a final list for your globset via

```rust
// Collect up chained values
let mut combined = matches.values_of("include").unwrap()
    .with_positions()
    .chain(matches.values_of("exclude").unwrap()
        .with_positions()
   .collect();

// Sorting by position
combined.sort_by_key(|tup| tup.0);

// Remove "overrides"
let mut overrides = vec![];
for (i, a) in combined.iter().enumerate() {
    if combined[i+1..].iter().any(|tup| tup.1 == a.1) {
        overrides.push(i);
    }
}
for &i in overrides.iter().rev() {
    combined.swap_remove(i);
}

// swap back out into separate fields
let mut includes = Vec::new();
let mut excludes = Vec::new();

let inc_p: Vec<_> = matches.positions_of("include").unwrap().collect();
for (p, val) in combined.into_iter() {
    if inc_p.contains(p) {
        includes.push(val);
    } else {
        excludes.push(val);
    }
}

// From our original argv, we now have
// include = ["A", "B"]
// exclude = ["C"]
```

I'm sure there's far more clean ways to do this (and more efficient), but this was just off the top of my head.

---

_Comment by @kbknapp on 2018-02-16 21:49_

Since the `with_positions` would just be a normal Rust iterator where `Item` is a tuple of a position and a value, I wonder if there is an `itertools` function to combine the two (since includes and excludes are separate iterators) removing duplicate values? Dunno, just a thought.

---

_Label `enhancement` added by @BurntSushi on 2018-02-20 12:41_

---

_Renamed from "Include/exclude precedence?" to "add --include/--exclude flags, with correct precedence" by @BurntSushi on 2019-09-10 21:18_

---

_Comment by @ilyagr on 2021-03-12 03:16_

I think there's no need for quite so much custom argv parsing, at least not anymore. `clap` now has [`indices_of`](https://docs.rs/clap/2.33.3/clap/struct.ArgMatches.html#method.indices_of) which should do the job: it will give positions of repeated arguments ~~together with their values~~ (on second look, they would have to be manually zipped together with the values). So, it should be possible to call this for `--type` and `--type-not` (or `--include` and `--exclude`) and then merge the results in the correct order. I also found a pre-made [function](https://docs.rs/itertools/0.10.0/itertools/trait.Itertools.html#method.kmerge_by) that would do the merging ([see also](https://docs.rs/itertools/0.10.0/itertools/fn.kmerge_by.html)), just as @kbknapp suggested. It's in the `itertools` crate. `ripgrep` currently doesn't seem to use that crate, so perhaps it's better to re-implement the function.

I think that, as this is done, `--type` should also become an alias for the corresponding `--glob`s.  This has the potential to make the `ignore` crate simpler. More importantly, it would make the mental model of how all of these flags interact simpler. I wrote more about this in my other proposal [of making `--glob` respect ignore files](https://github.com/BurntSushi/ripgrep/issues/1808#issuecomment-794839522). So we'd actually be merging together the values for `--type`, `--type-not`, `--glob`, and probably `--iglob` all together, keeping track of the exact order.

I *think* that with these two proposals, the user could accomplish most things they might want to do with `--include` and `--exclude`. They would either use `--glob`/`--type`/`--type-not` or, if they *really* wanted to include something, they would add more positional arguments.

---

_Comment by @Tal500 on 2024-01-15 09:18_

> I think there's no need for quite so much custom argv parsing, at least not anymore. `clap` now has [`indices_of`](https://docs.rs/clap/2.33.3/clap/struct.ArgMatches.html#method.indices_of) which should do the job: it will give positions of repeated arguments ~together with their values~ (on second look, they would have to be manually zipped together with the values). So, it should be possible to call this for `--type` and `--type-not` (or `--include` and `--exclude`) and then merge the results in the correct order. I also found a pre-made [function](https://docs.rs/itertools/0.10.0/itertools/trait.Itertools.html#method.kmerge_by) that would do the merging ([see also](https://docs.rs/itertools/0.10.0/itertools/fn.kmerge_by.html)), just as @kbknapp suggested. It's in the `itertools` crate. `ripgrep` currently doesn't seem to use that crate, so perhaps it's better to re-implement the function.
> 
> I think that, as this is done, `--type` should also become an alias for the corresponding `--glob`s. This has the potential to make the `ignore` crate simpler. More importantly, it would make the mental model of how all of these flags interact simpler. I wrote more about this in my other proposal [of making `--glob` respect ignore files](https://github.com/BurntSushi/ripgrep/issues/1808#issuecomment-794839522). So we'd actually be merging together the values for `--type`, `--type-not`, `--glob`, and probably `--iglob` all together, keeping track of the exact order.
> 
> I _think_ that with these two proposals, the user could accomplish most things they might want to do with `--include` and `--exclude`. They would either use `--glob`/`--type`/`--type-not` or, if they _really_ wanted to include something, they would add more positional arguments.

Turns out that clap now was archived in Github. Any direction for now?

---

_Comment by @BurntSushi on 2024-01-15 14:24_

[clap is not archived](https://github.com/clap-rs/clap) and as of ripgrep 14, [ripgrep no longer uses clap](https://github.com/BurntSushi/ripgrep/commit/082245dadb3854238e62b91aa95a46018cf16ef1).

---

_Comment by @Tal500 on 2024-01-16 12:49_

> [clap is not archived](https://github.com/clap-rs/clap)

Sorry, my bad. I was confuse with an older Github repo: https://github.com/epage/clapng

> and as of ripgrep 14, [ripgrep no longer uses clap](https://github.com/BurntSushi/ripgrep/commit/082245dadb3854238e62b91aa95a46018cf16ef1).

So it means that the blocker for this issue is irrelevant, and this issue may be solved?


---

_Comment by @BurntSushi on 2024-01-16 12:57_

From a narrow technical perspective, sure, it is now very easy to implement `--include/--exclude` semantics in ripgrep.

But, I am currently in the (long) process of overhauling how globbing is implemented. And then I plan to move into figuring out how to make ripgrep's filtering story crisper. A sizeable fraction of extant open issues are related to filtering in one form or another. So I'd rather add these flags as part of figuring out the grander filtering story.

---

_Comment by @Tal500 on 2024-01-16 13:24_

> From a narrow technical perspective, sure, it is now very easy to implement `--include/--exclude` semantics in ripgrep.
> 
> But, I am currently in the (long) process of overhauling how globbing is implemented. And then I plan to move into figuring out how to make ripgrep's filtering story crisper. A sizeable fraction of extant open issues are related to filtering in one form or another. So I'd rather add these flags as part of figuring out the grander filtering story.

thanks

---

_Comment by @rcfox on 2025-07-17 13:55_

I found that upgrading from ripgrep 13 to 14 resolved the original issue of an earlier -T overriding -t.

---

_Comment by @BurntSushi on 2025-07-17 14:08_

@rcfox Yeah that's right. ripgrep 14 did a complete rewrite of its CLI argument parsing.

---
