```yaml
number: 2211
title: "\"After\" context based on regex"
type: issue
state: open
author: jetzerb
labels: []
assignees: []
created_at: 2022-05-12T19:41:32Z
updated_at: 2023-12-07T15:52:41Z
url: https://github.com/BurntSushi/ripgrep/issues/2211
synced_at: 2026-01-12T16:13:24Z
```

# "After" context based on regex

---

_@jetzerb_

#### Describe your feature request
Would love an option like `-A <NUM>` but instead of specifying a fixed number of lines, I could specify a regular expression.  Similar to `sed -n '/regex1/,/regex2/p'`.



---

_Comment by @BurntSushi on 2022-05-12 19:59_

Could you please provide a detailed use case?

---

_Comment by @jetzerb on 2022-05-12 21:03_

One use case is to find and dump out the contents of methods from code files. Assume the following sample file.

```cs
// comments for Foo
public bool Foo ()
{
    return true;
}

// comments for DoSomething
public void DoSomething(...)
{
    . . . some code . . .
}

// Comments for Bar
public bool Bar ()
{
    string xyz;
    int pdq;
    . . . some code . . .
    return result;
}
```

`rg --after-regex '^}' 'public\s+bool\s+(Foo|Bar)'` would show
```
2:public bool Foo ()
3-{
4-    return true;
5-}
--
14:public bool Bar ()
15-{
16-    string xyz;
17-    int pdq;
18-    . . . some code . . .
19-    return result;
20-}
```

The best I can do now is provide a fixed number, and then for some matches I'll get more than I want and for others I'll get less:

```
$ rg -A 3 'public\s+bool\s+(Foo|Bar)'

2:public bool Foo ()
3-{
4-    return true;
5-}
--
14:public bool Bar ()
15-{
16-    string xyz;
17-    int pdq;
       <-- THIS METHOD'S BODY IS INCOMPLETE
```

```
$ rg -A 6 'public\s+bool\s+(Foo|Bar)'

2:public bool Foo ()
3-{
4-    return true;
5-}
6-                                 <-- DON'T WANT THIS
7-// comments for DoSomething      <-- DON'T WANT THIS
8-public void DoSomething(...)     <-- DON'T WANT THIS
--
14:public bool Bar ()
15-{
16-    string xyz;
17-    int pdq;
18-    . . . some code . . .
19-    return result;
20-}
```

Notes:
- I'm sure valid use cases could pop up for analogous "before" context regex as well
- And if you _really_ want to get fancy, there would be inclusive / exclusive options so that the line containing the context boundary regex is either the last one printed _or_ the first one _not_ printed (if all the methods are surrounded by `#region REGIONTITLE` and `#endregion`, then you could specify `--before-regex-exclusive '#region' --after-regex-exclusive '#endregion'` so that `ripgrep` would dump out everything _between but not including_ the lines containing `#region REGIONTITLE` and `#endregion`

---

_Comment by @VladimirMarkelov on 2022-05-12 21:08_

@jetzerb 

I doubt regex works fine here. What about more complex function which has loops/ifs/whiles:

```
// Comments for Bar
public bool Bar ()
{
    if Foo() == 3 {
        ..
    } // regex cut it here, but the function is longer
    ...
}
```

Edit: sorry, I saw you look for `^}`, it should works in this particular case, but for generic usage, Regex seems less useful. Consider a case when a function is a method, so it is indented, and `^}` does not work:

```
# it is not any real language, but in some languages this construction is possible
namespace Abc {
   public bool Bar ()
   {
         ...
   }
}  # this one will be in the output
```

---

_Comment by @jetzerb on 2022-05-12 21:23_

@VladimirMarkelov 
Yes, certainly not perfect, but more helpful than `-A <NUM>` in many cases.

Depending on how consistent the code is, I could specify `--after-regex '^ {4}}'` or `--after-regex '^\t}'` to get it to work with a plurality or even a majority of methods.

There are other use cases besides searching code files as well.  Perhaps you want to search LaTeX files for everything within an environment:
```latex
\begin{align}
. . .
\end{align}
```

Or maybe you're searching a text file from Project Gutenberg, and you want to dump out all paragraphs containing a particular word or phrase.  You could
`rg -i --before-regex '^$' --after-regex '^$' 'jabberwocky'`

---

_Comment by @VladimirMarkelov on 2022-05-12 21:25_

Yes, it may be helpful. I agree. Though, I am not the one who decides what feature to include into the project :)

