```yaml
number: 1017
title: "libripgrep: PCRE2 support, multiline search, JSON output and more"
type: pull_request
state: merged
author: BurntSushi
labels:
  - libripgrep
assignees: []
merged: true
base: master
head: ag/libripgrep
created_at: 2018-08-19T14:29:38Z
updated_at: 2018-08-30T20:22:34Z
url: https://github.com/BurntSushi/ripgrep/pull/1017
synced_at: 2026-01-12T18:23:13Z
```

# libripgrep: PCRE2 support, multiline search, JSON output and more

---

_@BurntSushi_

After a lot of work, libripgrep is finally read to be merged to master. This includes planned features such as multiline search and JSON output, but also a surprise: ripgrep now provides the ability to opt into using PCRE2 as the regex engine instead of Rust's default regex engine. This provides a way to use look-around and backreferences with ripgrep. This support should work on Windows, macOS and Linux.

libripgrep still needs high level documentation in the form of a cookbook and a guide, so it is not quite ready for wide use yet. However, the API itself is minimally documented, so ambitious individuals should feel empowered to try it out. :-)

Closes #162 (libripgrep)
Closes #176 (multiline search)
Closes #188 (opt-in PCRE2 support)
Closes #244 (JSON output)
Closes #416 (Windows CRLF support)
Closes #917 (trim prefix whitespace)
Closes #993 (add --null-data flag)
Closes #997 (--passthru works with --replace)

Fixes #2 (memory maps and context handling work)
Fixes #200 (ripgrep stops when pipe is closed)
Fixes #389 (more intuitive `-w/--word-regexp`)
Fixes #643 (detection of stdin on Windows is better)
Fixes #441, Fixes #690, Fixes #980 (empty matching lines are weird)
Fixes #764 (coalesce color escapes)
Fixes #922 (memory maps failing is no big deal)
Fixes #937 (color escapes no longer used for empty matches)
Fixes #940 (--passthru does not impact exit status)
Fixes #1013 (show runtime CPU features in --version output)

cc @roblourens

---

_Label `libripgrep` added by @BurntSushi on 2018-08-19 14:29_

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-08-19 14:29_

---

_Comment by @okdana on 2018-08-19 15:55_

This is huge and amazing, thank you!

I was wondering whether a `--no-multiline-dotall` would be appropriate? Obviously people can just flip it off with `(?-s)`, but they can do that with `(?s)` too, so maybe for UI consistency/discoverability it'd makes sense...?

I also had some feedback on the zsh completion — mostly just nit-picking, but a few more functional things (like exclusivity errors). Rather than flood you with a bunch of comments, i think i'll prepare a patch that can be applied against your branch.

One particular issue i do see with the completion is that, since `--pcre2-unicode` is enabled by default, `--no-pcre2-unicode` doesn't really qualify for `$no` — it should actually be the other way around. The problem with that (maybe one you ran into?) is that `--no-pcre2-unicode` doesn't have its own entry in the usage help, so the CI script won't deal with it properly.

We could work around that by giving `--no-pcre2-unicode` the 'canonical' spot in the usage help (and that might be more consistent with the way some of the other options are documented, tbh), but i think it's probably just time to make that script also extract long options from the option-description bodies. I'll do that.

Let me know how you feel about `--no-multiline-dotall` and i'll incorporate that into my changes.

I only looked at a few other things but so far everything else works great for me.

---

_Comment by @okdana on 2018-08-19 18:24_

Something like this?

https://github.com/BurntSushi/ripgrep/compare/ag/libripgrep...okdana:dana/libripgrep?expand=1

