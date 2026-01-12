```yaml
number: 83
title: add a way for file types to include rules from other file types
type: issue
state: closed
author: SigmundVik
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2016-09-25T13:42:37Z
updated_at: 2017-01-07T23:15:06Z
url: https://github.com/BurntSushi/ripgrep/issues/83
synced_at: 2026-01-12T16:13:21Z
```

# add a way for file types to include rules from other file types

---

_@SigmundVik_

This would be useful for defining file types that are aggregates of other file types, for example:

```
--type-add src:cpp,py
```

would be equivalent of specifying:

```
--type-add src:*.C,*.H,*.cc,*.cpp,*.cxx,*.h,*.hh,*.hpp,*py
```


---

_Comment by @BurntSushi on 2016-09-25 14:02_

Would `--type-include 'src:cpp'` do it for you? (I think it'd be better to have separate flags for specifying globs and for subsuming other file types, to avoid trying to guess the user's intent.)


---

_Comment by @BurntSushi on 2016-09-25 14:04_

The argument against this feature is:
1. Specifying file types is already too complicated.
2. Specifying custom file types is probably done in an alias, so you only need to write them once. The advantages of having an include-type directive are therefore not as prescient.

A good counterpoint to (2) is that type inclusion means you always get updates in `ripgrep` automatically, but even that seems a touch weak to me.


---

_Comment by @SigmundVik on 2016-09-25 14:40_

I agree that a separate flag like `--type-include` would be more clear.

I do indeed specify my file types in an alias, but I would like the alias to be easy to read.  The curly brace glob syntax will help a lot with shortening my alias, so maybe the downsides of adding another flag outweigh the benefits in this case.


---

_Comment by @BurntSushi on 2016-09-25 15:07_

I do kind of come down on your side of things, honestly. The various `--type` flags need a bit more docs/examples though. (See #76)


---

_Comment by @BurntSushi on 2016-09-25 18:39_

One possible issue with adding this is that ripgrep's argv parser can't tell the position of each flag relative to one another. So for example, if you did this: `--type-add 'foo:*.foo' --type-include 'bar:foo' --type-add 'foo:*.wat'`, then it's not exactly clear what `bar` should contain. We can either process all includes after all additions, or all includes before any additions. Neither are necessarily intuitive.

We have the same problem with --type-clear, but I'm hesitant to make it worse until we either decide to live with it, or fix it. So I'm going to let this one linger for now.

The other potential relief is that I expect `rg` to gain support for an RC file or similar.


---

_Renamed from "It would be nice if file types somehow could refer to other file types" to "add a way for file types to include rules from other file types" by @BurntSushi on 2016-09-25 19:03_

---

_Label `enhancement` added by @BurntSushi on 2016-09-25 19:04_

---

_Label `question` added by @BurntSushi on 2016-09-25 19:04_

---

_Comment by @BurntSushi on 2016-11-19 14:53_

OK, I'd like to propose a specification for this so we can move forward. Here are a couple key design constraints that I've imposed:
1. Do not add any additional flags for the reasons pointed out above. Therefore, this feature has to be made available in an existing flag like `--type-add` (which is really the only sensible choice).
2. Do not break any existing uses.

OK, now for the spec. The documentation for `--type-add` should be modified to include:

`--type-add` can also be used to include rules from other types with the special `include` directive. The `include` directive permits specifying one or more other type names (separated by a comma) that have been defined and its rules will automatically be imported into the type specified. For example, to create a type called `src` that matches C++, Python and Markdown files, one can use:

```
--type-add 'src:include:cpp,py,md'
```

Additional glob rules can still be added to the `src` type by using the `--type-add` flag again:

```
--type-add 'src:include:cpp,py,md' --type-add 'src:*.foo'
```

Note that type names must consist only of Unicode letters. Punctuation characters are not allowed.

---

If someone has thoughts on the above or would like to implement it, I'd be happy to mentor it. I think it is a relatively isolated change and should probably only require modifying the implementation of [`TypesBuilder::add_def`](https://docs.rs/ignore/0.1.5/ignore/types/struct.TypesBuilder.html#method.add_def) in the [`ignore` crate](https://docs.rs/ignore).


---

_Label `help wanted` added by @BurntSushi on 2016-11-19 14:53_

---

_Label `question` removed by @BurntSushi on 2016-11-19 14:53_

---

_Comment by @SimenB on 2016-11-25 14:21_

Huge +1 for this! Just opened #252, my Github issue search-fu failed me (should have been able to ripgrep it 😉).

I've never touched Rust before, or I'd give this feature a go myself.

> The other potential relief is that I expect rg to gain support for an `RC` file or similar.

Great news!

---

_Comment by @isker on 2017-01-01 21:17_

Hi @BurntSushi, I'm taking a swing at this.  You say in your spec here:

> Note that type names must consist only of Unicode letters. Punctuation characters are not allowed.

but I think the only character disallowed presently by `TypesBuilder` for names is `:`.  Do we want to expand that as a part of this?  

---

_Comment by @BurntSushi on 2017-01-02 17:03_

@CannedYerins Thanks for taking a look at this!

And yes, I believe your observation is correct. We'll need to do a semver bump of the `ignore` crate, but that's OK.

---

_Comment by @BurntSushi on 2017-01-06 00:04_

@tankorsmash No, sorry. I feel that we've been getting along just fine so far with wrapper scripts and aliases. There is talk of `clap` (ripgrep's argv parser) potentially providing configuration automatically, but there's no ETA.

---

_Comment by @BurntSushi on 2017-01-06 00:05_

@tankorsmash Also, please take further discussion to the appropriate issue: #196. Let's not hijack this one!

---

_Comment by @tankorsmash on 2017-01-06 00:06_

Thanks, thought I looked hard enough, that's how I stumbled onto this one. I'll delete my comment to clear this up. 

---

_Comment by @BurntSushi on 2017-01-06 00:08_

@tankorsmash Deletion wasn't necessary, but thanks! :-)

---

_Closed by @BurntSushi on 2017-01-07 23:15_

---