---

_Comment by @BurntSushi on 2022-05-12 23:04_

This is _very close_ to a duplicate of #502. If #502 doesn't happen (although I may still reconsider because I think it is a neat feature that can work well in practice), then I think it's unlikely for this feature to happen either. The only real difference between this issue and #502 is that #502 requires writing a bunch of language specific regexes to find the bounds of a function. I'm not at all opposed to that (it can easily be crowd sourced). It's really about the underlying mechanism for making `--{before,after}-regex` work in the first place that is tricky.

---

_Comment by @jetzerb on 2022-05-13 12:34_

Per [ripgrep's search mechanism](https://blog.burntsushi.net/ripgrep/#mechanics) (emphasis mine):
> Worst of all: your searcher needs to be able to show the context of a match, e.g., the lines before and after a matching line. For example, consider the case of a match that appears at the beginning of your buffer. How do you show the previous lines if they aren’t in your buffer? You guessed it: _**you need to carry over at least as many lines that are required to satisfy a context request**_ from buffer to buffer.

I had imagined this enhancement to be implemented something like this.  Of course it's much simpler in my head because I'm not the one who actually has to do the work 😄 so feel free to tell me why this is impractical.
1. Search from the current position within the current buffer for a match against the primary pattern
1. If primary pattern is found
   1. Search the previous contents of the buffer for the last occurrence of the `--before-regex` pattern.  If not found, read in previous buffers as necessary. To avoid printing overlapping sections of the file, you'd probably also stop if you run into a match against the primary search pattern before you encounter the `--before-regex` pattern.
      > I don't know whether reading previous buffers is possible when data is being piped to `rg`, which is why the original request only included an `--after-regex` option. This step and the next would be skipped if not implementing `--before-regex` functionality.
   1. print from the `--before-regex` line until the line before the primary pattern line.  Highlight the `--before-regex` pattern in a different color than the primary pattern.
   1. Print the line with the primary pattern
   1. keep printing file contents until the `--after-regex` pattern is encountered, reading in subsequent buffers as necessary.  In _this_ direction, I think it'd be appropriate to highlight any further instances of the primary pattern encountered before the `--after-regex` pattern.  Highlight the `--after-regex` in a different color than the primary pattern.
1. Else read in the next buffer.
1. Go to step 1.
 


---

_Comment by @BurntSushi on 2022-05-13 12:48_

If you aren't familiar with ripgrep's existing implementation, then it probably doesn't make sense to try to come up with an implementation path for this feature. For example, you're assuming you can just arbitrarily "get the previous buffers." ripgrep doesn't keep those around. So now you're talking about introducing seek calls and re-reading parts of the file. That's no good.

The first thing here is that to do this correctly, it's best to divert to the slower strategy of reading the input line-by-line instead of trying to be clever. In that context, handling `--after-regex` is easy enough. As you say, after printing a matching line, you keep printing lines until the `--after-regex` is matched. If a proper match is found before then, then it is printed as a normal match.

The hard bit here is `--before-regex`. Since arbitrary bidirectional flow is not a path I want to go down, the only way I can see to do this correctly is to optimistically match `--before-regex` and print the before context into a side buffer. Every match of `--before-regex` resets this buffer. When a proper match is found, we print the contents of this buffer first. The problem with this approach is that in the worst case it could read the entire file into memory, which isn't good. But I don't really see a way around this.

---

_Comment by @jamesharr on 2023-09-19 16:41_

I have a potential use-case for `--before-regex`. I work in networking and Cisco/Arista-like devices represent configuration in a white-space indented context.

```
...
interface Ethernet 3/7.800
 ipv4 address 192.0.2.1/30
 ipv6 address 2001:db8::1/126
 load-interval 30
 flow ipv4 monitor fmm_ipv4 sampler ipfix-sample ingress
 flow ipv6 monitor fmm_ipv6 sampler ipfix-sample ingress
 flow mpls monitor fmm_mpls sampler ipfix-sample ingress
 description CUST1: Primary Circuit
 encapsulation dot1q 800

interface Ethernet 3/8.900
 ...
 description CUST2: Primary Circuit
```

What I would like to do is run something like:
```
% rg --before-regex='^interface' "description CUST1:" config_backups
```

and get back the following:
```
config_backups/switch1.config:
100-interface Ethernet 3/7.800
107: description CUST1: Primary Circuit
```

More or less, what I want is something that works/functions a lot like the context line in a diff/patch. I can see the use-case to include JUST the context line, but I can also see the use-case to include anything between the match and the context-line (line 101-106)

For an implementation, I'm wondering if what's below might fit rust well. I'm not describing rust types (apologies, not a Rust dev), but I think it might match the memory model well if I understand it.
- Define a `before_context` variable that's an array of strings
- When the --before-context line matches:
   - Clear the existing `before_context` (if any)
   - Add current line to context
- When the pattern matches
   - Output the contents of `before_context`
   - Output the matched line (as you would normally do)
   - Clear the `before_context` variable
- When neither the pattern or --before-context matches:
   - Add the line of the file to the context... or not... maybe this needs to be an option to allow for either of the things above?

That last bullet has a bounding issue in that it's worst-case scenario is "the whole file". If you want to bound the problem, you may want to have a flag that sets the maximum length `before_context` can be and truncates if it's exceeded.

I also suppose that array would need to be more than "an array of strings" since you need line-numbers with it.

I tend to get by with using regex like `^int| description CUST1:`, but it still gives me more "context" than I need since it includes EVERY interface and not just the one I'm matching. So, it's not an urgent feature, but it'd be a nice addition for me.

LMK if you want me to spin up a separate ticket for this.

---

_Comment by @jamesharr on 2023-09-19 16:58_

There's another use-case that's related to this, but not exactly the same. Configuration can be nested fairly deep. The context is usually displayed by adding a space in front.

```
router bgp 65001
 vrf A
  neighbor 192.0.2.1
   ...
  neighbor 192.0.2.2
   ...

 vrf B
  neighbor 192.0.2.3
   remote-as 65002
   local-as 65002 no-prepend replace-as
   description Route Collector
   update-source Loopback7
   address-family ipv4 unicast
    route-policy DROP-ALL in
    route-reflector-client
    route-policy ROUTE-COLLECTOR out
```

I might want to run
```
rg --indent-context "route-policy ROUTE-COLLECTOR" config_backups
```
and get back
```
config_backups/router1.config:
100-router bgp 65001
107- vrf B
108-  neighbor 192.0.2.3
113-   address-family ipv4 unicast
116:    route-policy ROUTE-COLLECTOR out
```

[Pedantry] Technically, the language grammar doesn't assign significance to the leading whitespace (like Python does), this is just how the router will display it. You can take the example above, and remove leading/trailing whitespace and it's still valid configuration. However, for the purposes of ripgrep, I'd assume that the leading space(s) are an indicator of context.

Similar to before, I can see the use-case for both "I just want to know what section I'm in" and "I want to know everything between "router bgp 65001" and the "route-policy ROUTE-COLLECTOR" line.

I suspect this one is going to be a bit more challenging to implement, but it's still based on a real-world use-case.

Again, not urgent. I don't know of anything that does this today, but it's in one of those "this would have been nice to have for the last 20 years" category for a lot of network engineers.

---

_Comment by @Brixy on 2023-12-07 15:52_

Some [(non-UNIX) versions of grep](http://dbosnothers.blogspot.com/p/search-and-print-matching-paragraph.html) seem to provide a `-p` flag for *paragraph*.

`--after-regex '\n\n'` would be a simple and flexible alternative.

Thank you for your excellent tool!

---