([raw diff](https://github.com/BurntSushi/ripgrep/compare/ag/libripgrep...okdana:dana/libripgrep.diff), [raw diff minus `--no-multiline-dotall`](https://github.com/BurntSushi/ripgrep/compare/ag/libripgrep...okdana:dana/libripgrep~1.diff))

* Fixed several wording-consistency and exclusion issues in the completion function. I was able to remove a lot of the exclusions surrounding `-c`, `-o`, `-r`, and `--passthru` thanks to these changes and some earlier ones that i hadn't accounted for. Other stuff is probably self-explanatory, but let me know if you disagree with any of it obv

  Technically the JSON options should be exclusive with all of the other output-related options, but that gets a bit messy, so i've just left it alone for now

* Updated `test_complete.sh` to search for long options pretty much anywhere in the usage help. It's hard to make this work perfectly without some way to semantically mark up `rg`'s own options; for example it matches on the reference to `find --print0`, so i had to manually exclude that. But i don't think it'll come up often

  One thing that a few tools do is provide a machine-readable method of listing all of their options. Symfony Console applications do this, and `clang` even has a fancy `--autocomplete` which does the work for their bash completion function. I wonder if Clap can be made to do something similar? (If it can, is it worth it? Not sure)

* When i was double-checking the overrides in `app.rs` i noticed some typos; i just fixed those

* Anticipating that you might want it, i added `--no-multiline-dotall` — but it's a separate commit, so if you don't like it you can just leave it out

PS: Treating passed-through lines as context is so much smarter than the old method (and totally obvious in hind sight), i like that a lot

---

_Comment by @roblourens on 2018-08-19 18:36_

This is awesome. I know a lot of people will be very happy about being able to search with PCRE2 in vscode!

---

_Comment by @BurntSushi on 2018-08-19 23:53_

Thanks for the comments @okdana!

> I was wondering whether a `--no-multiline-dotall` would be appropriate?

So my thinking here is that `--multiline-dotall` probably isn't a flag folks are going to type often. If someone wants that behavior "on demand," then I think they're more likely to just add the inline `s` flag. Instead, I expect that flag would be added in an alias or a config, and when one wants to disable it, I'd expect folks to use the inline `s` flag for that too, instead of typing out `--no-multiline-dotall`.

But, it doesn't cost much to add the flag and maybe it's worth it for consistency reasons. We don't yet do it for every flag though I don't think, but it's probably pretty close.

> i think i'll prepare a patch that can be applied against your branch.

That's great, thanks! Would it work for you if I merged this PR and then you just submit the patch against master? I'm not planning on doing a release imminently or anything.

Also, in general, I've found it pretty difficult to update the completion script. I knew I probably got some stuff wrong and I'm grateful you've caught it. :-) I can't tell whether a little bit of documentation describing the syntax would fix my problem editing the completion rules, or if it would be better to try to generate the completion script like we generate the man page (potentially by adding addition metadata on `RGArg`).

> One particular issue i do see with the completion is that, since --pcre2-unicode is enabled by default, --no-pcre2-unicode doesn't really qualify for $no — it should actually be the other way around. The problem with that (maybe one you ran into?) is that --no-pcre2-unicode doesn't have its own entry in the usage help, so the CI script won't deal with it properly.

> We could work around that by giving --no-pcre2-unicode the 'canonical' spot in the usage help (and that might be more consistent with the way some of the other options are documented, tbh), but i think it's probably just time to make that script also extract long options from the option-description bodies. I'll do that.

Yeah, I'm not sure what I want to do here. You're right that making `--no-pcre2-unicode` be the "canonical" flag would fix this issue and would also be more consistent with how we do other things. My only rationale for doing it the way I did was because this way, `--pcre2-unicode` will show up right next to `--pcre2` when sorting flags alphabetically, which I think is nice. But maybe the fix there is to group options logically instead of alphabetically. Gah.

> One thing that a few tools do is provide a machine-readable method of listing all of their options. Symfony Console applications do this, and clang even has a fancy --autocomplete which does the work for their bash completion function. I wonder if Clap can be made to do something similar? (If it can, is it worth it? Not sure)

ripgrep does kind of have at least the _guts_ to make this happen now I think. This was in large part motivated by my desire to generate a quality man page directly from the flag definitions. This is why `RGArg` exists, which is basically a light layer right above clap that ripgrep can read at build time and do stuff with. If we could generate a completion script from that, I think that would be the best of all worlds. Even if `RGArg` as it exists today isn't enough, I think it is more than OK to add additional metadata just for generating completions such that flag definitions might need to be modified to add it. This also increases the locality trend, such that if you add a new flag to ripgrep, there are fewer places you need to edit to add it. (Today, there are two places: the definition itself and the zsh completion script.)

> PS: Treating passed-through lines as context is so much smarter than the old method (and totally obvious in hind sight), i like that a lot

Hah yeah. It also fixes the exit status bug nicely. It would not have been feasible to do in the old ripgrep code though!

@okdana Your patch LGTM, thank you. :-) I'm fine with taking in `--no-multiline-dotall`.

---

_Comment by @jessegrosjean on 2018-08-20 00:39_

Nice work and big thanks from me too! I've been using libripgrep in my own app, results are much better then I what I would have come up with by myself. It's also introduced me to the world of rust and having full control over memory and everything else. It's been and adventure, mostly good :).

---

_Comment by @okdana on 2018-08-20 06:44_

>That's great, thanks! Would it work for you if I merged this PR and then you just submit the patch against master?

Yeah, i can do that.

>Also, in general, I've found it pretty difficult to update the completion script. I knew I probably got some stuff wrong and I'm grateful you've caught it. :-) I can't tell whether a little bit of documentation describing the syntax would fix my problem editing the completion rules, or if it would be better to try to generate the completion script like we generate the man page (potentially by adding addition metadata on RGArg).

I worry that, unless it was extremely comprehensive, any automatic generation done by ripgrep would have similar limitations to the ones Clap had. Obviously it can be done, but i don't know, i have mixed feelings i guess.

Short of that, maybe it would help if i put together a little Markdown guide to explain in simple terms how the option specifications work, and what the conventions are? I think i could make something that's easier to follow than the official documentation, at least for the sub-set of features that most of our specs use. Then we could put that in the repo and link to it at the top of the function.


---

_Comment by @BurntSushi on 2018-08-20 11:10_

@okdana A small guide would be great! I think it would be nice to just inline it into a comment in the completion script though, just so that it's all in one place.

> I worry that, unless it was extremely comprehensive, any automatic generation done by ripgrep would have similar limitations to the ones Clap had. Obviously it can be done, but i don't know, i have mixed feelings i guess.

I'm not too familiar with the limitations that Clap had, but yeah, that's why I would expect we'd need to add additional structure to `RGArg` to make auto-generation as good as the manually curated script.

Either way, I suspect a mini-guide describing how the completion script works is an excellent next step. Thank you. :-)

