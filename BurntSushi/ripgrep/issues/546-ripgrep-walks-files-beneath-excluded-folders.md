```yaml
number: 546
title: Ripgrep walks files beneath excluded folders
type: issue
state: closed
author: roblourens
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2017-07-07T22:46:39Z
updated_at: 2018-08-22T12:54:38Z
url: https://github.com/BurntSushi/ripgrep/issues/546
synced_at: 2026-01-12T16:13:22Z
```

# Ripgrep walks files beneath excluded folders

---

_@roblourens_

It seems like ripgrep could be more aggressive in not walking folders that won't have any included files. I'm not sure whether this is just an optimization that isn't implemented, or if there's a scenario I'm missing where this would be necessary.

Here I have
```
foo/
  - a.txt
  - b.txt
bar/
  - a.txt
  - b.txt
```

I search to include only /foo, but ripgrep walks everything under `bar/`. 

```
robmac:rgtest roblou$ rg --files -g '/foo/**' --debug
DEBUG:globset: glob converted to regex: Glob { glob: "foo/**/*", re: "(?-u)^foo(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('f'), Literal('o'), Literal('o'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG:grep::search: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 0 regexes
DEBUG:ignore::walk: ignoring ./bar/a.txt: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: ignoring ./bar/b.txt: Ignore(IgnoreMatch(Override(Glob(UnmatchedIgnore))))
DEBUG:ignore::walk: whitelisting ./foo/a.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/foo/**", actual: "foo/**/*", is_whitelist: false, is_only_dir: false })))))
DEBUG:ignore::walk: whitelisting ./foo/b.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/foo/**", actual: "foo/**/*", is_whitelist: false, is_only_dir: false })))))
foo/a.txt
foo/b.txt
```

Is there a way to write the glob such that ripgrep would know that it can skip `bar/` entirely?

---

_Comment by @roblourens on 2017-07-19 05:08_

Ping @BurntSushi in case you missed this one?

---

_Comment by @BurntSushi on 2017-07-19 11:36_

As you say, I think this is "just" an optimization that ripgrep doesn't do. It might be tricky to do this in the general case. Basically, the `/foo/**` glob doesn't actually match `foo` itself, only whatever is inside of it. So you have to descend into every top-level directory *in the general case* to determine matches. [This conditional check](https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/overrides.rs#L101-L103) is important:

```rust
        if mat.is_none() && self.num_whitelists() > 0 && !is_dir {
            return Match::Ignore(Glob::unmatched());
        }
```

What this is saying is that if there is no match (`/foo/**` does not match `./bar`) and there is at least one whitelisted glob (there is) *and* it's not a directory, then we can safely ignore it. But since `./bar` is a directory, we have to return the non-match, which means the path isn't skipped.

One thing you could do is start with "ignore everything" and then specifically whitelist the thing you want to descend. You can do this because, fundamentally, the `-g` flag is essentially a command line interface to specifying adhoc `.ignore` rules. The only difference is the matching behavior I outlined above and the fact that it is inverted (`-g` whitelists where as `-g '!glob'` blacklists). So you might try this:

```
$ rg --files -g '!*' -g '/foo/**'
```

But this does not work. This causes `./foo` to be ignored because `/foo/**` does not actually match `./foo`---it only matches whatever is inside of it. So all you need to do here is just whitelist `/foo` specifically:

```
$ rg --files -g '!*' -g '/foo' -g '/foo/**' --debug
DEBUG:globset: glob converted to regex: Glob { glob: "**/*", re: "(?-u)^(?:/?|.*/).*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "foo/**/*", re: "(?-u)^foo(?:/|/.*/)[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([Literal('f'), Literal('o'), Literal('o'), RecursiveZeroOrMore, ZeroOrMore]) }
DEBUG:globset: built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 2 regexes
DEBUG:grep::search: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
DEBUG:ignore::walk: ignoring ./bar: Ignore(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "!*", actual: "**/*", is_whitelist: true, is_only_dir: false })))))
DEBUG:ignore::walk: whitelisting ./foo: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/foo", actual: "foo", is_whitelist: false, is_only_dir: false })))))
DEBUG:ignore::walk: whitelisting ./foo/b.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/foo/**", actual: "foo/**/*", is_whitelist: false, is_only_dir: false })))))
DEBUG:ignore::walk: whitelisting ./foo/a.txt: Whitelist(IgnoreMatch(Override(Glob(Matched(Glob { from: None, original: "/foo/**", actual: "foo/**/*", is_whitelist: false, is_only_dir: false })))))
```

And you can see here that `./bar` is ignored and isn't descended into.

I don't know whether this specific work-around will suit your needs. But you can specify multiple whitelists, so it at least generalizes in that sense. I agree it would probably be better if ripgrep could just do this automatically. But the semantics are tricky and it requires more thought. In this case, you probably need something like "is it possible for this glob to ever match a path below this directory," which in turn seems to imply deeper analysis into the glob.

---

_Comment by @roblourens on 2017-07-19 20:40_

Thanks, that makes sense. That workaround is a good idea. I'm working around it by splitting out the path portion and passing that to ripgrep as a path to search. And yeah, what I'm asking for would require some understanding of what the glob does and prediction of what will match.

---

_Label `enhancement` added by @BurntSushi on 2017-07-19 20:58_

---

_Label `icebox` added by @BurntSushi on 2017-07-19 20:58_

---

_Comment by @BurntSushi on 2018-08-22 12:54_

I'm going to close this out because I don't anticipate working on this any time soon, and I'm not sure whether it's actually worth doing considering the code complexity that is involved here. If and when the directory traversal gets a face lift, I'll try to revisit whether an optimization like this is possible to do.

---

_Closed by @BurntSushi on 2018-08-22 12:54_

---
