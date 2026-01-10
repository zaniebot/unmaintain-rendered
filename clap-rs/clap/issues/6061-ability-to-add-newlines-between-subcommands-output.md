---
number: 6061
title: Ability to add newlines between subcommands output
type: issue
state: open
author: s1neet
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2025-07-04T09:54:32Z
updated_at: 2025-08-07T22:19:55Z
url: https://github.com/clap-rs/clap/issues/6061
synced_at: 2026-01-10T01:28:21Z
---

# Ability to add newlines between subcommands output

---

_Issue opened by @s1neet on 2025-07-04 09:54_

Hi Clap team

I'm building a CLI tool with many subcommands grouped by feature domains. I'd like to improve the readability of the help output by inserting blank lines between subcommands in the --help display — ideally without building a custom help renderer as I coudnt use builtin features of subcommand if use custom help renderer.

Instead of this:


```
Commands:
  list       List all items
  add        Add an item
  remove     Remove an item
  update     Update an item

```
I’d like something like:

```
Commands:
  list       List all items

  add        Add an item

  remove     Remove an item

  update     Update an item
```
**What I’ve Tried**
Adding \n at the end of each #[command(about = "...")] — doesn’t work (Clap trims it).

next_line_help: which adds a newline after the name of the subcommand heading which looks bit wierd. 
This is how it looks after using next_line_help:
```
Commands:
  list       
                 List all items
  add        
                 Add an item
  remove     
                 Remove an item
  update    
                 Update an item

```

Custom help_template with {subcommands} — doesn’t support spacing between entries.


Would you consider supporting custom spacing or grouping between subcommands natively in Clap’s help renderer?

For example:

An attribute like #[command(blank_line_after)]

A new built-in {subcommands-with-spacing} placeholder

---

_Label `C-enhancement` added by @s1neet on 2025-07-04 09:54_

---

_Label `A-help` added by @epage on 2025-07-07 14:32_

---

_Comment by @epage on 2025-07-07 14:38_

To be clear, you aren't wanting blank lines between groups of subcommands but after every subcommand?  We have #1553 for the former.

As for blank lines between every subcommand, what I see lacking is a motivation.  How is this expected to impact users?  While we try to remain flexible in some ways, we also see value in consistency, encouraging best practices, balancing functionality with the costs.  What I mean by that last point is that there is a cost to every new piece of configuration we give users.  We increase the scope of the API, making it harder to find what is there (including this new feature) making it so people use less of what we provide.  This also increases our build times and compile times.

There are two options for something like this to go forward:
- A clear case for why this would be justifiable as a default
- A clear case to justify offering this as configuration.

From my own experience and seeing others write and use CLIs, my hunch is the bar won't be met for either but having your motivation for this request can help us properly evaluate that.

---

_Comment by @s1neet on 2025-07-15 08:35_

Hi, thank you for the thoughtful reply! I completely understand the need to weigh API surface and consistency carefully.

Let me clarify the motivation behind this request.

In our case, we have large CLI tools (20–30+ subcommands per group) with long descriptions. The dense listing of subcommands even when logically grouped makes it visually hard to scan. Specifically with a small terminal the text wrapping makes it awkward. Especially for commands that have very long descriptions. I did use the the term width feature but thats not helped a lot as the text looks cluterred when the terminals are smaller. A feature like next_line_help adds some readability but it just add the help text in the next line. I think having spacing between the commands/options instead would make it more readable. Example: 

```
Commands:
  list       
                 List all items,List all items,List all items,List all items,List all items,List all items,
List all items,List all items,List all items,List all items,List all items,List all items,List all items,
List all items,List all items,List all items,List all items,List all items,List all items,List all items,
  add        
                 Add an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd
an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an
itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an item
  remove     
                 Remove an itemRemove an itemRemove an itemRemove an itemRemove an item
Remove an itemRemove an itemRemove an itemRemove an itemRemove an itemRemove an
 itemRemove an itemRemove an itemRemove an itemRemove an itemRemove an itemRemove
```
As you can see, while everything is technically aligned, the output becomes overwhelming with large descriptions. The subcommand names get lost in the visual noise, especially when descriptions wrap.

Now imagine if there was an optional blank line after each subcommand:
```
Commands:
  list       
                 List all items,List all items,List all items,List all items,List all items,List all items,
List all items,List all items,List all items,List all items,List all items,List all items,List all items,
List all items,List all items,List all items,List all items,List all items,List all items,List all items,

  add        
                 Add an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd
an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an
itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an itemAdd an item

  remove     
                 Remove an itemRemove an itemRemove an itemRemove an itemRemove an item
Remove an itemRemove an itemRemove an itemRemove an itemRemove an itemRemove an
 itemRemove an itemRemove an itemRemove an itemRemove an itemRemove an itemRemove
```
Even minimal vertical spacing dramatically improves readability and scan-ability, especially when users are not already familiar with all the commands. It makes the help feel more welcoming and less dense, when reading in narrow terminals. I’m not asking for this to be the default, but rather an opt-in feature (similar to next_line_help) that introduces consistent blank lines between subcommands/options in the help output.

Thank you again for considering it.

---

_Comment by @epage on 2025-08-07 21:50_

So looking at this more:
- Our "short" next line help doesn't include a blank line while our "long" does
- We render subcommands in a "short" manner with the assumption being that this is more of an index and that users should run `-h` on the specific subcommand for more details (or use `flatten_help`)

However, if we are not wrapping and indenting the description for subcommands, with or without `next_line_help`, then that is a bug.

---

_Referenced in [clap-rs/clap#6095](../../clap-rs/clap/pulls/6095.md) on 2025-08-07 22:16_

---

_Comment by @epage on 2025-08-07 22:19_

I created #6095 to show our wrapping and next line help behavior.  Seems like we are wrapping which wasn't made clear by the examples above and I'm seeing less of an issue here.

> In our case, we have large CLI tools (20–30+ subcommands per group) with long descriptions. 

That sounds unwieldy to use.  Why is it you feel that the short descriptions of the commands should be so long?

---
