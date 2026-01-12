```yaml
number: 360
title: multiline search for simple cases
type: issue
state: closed
author: timotheecour
labels:
  - question
assignees: []
created_at: 2017-02-12T07:11:47Z
updated_at: 2017-02-18T17:49:06Z
url: https://github.com/BurntSushi/ripgrep/issues/360
synced_at: 2026-01-12T16:13:21Z
```

# multiline search for simple cases

---

_@timotheecour_

even though ripgrep is line oriented and full multiline search is sadly out of question (after reading https://github.com/BurntSushi/ripgrep/issues/176), supporting simple cases would be very useful, eg:

## full description
* `--multiline=[unordered|ordered] --window_size=N [--pattern $regex_i ...]`

will return a list of matches `file:line` such that:

with --unordered:
* file:line contains one of the K requested patterns
* the K-1 other patterns can be found in file at lines [line...line+N)

with --ordered:
* file:line contains the 1st of the K requested patterns
* the K-1 other patterns can be found in file at lines [line...line+N) in the order in which they are given on command line

`--window_size=N` represents the context to search for the K matches. When unspecified, N defaults to infinity (ie all patterns must occur in `file` from `line` to `end of file`

When only a single pattern is used, it has same behavior as standard `rg`

## Examples

`foo.d`:
```d
// first line
auto getFoo(string[] bar){
  // some comment
  return bar.sort.uniq;
}
```

```bash
$ rg --multiline=ordered --window_size=3 --pattern `string[]` --pattern `sort\b`
foo.d:2:auto getFoo(string[] bar){
foo.d:4%return bar.sort.uniq;

$ rg --multiline=ordered --window_size=2 --pattern `string[]` --pattern `sort\b`
# no match, `foo.d:4` is beyond `foo.d:[2...2+window_size)

$ rg --multiline=ordered --window_size=3 --pattern `sort\b` --pattern `string[]`
# no match, the first pattern matches at `foo.d:4` and we can't find the 2nd pattern in `foo.d:[4...4+3)`

$ rg --multiline=unordered --window_size=3 --pattern `sort\b` --pattern `string[]`
# now there's a match since we provided `unordered`
foo.d:2:auto getFoo(string[] bar){
foo.d:4%return bar.sort.uniq;
```



---

_Comment by @BurntSushi on 2017-02-12 15:28_

I don't understand the feature request. I don't know what `window_size` means and I don't know what `--multiline` does and I don't know the difference between `unordered` and `ordered`. It sounds like you have a lot of context in your head that you need to put into writing as a specification.

Tip: consider reusing the `-A`/`-B`/`-C` flags.

---

_Label `question` added by @BurntSushi on 2017-02-12 15:28_

---

_Comment by @BurntSushi on 2017-02-12 15:31_

FWIW, I am personally sympathetic to the idea of running regexes against the context of a match (but where each individual regex can still only match in a single line). That is certainly feasible to do in a way that true multiline search is not. However, it is still a significant addition in terms of implementation.

Please also keep in mind that I am *very* against a complicated UX. A complicated UX is one with lots of knobs that only implementors understand.

---

_Comment by @timotheecour on 2017-02-13 00:51_

* @BurntSushi I've rewritten the proposal, let me know what you think!
This would be super useful as a generic advanced code search among other things. Happy to discuss implementation details if needed


* > each individual regex can still only match in a single line

yes, that's the case

* To implement this (efficiently) using an external tool built on top of (lib)ripgrep, here's what i currently have to emulate a multiline search:

```
rg --multiline=unordered --window_size=4 --pattern 'green' --pattern '\?old'
rg --column --no-heading -A 4 '((green)|(\?old))' --replace '@P1{{$2}P2{$3}}@'
```
and then parse output looking for `@P1{{...}}@`, deducing each match from the regex expansions. That's of course no fun and super brittle. 

* Could we for starters have a machine output (https://github.com/BurntSushi/ripgrep/issues/359) that tells us each match with no information loss for a disjunctive regex of the form `(pattern1|pattern2|...)`
`file:offset:length:regex_index`
with `regex_index` indicating which pattern got matched (eg: 0 for green, 1 for `?old` in this example) and how many bytes the corresponding match is


---

_Comment by @BurntSushi on 2017-02-18 17:49_

Parseable output is here: #244 --- Having that return the index of the regex that matched is not going to happen, since, in the current implementation, that would have a big performance hit.

Overall, I find the feature proposed here to be way too complicated. It doesn't reuse existing flags (like `-A/-B/-C`) to search contexts and instead invents a new "window size" concept. The `--multiline=ordered|unordered` modes don't really make sense to me and seem very unintuitive.

Your needs would be better met by writing code.

---

_Closed by @BurntSushi on 2017-02-18 17:49_

---
