```yaml
number: 606
title: Filtering on the presence or absence of captures
type: issue
state: closed
author: chocolateboy
labels:
  - enhancement
  - help wanted
  - icebox
assignees: []
created_at: 2017-09-17T20:06:00Z
updated_at: 2021-10-02T12:25:31Z
url: https://github.com/BurntSushi/ripgrep/issues/606
synced_at: 2026-01-12T16:13:22Z
```

# Filtering on the presence or absence of captures

---

_@chocolateboy_

> 💡 [This trick] relies on your ability to inspect Group 1 captures (at least in the generic flavor), so it will not work in a non-programming environment, such as a text editor's search-and-replace function **or a grep command** -- [The Best Regex Trick](http://www.rexegg.com/regex-best-trick.html)

### TL;DR

Select all lines which match `\bTarzan\b` but not `"Tarzan"`:

    $ rg -w '"Tarzan"|(Tarzan)' --defined '$1'

AKA

    $ rg -w '"Tarzan"|(Tarzan)' -d '$1'

---

Suppose I want to select all lines which contain the unquoted word `Tarzan` i.e. `\bTarzan\b` but not `"Tarzan"` e.g. the first 4 lines of:

### test.txt

    "Tarzan and Jane"
    "Jane and Tarzan"
    Me Tarzan, you Jane
    Tarzan vs "Tarzan"
    This line doesn't mention him
    He's moved to Tarzania
    He's no "Tarzan"!

It can be done with a pipeline e.g.:

    $ rg -w 'Tarzan' test.txt | rg -v '"Tarzan"'

But that particular example rejects lines which contain both, which is not what we want in this case. The same would be true if ripgrep added e.g. an `-E` (`--no-regexp`) option to complement `-e`/`--regexp`:

    $ rg -we 'Tarzan' -E '"Tarzan"' test.txt

It can be done in one pass with PCRE-flavored greps such as GNU grep and ack, with varying degrees of [difficulty/unreadability](http://www.rexegg.com/regex-best-trick.html#typical), by using negative lookahead/look-behind assertions e.g.:

    $ grep -P '^(?:(?!"Tarzan"|Tarzan\w+)(Tarzan|.))+$' test.txt

That's already pretty gnarly for a single exclusion, and quickly becomes impractical/incomprehensible for multiple exclusions. It also matches lines which don't contain `Tarzan` and, again, excludes lines which contain both patterns.

In programming languages, there's a common pattern for performing exclusions in a simple, readable way without multiple passes:

1. match and discard the exclusions
2. match and capture the inclusion
3. test for its existence

e.g.:

#### JavaScript

```javascript
[ '', '"Tarzan"', 'Tarzan', 'Tarzania', 'Tarzan vs "Tarzan"' ].filter(it => {
    const m = it.match(/"Tarzan"|\b(Tarzan)\b/)
    return m && m[1]
}) // [ 'Tarzan', 'Tarzan vs "Tarzan"' ]
```
#### ES.next<sup>[[1]](https://github.com/tc39/proposal-optional-chaining#syntax)</sup>

```javascript
[ '', '"Tarzan"', 'Tarzan', 'Tarzania', 'Tarzan vs "Tarzan"' ].filter(it => {
    return it.match(/"Tarzan"|\b(Tarzan)\b/)?.[1]
}) // [ 'Tarzan', 'Tarzan vs "Tarzan"' ]
```

#### Ruby

```ruby
[ '', '"Tarzan"', 'Tarzan', 'Tarzania', 'Tarzan vs "Tarzan"' ].select { |it|
    it[/"Tarzan"|\b(Tarzan)\b/, 1]
} # => [ 'Tarzan', 'Tarzan vs "Tarzan"' ]
```

    $ ruby -ne 'print if $_[/"Tarzan"|\b(Tarzan)\b/, 1]' test.txt

etc.

This isn't available in any greps I'm aware of, but since the machinery is already there to capture and reference subexpressions by index and name, it seems like a small step to use them in predicates to reproduce the flexibility and simplicity of this pattern on the command line e.g.:

    $ rg -w '"Tarzan"|(Tarzan)' -d '$1' test.txt

#### output

```
"Tarzan and Jane"
"Jane and Tarzan"
Me Tarzan, you Jane
Tarzan vs "Tarzan"
```

### Notes

1\) I assume that the predicate can be inverted e.g.:

    $ rg --not-defined '$1'

AKA

    $ rg -D '$1'
    
There aren't many single-letter options left. The last remaining pairs are -`d`/`-D`, `-y`/`-Y` and `-z`/`-Z`. The latter are commonly used to denote null/zero values, so they could be used instead, with the meaning of `-d` and `-D` inverted e.g.:

    $ rg -z '$1' # AKA rg --not-defined '$1'
    $ rg -Z '$1' # AKA rg --defined '$1'