---

_Merged by @BurntSushi on 2018-08-20 11:10_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

_Branch deleted on 2018-08-20 11:10_

---

_Comment by @kankri on 2018-08-30 20:06_

This is a bit off topic...

> Also, in general, I've found it pretty difficult to update the completion script. I knew I probably got some stuff wrong and I'm grateful you've caught it. :-) I can't tell whether a little bit of documentation describing the syntax would fix my problem editing the completion rules, or if it would be better to try to generate the completion script like we generate the man page (potentially by adding addition metadata on RGArg).

I'm surprised we are still using custom completion scripts for each command and each shell... Back when Bash didn't yet have support for user completion rules I had my own Bash fork which gave completions *and option help* when I typed `foo --bar<TAB>` by running `foo --help` and parsing and filtering the output. It worked great but I haven't bothered to keep maintaining it, most of the time I don't want to complete options, I just abbreviate them to keep command lines short.

To make things a bit less heuristic, each command could support some option, e.g. `foo --command-syntax-description`, which would output the option descriptions in a more machine readable form (JSON, YAML, ...). Or the description could be delivered as a separate file along with the binary and manual pages.

Maybe the Rust community can get a convention started?


---

_Comment by @BurntSushi on 2018-08-30 20:21_

@kankri I think it would be better if you sought out the Rust CLI working group: https://github.com/rust-lang-nursery/cli-wg There hasn't been too much discussion on completions there, so you might be able to get the ball rolling! https://github.com/rust-lang-nursery/cli-wg/search?q=completions&type=Issues

I personally don't have any plans to output ripgrep's options in a machine readable format. I do still think it's possible to generate completions with the same level of quality as our current manually curated completions, but this will probably require attaching more metadata to each flag, so it requires some work and I haven't thought through it completely yet. @okdana's excellent docs have honestly reduced my desire for this since I think I can now competently edit the completion file.

Otherwise, I don't really see any good reason to be "surprised." It's not surprising at all that good context sensitive completions require a lot more knowledge than what you typically see in a command's `--help` output. Certainly, I am [no stranger to that strategy](https://github.com/docopt/docopt.rs/blob/master/completions/docopt-wordlist.bash).

---
