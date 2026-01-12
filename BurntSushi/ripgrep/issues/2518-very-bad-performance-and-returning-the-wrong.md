```yaml
number: 2518
title: Very bad performance and returning the wrong results for a set of words to search
type: issue
state: closed
author: burntbitterbal
labels:
  - bug
assignees: []
created_at: 2023-05-25T14:07:22Z
updated_at: 2023-08-28T01:41:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2518
synced_at: 2026-01-12T16:13:24Z
```

# Very bad performance and returning the wrong results for a set of words to search

---

_@burntbitterbal_

#### What version of ripgrep are you using?

ripgrep 13.0.0

#### How did you install ripgrep?

Macports

#### What operating system are you using ripgrep on?

Apple M1 Pro
MacOS 12.6.3 

#### Describe your bug.

Very surprised this happens since we trusted ripgrep to return correct matches, but it doesn't.

Slow down is observed and wrong results are produced when running ripgrep with option `-o` against [enwik8.txt](https://www.tensorflow.org/datasets/community_catalog/huggingface/enwik8) when searching for this set of words
~~~
inquisitorial
Diablotins
Tolkien
Louverture
overpower
dhibh
basses
meteors
rumour
gimmors
FiringSquad
USNS
Berlina
uker
WKNR
Avia
Achish
Halys
sunnyfuerteventura
MedImmune
Oshikawa
Boliwia
Rfrisbie
ethnologue
On
thermally
Wiesenthal
Gesalec
punjabjustice
Zahuri
interpro
Deveson
~~~
The search results are wrong and search takes 36 times as long compared to searching the same set of words minus one word removed or added or a different set of words. This also appears to happen with other combinations of 32 words and other files.

#### What are the steps to reproduce the behavior?

Download enwik8.txt and run `rg -on -f words.txt enwik8.txt`.

#### What is the actual behavior?

Slowdown and wrong results:
~~~
/usr/bin/time rg -on -f words  enwik8.txt | wc
        7.70 real         7.53 user         0.13 sys
 100000000 100000000 802126412
~~~

#### What is the expected behavior?

A normal run would be similar to this, with these words and with `TEST` added (33 words to search):
~~~
/usr/bin/time rg -on -f words  enwik8.txt | wc
        0.21 real         0.20 user         0.01 sys
   14186   14186  145933
~~~


---

_Comment by @BurntSushi on 2023-05-25 14:12_

Can you please provide a link to download `enwik8.txt`? I followed the link you gave, but I see no download instructions.

---

_Comment by @BurntSushi on 2023-05-25 14:14_

> The search results are wrong and search takes 36 times as long compared to searching the same set of words minus one word removed or added or a different set of words.

Without commenting on correctness, your performance expectations are definitely wrong here. Adding one word can indeed slow things down by quite a bit. It really depends how common the word is. If adding a word results in a big increase in the number of matches, then I would expect that to slow things down.

But I really need a full reproduction to say what's going on in this specific case. And also, why not say which word you removed that caused it to go faster?

---

_Comment by @BurntSushi on 2023-05-25 14:22_

Apparently this is where the data comes from: http://mattmahoney.net/dc/textdata.html

That link is waaaaaaay better than the one you gave. It took quite some time to find it.

---

_Label `bug` added by @BurntSushi on 2023-05-25 14:34_

---

_Comment by @BurntSushi on 2023-05-25 14:35_

Yup. There is a bug here. Interestingly, even small changes to the words, like chaging `Tolkien` to `tolkien` make ripgrep produce the correct result.

It seems like what's happening is that something is triggering a bug that causes ripgrep to match at every position. That explains the slowdown and the large number of matches reported.

This looks like another literal optimization bug.

---

_Comment by @BurntSushi on 2023-05-25 15:14_

Here's a minimal reproduction as a Rust program using the regex engine directly:

```rust
fn main() {
    let needles = vec![
        "aA", "bA", "cA", "dA", "eA", "fA", "gA", "hA", "iA", "jA", "kA", "lA",
        "mA", "nA", "oA", "pA", "qA", "rA", "sA", "tA", "uA", "vA", "wA", "xA",
        "yA", "zA",
    ];
    let pattern = needles.join("|");
    let re = regex::Regex::new(&pattern).unwrap();
    let hay = "FUBAR";
    assert_eq!(0, re.find_iter(hay).count());
}
```

That assert should pass, but it panics because `find_iter` found 6 matches even though there are zero.

This is indeed a literal optimization bug. Sigh.

I also don't have any great work arounds for you either. The bug happens as a result of using a sequence of literals with 26 different start bytes. It doesn't happen in every such case, because this particular bug also requires a suffix literal searcher to be extracted. One way to push ripgrep out of this bug is to add a pattern that can never match that also suppresses the suffix literal optimizer. For example, `ZQZQZQZQZQZQ\w`.

---

_Comment by @BurntSushi on 2023-05-25 15:28_

I filed a bug against the regex library as well: https://github.com/rust-lang/regex/issues/999

---

_Closed by @BurntSushi on 2023-05-25 17:06_

---

_Comment by @burntbitterbal on 2023-08-27 23:37_

Sorry to bother you again, I'm sure you're busy updating ripgrep. But this looks important.

I was alerted to this article https://www.genivia.com/ugrep.html was mentioned in the Reddit post https://www.reddit.com/r/opensource/comments/160zla6/ugrep_ultra_fast_grep_with_tui_unicode_support/

Is this serious bug actually fixed in all ripgrep installs and all Rust regex library installs?

I agree that this could be a security vulnerability, especially when it is more wide-spread than ripgrep (the Rust regex library.) I don't see a CVE on this issue or an advisory mention this issue. Other regex libraries have CVEs when such a bug was found.

---

_Comment by @BurntSushi on 2023-08-27 23:40_

> Other regex libraries have CVEs when such a bug was found.

Can you provide links?

> Is this serious bug actually fixed in all ripgrep installs and all Rust regex library installs?

It's fixed on master: https://github.com/BurntSushi/ripgrep/commit/fc0d9b90a9dbc1ba8d93a3f58da4ddbac0c40a7f

---

_Comment by @burntbitterbal on 2023-08-28 01:34_

Searching for pcre cve gives hits, but I have not checked other libraries, just spent about a minute finding one like this: 

**CVE-2015-8393**
pcregrep in PCRE before 8.38 mishandles the -q option for binary files, which might allow remote attackers to obtain sensitive information via a crafted file, as demonstrated by a CGI script that sends stdout data to a client.

I'm not a security specialist. This stuff is out of my league.

---

_Comment by @BurntSushi on 2023-08-28 01:40_

That CVE is quite sparse on the details, but it reads to me like it's talking about the `--quiet` flag in combination with specific types of binary files. That's not applicable to this bug, which can only be triggered by a certain class of regexes. If your attacker can already change the regex to change what matches are emitted, then the threat model of "produces matches when it shouldn't" doesn't seem relevant.

But yeah you made a statement of fact

> Other regex libraries have CVEs when such a bug was found.

So I just assumed you actually had specific CVEs in mind that were relevant here.

I'm not aware of any standing policy by any regex engine that treats match bugs as security vulnerabilities all on their own. That doesn't mean that treating match bugs as security problems is wrong, I'm just not aware of any precedent there.

---