2\) I assume that indices increment across multiple patterns, and that multiple `-d` and `-D` options can be combined e.g.:

    $ rg -e 'Foo|(Bar)' -e '(Baz|(Quux))' -d '$1' -D '$3'

3\) I also assume that numbered and named captures can be mixed e.g.:

    $ rg -e 'Foo|(Bar)' -e 'Baz|(?P<name>Quux)' -d '$1' -D '$name'

4\) The full version of the matching command would currently be:

    rg '^.*?(?:"Tarzan"|\b(Tarzan)\b).*$' -d '$1' test.txt

Hopefully some of that boilerplate can be removed e.g. via #389 or #593.

5\) For clarity, `"Tarzan" vs Tarzan` is omitted from the examples. Handling it only slightly complicates the regex:

    $ rg '^(?:"Tarzan"|\b(Tarzan)\b|.)*$' -d '$1' test.txt

---

_Comment by @BurntSushi on 2017-09-24 12:50_

Thanks for this very thorough write up!

I kind of feel like that semantics of this are too complex, which will probably lead to a feature that almost nobody uses. By that, I don't mean that the flags `--not-defined` and `--defined` are themselves complex, but using them effectively---as you've demonstrated here---requires some ingenuity in crafting the regex.

With that said, I'd be willing to adopt a feature like this because I do agree that it could be useful, but I'd have to **strongly** insist on the following:

1. It should not begin life with short flags. I used short flags whenever the flags are common, or if there was a precedent for their existence in other tools. For a feature like this, that is neither common nor familiar, I would like to hold off on adding short flags. If I'm wrong and it becomes popular, then we can revisit it.
2. The maintenance burden of the feature needs to be low. That means adding the feature shouldn't require any significant complications and it should be reasonably well tested.
3. Since the use case motivating the existence of these flags is somewhat complicated, I would like the documentation to be clear. It should be concise, but contain an example usage. (Perhaps a condensed version of the example in this ticket.)

---

_Label `enhancement` added by @BurntSushi on 2017-09-24 12:50_

---

_Label `help wanted` added by @BurntSushi on 2017-09-24 12:50_

---

_Label `icebox` added by @BurntSushi on 2017-10-21 22:44_

---

_Comment by @BatmanAoD on 2018-09-21 19:05_

Now that PRCE is (optionally) supported, can either of you think of a use-case for this that isn't handled by lookahead and lookbehind? I *think* this would be strictly more powerful than negative *lookbehind*, since lookbehind can't contain variable-length patterns, but that's the only advantage I can see. (Granted, that's an advantage I think I would occasionally find useful.)

---

_Comment by @BurntSushi on 2019-04-03 13:18_

I think it could be possible to define a simpler UX than needing to resort to look-around.

With that said, it's a good point and I was never a big fan of adding this feature anyway. So I'm going to close this.

---

_Closed by @BurntSushi on 2019-04-03 13:18_

---

_Comment by @chocolateboy on 2020-09-20 14:51_

Lookaround assertions still have the issues mentioned above. For anyone looking for a clean solution to this with the PCRE engine, the [backtracking-control verbs](https://pcre.org/current/doc/html/pcre2pattern.html#SEC29)[^1][^2][^3] are your friends:

### Input

```
"Tarzan and Jane"
"Jane and Tarzan"
Me Tarzan, you Jane
Tarzan vs "Tarzan"
"Tarzan" vs Tarzan
This line doesn't mention him
He's moved to Tarzania
He's no "Tarzan"!
```

### Command

```bash
$ rg --pcre2 '(?:"Tarzan")(*SKIP)(*FAIL)|\bTarzan\b' test.txt
```

### Output

```
"Tarzan and Jane"
"Jane and Tarzan"
Me Tarzan, you Jane
Tarzan vs "Tarzan"
"Tarzan" vs Tarzan
```

Or, to exclude lines which contain `"Tarzan"`:

### Command

```bash
$ rg --pcre2 '(?:.*?"Tarzan".*)(*SKIP)(*FAIL)|\bTarzan\b' test.txt
$ rg --pcre2 '(?:.*?"Tarzan".*)(*COMMIT)(*FAIL)|\bTarzan\b' test.txt
```

### Output

```
"Tarzan and Jane"
"Jane and Tarzan"
Me Tarzan, you Jane
```

[^1]: https://www.rexegg.com/regex-best-trick.html#pcrevariation
[^2]: https://www.rexegg.com/backtracking-control-verbs.html
[^3]: https://perldoc.perl.org/perlre.html#Special-Backtracking-Control-Verbs

---

_Comment by @BatmanAoD on 2020-09-20 15:42_

@chocolateboy Wow, I had never heard of those before. Thanks for sharing. 

---
