```yaml
number: 236
title: limit search to specific namespaces and classes
type: issue
state: closed
author: phipse
labels: []
assignees: []
created_at: 2016-11-16T11:44:24Z
updated_at: 2017-01-25T03:13:26Z
url: https://github.com/BurntSushi/ripgrep/issues/236
synced_at: 2026-01-12T16:13:21Z
```

# limit search to specific namespaces and classes

---

_@phipse_

Hi,
I'd like to be able to focus the search in a C++ code base to classes/functions in specific namespaces;
Meaning, I'd like to do something like this:
```sh
rg ns::Foo ./path/
```

and get a result 'class Foo' defined in namespace 'ns' ; and no results of 'class Foo'  defined in other namespaces.

Or if I am looking for a function 'fn' in class 'Bar', I'd like to do:
```sh
rg Bar::fn ./path/
```
and get not only a result for a member function definition outside the class declaration, which already contains the string "Bar::fn", e.g.:

```c++
void Bar::fn(int a, int b) { ... }
```

but also results for a function definition 'fn' inside a class declaration e.g.:
```c++
class Bar {
void fn(int a, int b) { ... }
};
```
Is something like this already possible?

---

_Comment by @BurntSushi on 2016-11-16 11:50_

This requires language aware search. As of right now, ripgrep is a _general purpose line oriented search tool_. Language aware search is not something ripgrep currently supports, and it's not really on the horizon either. Language aware search would require _significant effort_.

The closest thing to your request (and it's admittedly not _that_ close) is #95, which is about adding fulltext search to ripgrep. It doesn't have to be strictly language aware, but having language aware tokenizers could potentially improve the quality of results. However, language aware tokenizers is a totally different story than actually understanding language level concepts like classes or namespaces.

Maybe something like this will appear in ripgrep (or a related tool) some day, but I'd expect that time scale to be measured in _years_. So I'm just going to close this for now.


---

_Closed by @BurntSushi on 2016-11-16 11:50_

---

_Comment by @BurntSushi on 2016-11-16 11:53_

@phipse Out of curiosity, is there another tool you use that has this feature? (Other than something like a specific C++ IDE plugin or something.)


---

_Comment by @phipse on 2016-11-16 12:22_

No, there isn't, unfortunately. Is there a way to match the corresponding closing brace using ripgrep,  such that I can search for "namespace Foo {.*}" ?

In my head it's basically this:

```
for each file in `rg -l "namespace Foo"`:
do
rg "class Bar" file
done;
```

and in between count the opening and closing braces to limit the content of `file` to the namespace.
A language aware tokenizer would be nice to have, but I think for my use case this might also be a little bit over the top.


---

_Comment by @BurntSushi on 2016-11-16 12:39_

> Is there a way to match the corresponding closing brace using ripgrep, such that I can search for "namespace Foo {.*}" ?

ripgrep is line-oriented. It doesn't support multi-line search. Stated more clearly: no part of a regex can ever match a `\n`.

> and in between count the opening and closing braces to limit the content of file to the namespace.

In normal operation, ripgrep never actually holds the contents of an entire file in memory (unless it happens to be small enough to fit into ripgrep's buffer).

> A language aware tokenizer would be nice to have, but I think for my use case this might also be a little bit over the top.

But the specification you've described _is_ language aware. (I only mentioned a language aware _tokenizer_ in the context of _fulltext search_. I agree that it is both insufficient and overkill at the same time for your problem.) More to the point, the implementation path you've described is also insufficient. You can't just count braces because they can appear inside of strings or comments where they have no significance.

---

I will say this: I do continue to noodle on the possibility of multi-line search, because it is occasionally useful, and it could possibly bail you out too in a problem like this. There are unfortunately significant performance trade offs that come with multi-line search though (both time _and_ memory), so it needs careful thought and probably won't happen soon.


---

_Comment by @phipse on 2016-11-16 12:55_

Alright, thanks a lot for your thoughts.


---
