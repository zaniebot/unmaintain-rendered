```yaml
number: 504
title: "[request] More built-in types (globs)"
type: issue
state: closed
author: phrohdoh
labels: []
assignees: []
created_at: 2017-06-06T15:44:10Z
updated_at: 2017-06-06T22:08:43Z
url: https://github.com/BurntSushi/ripgrep/issues/504
synced_at: 2026-01-12T16:13:22Z
```

# [request] More built-in types (globs)

---

_@phrohdoh_

I'd like to have (and possibly implement myself, I can't image this is too difficult):

```
$ rg -t cshtml
$ rg -t javascript
```

I say `javascript` because `cs` has a matching `csharp` but `js` is the only javascript option.

These could of course be added in a shell alias with `--type-add` but then I also need to alias in a `-tcshtml -tjavascript $@`.

---

_Comment by @BurntSushi on 2017-06-06 15:50_

Sorry, but I don't understand your feature request. Could you please rephrase and show an example? I also don't understand what the problem with `--type-add` is?

---

_Comment by @phrohdoh on 2017-06-06 15:52_

ripgrep has types [`cs`, `csharp`](https://github.com/BurntSushi/ripgrep/blob/cd6c54f5f4ec6882d5ee3f8c5f0ce74e025477af/ignore/src/types.rs#L117-L118), and [`js`](https://github.com/BurntSushi/ripgrep/blob/cd6c54f5f4ec6882d5ee3f8c5f0ce74e025477af/ignore/src/types.rs#L140-L142) but no `javascript`.

So I'm requesting a `javascript` be added for the reason above (cs + csharp, js + javascript), and also asking for `cshtml` (because I do a lot of ASP.NET work and often want to do `rg -t csharp -t cshtml pattern`).

`cshtml` would be `("cshtml", &["*.cshtml"]),`.

Does this make sense?

---

_Comment by @BurntSushi on 2017-06-06 16:00_

I'm still not clear. Are you asking for two completely different things? You want a `javascript` type that is an alias of `js`... for what reason? Some types have aliases, but it's probably because `csharp` was added first and someone wanted something shorter.

It's hard to understand your other request. You're asking for `cshtml` but your example is `rg -t csharp -t cshtml pattern` which doesn't make sense to me, since you're including the thing you want in its definition? Did you mean `rg -t csharp -t html pattern`?

Have you by any chance looked at the document for `--type-add`? You can include the rules of other types. For example:

```
$ rg --type-add 'cshtml:include:csharp,html' -tcshtml pattern
```

---

_Comment by @phrohdoh on 2017-06-06 16:09_

`javascript` as an alias for `js` would address an inconsistency with `cs` and `csharp` existing but not `javascript`.

If ripgrep is to favor shorter aliases then `csharp` should probably be removed because, IME, once people see `csharp` they expect other long forms to exist and get tripped up when they don't.

> Are you asking for two completely different things?

Yes: Adding `javascript` and `cshtml` types.

> Did you mean rg -t csharp -t html pattern?

No, I am not asking for `-t html` but `-t cshtml` which, to use with the `-t` flag, has to be built into ripgrep.

Yes I know about `--type-add` and even mentioned it in the original description but would rather my search tool had this built in so I don't have to type out long commands.

I could of course write an alias that is:
`export rg="rg --type-add 'cshtml:*.cshtml' --type-add 'javascript:*.js'"`

But then to actually use those I have to remember to use `-tjavascript` or `-tcshtml` instead of `-t cshtml`, for example.

I don't know how else to word this. If this isn't desired then that's fine I understand not wanting to throw everything into ripgrep's type list.

If necessary I can jump onto IRC later today.

---

_Comment by @BurntSushi on 2017-06-06 16:12_

Could you please tell me exactly what you expect `-tcshtml` to do?

> If ripgrep is to favor shorter aliases then csharp should probably be removed because, IME, once people see csharp they expect other long forms to exist and get tripped up when they don't.

No, I'm not removing anything. It's an unnecessary breaking change. I'd rather deal with the wart.

---

_Comment by @phrohdoh on 2017-06-06 16:14_

`rg -t cshtml foo` would search all `*.cshtml` files for the `foo` pattern similar to how `rg -t cs foo` would search all `*.cs` files for `foo`.

---

_Comment by @BurntSushi on 2017-06-06 16:15_

OK, but you don't need to create an `rg-cshtml` alias for that. You can just create an `rg` alias:

```
$ alias rg="rg --type-add 'cshtml:*.cshtml'"
$ rg -tcshtml foo
... it works
```

---

_Comment by @phrohdoh on 2017-06-06 16:19_

Indeed but having `cshtml` built in to the type list means I can use `rg -t cshtml` (note the space) which is the same syntax as the existing types so I do not need to remember that I have aliases `rg` and therefore use `-tcshtml` instead of `-t cshtml`.

You seem quite opposed to adding `cshtml` but not `svg` (https://github.com/BurntSushi/ripgrep/issues/394#issuecomment-285808790) and I do not understand why.

If it won't happen then let's close this ticket and end the circular discussion.

---

_Comment by @BurntSushi on 2017-06-06 16:35_

@Phrohdoh Apologies if you think this is a circular discussion, but I am *trying to understand what your request is*. I didn't know what "cshtml" was and it wasn't at all clear from your comments. It wasn't until https://github.com/BurntSushi/ripgrep/issues/504#issuecomment-306537545 that I understood that `cshtml` meant `*.cshtml` and not `*.{cs,html}`. If it's the latter, then I hope you can understand why I was trying to point out that `--type-add` can be used to define a new type that includes other, pre-existing types. I haven't said *anything* about my willingness to add a new file type, but I am only just now realizing that's what you're asking for! 

I see you've edited some of your comments (with respect to what I saw in my email) that do now indeed contain important clarifications. :-)

> Indeed but having cshtml built in to the type list means I can use `rg -t cshtml` (note the space) which is the same syntax as the existing types so I do not need to remember that I have aliases rg and therefore use `-tcshtml` instead of `-t cshtml`.

Sorry, but I still don't understand this? I can't reproduce your space problem. Aliases work fine and you don't need to remember any specific alias:

```
$ alias rg="rg --type-add 'cshtml:*.cshtml'"
$ rg test
foo.cshtml
1:test

foo.html
1:test
$ rg -tcshtml test
foo.cshtml
1:test
$ rg -t cshtml test
foo.cshtml
1:test
```

---

If you want to add a new file type, then PRs against [`types.rs`](https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/types.rs) are welcome. If you look carefully at the existing list, you'll see that things like `*.ejs` are included in the `html` file type, and similarly, `*.jsx` is included in the `js` file type. It's impossible for me personally to know what the right classification is for every file type, so I've mostly just been relying on other folks to make the right decisions for me. I don't know whether `*.cshtml` belongs with `html` or `js` or in its own new file type `cshtml`, so I defer to your judgment on that. But yes, of course, `*.cshtml` certainly belongs somewhere in the file type list given that I now know it is its own distinct file extension used by lots of folks. (That was *not* clear to me.)

---

_Comment by @phrohdoh on 2017-06-06 16:42_

Okay I see the disconnect, sorry this is entirely my fault.

I did not realize that both `rg -tcshtml test` and `rg -t cshtml test` would work if `cshtml` was defined via `--type-add` (the help does not reflect this), that alone is enough to close this as solved.

I'll try to carve out some time later to open a PR.

As you noted I edited in more details as they came to me.

Apologies about the confusion!

---

_Comment by @phrohdoh on 2017-06-06 16:43_

The help for `--type-add` contains this:

```
            Example: rg --type-add 'foo:*.foo' -tfoo PATTERN.
```

Adding another example (such as the following) may help.

```
            Example: rg --type-add 'bar:*.bar' -t bar PATTERN.
```

---

_Closed by @BurntSushi on 2017-06-06 22:08_

---
