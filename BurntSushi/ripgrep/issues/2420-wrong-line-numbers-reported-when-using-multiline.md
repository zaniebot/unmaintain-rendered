```yaml
number: 2420
title: Wrong line numbers reported when using multiline search
type: issue
state: closed
author: gmile
labels:
  - wontfix
assignees: []
created_at: 2023-02-16T16:11:52Z
updated_at: 2023-02-16T16:56:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2420
synced_at: 2026-01-12T16:13:24Z
```

# Wrong line numbers reported when using multiline search

---

_@gmile_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Via homebrew, e.g.:

```
brew install ripgrep
```

#### What operating system are you using ripgrep on?

macOS 12.5.1 (21G83)

#### Describe your bug.

When using a multiline match, reported line numbers appear to not be correct.

#### What are the steps to reproduce the behavior?

Attempt to find out the line numbers of a `myapp` string, that is known to occur between `app_env` and `:billing` strings.

```
rg --replace '$1' --only-matching --multiline "(?s)app_env.*?(myapp).*?:billing"
```

Using the following text:

```elixir
defmodule MyModule do
  app_env(:plans, :myapp, [:billing, :plans],
    binding_order: [:config],
    required: true,
    type: :any
  )

  app_env(
    :plans_with_min_amount_of_integrations,
    :myapp,
    [:billing, :plans_with_min_amount_of_integrations],
    binding_order: [:config],
    required: true,
    type: :any
  )
end
```

#### What is the actual behavior?

The following output is observed:

```
rg --debug --replace '$1' --only-matching --multiline "(?s)app_env.*?(myapp).*?:billing" test.txt
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
2:myapp
8:myapp
```

#### What is the expected behavior?

The following output is expected:

```
rg --debug --replace '$1' --only-matching --multiline "(?s)app_env.*?(myapp).*?:billing" test.txt
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
2:myapp
10:myapp
```


---

_Comment by @gmile on 2023-02-16 16:14_

Potentially, this is of course could be me failing to understand the regular expression I am trying to use. My expectation is that `(anything)` will be captured into `$1` seems to not stand correct here 🤔 

---

_Comment by @BurntSushi on 2023-02-16 16:29_

It looks like your expected and actual output are the same? Or am I missing something?

The problem here is definitely because of the replacement. It's not obvious to me how line numbers should actually be counted/displayed. In this case, there are two matches in your haystack. One contained to line 2 and one that is spread across lines 8 through 11. You've also told ripgrep, "replace the entire span of the match with `$1`." So ripgrep does just that. And it reports the second replacement as "occurring" on line 8 because that's where the start of the match occurred. If your _replacement_ spans multiple lines, then the line numbers adjust as you might expect:

```
$ rg --only-matching --multiline "(?s)app_env.*?(myapp).*?:billing" haystack -r $'X\nY'
2:X
3:Y
8:X
9:Y
```

The really crucial bit of information here is what you _expect_ to happen, but it looks like that was maybe you have a mistake there?

---

_Comment by @BurntSushi on 2023-02-16 16:36_

> My expectation is that `(anything)` will be captured into `$1` seems to not stand correct here

That seems to be exactly what is happening here? `(myapp)` is capturing `myapp` into `$1`, and that's what is being replaced here.

---

_Comment by @gmile on 2023-02-16 16:44_

> The really crucial bit of information here is what you expect to happen, but it looks like that was maybe you have a mistake there?

@BurntSushi I've just updated the original post with correct expectation, not sure why I originally did it wrong 😞 

I get this output:

```
2:myapp
8:myapp
```

But expect this output:

```
2:myapp
10:myapp
```

It seems I am failing to wrap my head around either there's an issue with my regexp, or why how line numbers are calculated in case of multiline match 🤔 

From what you're saying, it sounds like the reported line numbers correspond to the whole matched string, e.g. starting with `app_env`? 🤔 For some reason I was ended up under impression it's the match group line number that will be reported :(

Is there a way the regexp could be re-written to match the conditions, e.g. to get a line location of string B, given string B is between strings A and C, even if all 3 are on different lines?

---

_Comment by @BurntSushi on 2023-02-16 16:51_

There's really no way for ripgrep to know what you intend the line number to be. ripgrep just reports line numbers based on the match. Consider how your desire would scale to multiple capture groups, or even if your replacement was just a literal string and not a capture group at all. There's no clear and obvious rule that I can see that gets you what you want. The status quo might not always line up with your intuition, but it's a consistent rule: regardless of whether multiline is used or not, ripgrep replaces the entire match with the replacement given, and the line number corresponds to the line number of where the match started. (At least to me, that _also_ matches my own intuition, because the replacement is actually deleting lines here.)

> Is there a way the regexp could be re-written to match the conditions, e.g. to get a line location of string B, given string B is between strings A and C, even if all 3 are on different lines?

It's hard for me to parse this request as-is, but based on the context, I don't think so. There really is a fundamental issue here where you're replacing the entire contents of a match that spans several lines with a a single string that spans only one line. The line numbers have to be reconciled in some way.

---

_Comment by @gmile on 2023-02-16 16:53_

@BurntSushi fair enough, thank you! :+1:

I think I understand the issue here, particularly, my wrong expectations towards what should be the line number. I'll have to think of a way to re-write my regex a bit more, or completely change the approach here.

Closing the issue since obviously this doesn't look like a bug in ripgrep.

---

_Closed by @gmile on 2023-02-16 16:53_

---

_Comment by @BurntSushi on 2023-02-16 16:56_

Aye. It's worth pointing out here that grep tools, and in particular ripgrep's `-r/--replace` flag, are first order approximations of search (and replace) tasks. They are constrained in what they can do, and that is in part (only in part) what permits them to be so fast and "simple."

What I'm trying to say here is that if a simple one-liner with ripgrep doesn't get you what you want, then a simple 10-liner in Python (or whatever language) might be the way to go. :-)

---

_Label `wontfix` added by @BurntSushi on 2023-02-16 16:56_

---
