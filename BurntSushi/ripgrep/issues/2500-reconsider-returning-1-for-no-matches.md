```yaml
number: 2500
title: "Reconsider returning 1 for \"no matches\""
type: issue
state: closed
author: dubrowgn
labels:
  - wontfix
assignees: []
created_at: 2023-04-27T08:55:35Z
updated_at: 2023-05-15T13:57:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2500
synced_at: 2026-01-12T16:13:24Z
```

# Reconsider returning 1 for "no matches"

---

_@dubrowgn_

I know this has been discussed here before. I'm not really sure what I'm expecting to happen, but I do want to add some thoughts to the conversation.

I've seen a lot of developers get bit by `grep`'s behavior of returning `1` when there are no matches, and I have personally had to work around this more times than I care to remember. So, I was sad to see this "feature" was replicated by `ripgrep`. I understand it can be useful in the following use case:

```bash
if rg -q foo file; then
    echo matched
fi
```
IMO, there is a better way to handle this specific case. I'm not aware of other use cases, so it would be interesting to learn about them if they exist.

## The problem with returning `1`

The root of the problem with returning `1` for no matches is that "no matches" is a perfectly normal and successful result for a search, but various aspects of shell scripting assume any non-zero return code is an error. This has a few annoying manifestations, but at a high level means error handing with `grep`/`ripgrep` is now more complicated. As error handling is highly desirable in reliable software, using these tools can introduce bugs because they don't follow the "non-zero return code is an error" convention. Some examples:

### 1. bailing out with `-e`

One very common pattern for handling errors in scripts is to set the `-e` option. This causes the script to terminate in the event of any error encountered in the script. This isn't a great way to do error handling, but it does at least prevent your script from doing dumb things if a step in the middle fails. Because `ripgrep` returns `1` for no matches, you cannot use it in a script with `-e` set. I've seen this bite a lot of devs, as scripts appear to work normally, but sometimes fail silently for no obvious reason with no error output. The reason is that a `grep` somewhere had no matches.

### 2. propagating errors with `-o pipefail`

Similarly, one can bail out of a series of piped commands if one of them fails by setting `-o pipefail`. This is very useful because you often otherwise end up propagating an empty string through your piped commands, again leading to silent failures that are not obvious. Because `ripgrep` returns `1` for no matches, you cannot use it in a series of piped commands with `-o pipefail` enabled. You can work around it by splitting your code, but the result is very ugly.

```bash
# without pipefail

results="$(rg 'foo' file-name.txt | sed 's/bar//' | sort | uniq -c | head)"
# $? == 0, even if file-name.txt does not exist
# we can't handle (most) errors because we can't detect them

# with pipefail

set -o pipefail

results="$(rg 'foo' file-name.txt | sed 's/bar//' | sort | uniq -c | head)"
# $? == 1 if file-name.txt does not contain 'foo'; sed does not run
# handing errors is impossible because we don't know where the `1` came from

# so, we must split rg into its own execution, inefficiently spooling results into a variable
# it's also very verbose
matches="$(rg 'foo' file-name.txt)"
rc="$?"
if [[ "$rc" -gt "1" ]]; then
   exit "$rc"
fi

results="$(echo "$matches" | sed 's/bar//' | sort | uniq -c | head)"
# now I can handle errors with $?
```

## Alternatives

If the only use case for returning `1` for no matches is so `ripgrep` can be used in conditionals, why not have a flag specifically for that?

```bash
# instead of this:
if rg -q foo file; then
    echo matched
fi

# why not this:
if rg --matches foo file; then
    echo matched
fi
```

Maybe I'm missing another use case, but it seems to me you would never use `-q` unless you were going to look at the return code. So rather than having a flag that works with the normal return codes indirectly to get what you want (`-q`), why not have a flag that specifically handles the conditional use case? E.g. `ripgrep` normally returns `0` on success and non-`0` on error; passing it `--matches` (or whatever you want to call it) suppresses output and changes the return code to be `0` for one or more matches and `1` for no matches. You can then remove `-q`.

## Related

This is most closely related to https://github.com/BurntSushi/ripgrep/issues/1290. See also: https://github.com/BurntSushi/ripgrep/issues/948 and https://github.com/BurntSushi/ripgrep/issues/1159.


---

_Comment by @BurntSushi on 2023-04-27 12:16_

There is really no way this is going to change. It's certainly not going to change for `grep` because of backcompat, and differing from `grep` on this particular point would be _massively_ confusing. This is a classic case of identifying some real problems with the status quo (I basically agree with the downsides and annoyances you've pointed out), but simultaneously not seeming to acknowledge the downsides of breaking existing behavior.

Shell scripting is kind of a mine field of various quirks like this. I wish we had something better. But I think the path to something better is to move on to a different shell language entirely where there is no expectation of backcompat.

---

_Closed by @BurntSushi on 2023-04-27 12:16_

---

_Label `wontfix` added by @BurntSushi on 2023-04-27 12:16_

---

_Comment by @dubrowgn on 2023-05-13 07:28_

I definitely hear what you are saying.  Not breaking existing consumers is important, but it feels like we may be dismissing this out of hand without really exploring the solution space. Just a couple of thoughts:

> It's certainly not going to change for grep

Agreed. That ship has sailed, but if we can't have nice things in ripgrep because grep doesn't have nice things, we might as well just stick to grep, right?

> differing from grep on this particular point would be massively confusing

Personally, ripgrep's saner defaults that are the primary attraction, this particular behavior being a notable exception. I don't find it confusing that ripgrep behaves differently than grep, I find it refreshing. If ripgrep and grep behaved exactly the same, I wouldn't use ripgrep. Sample size of one, but there it is.

> but simultaneously not seeming to acknowledge the downsides of breaking existing behavior.

It is true we need to consider any changes carefully, but changes don't necessarily have to be breaking. Given my proposal, it seems like making `-q` and alias for `--matches` (or whatever you want to call it), avoids breakage for current users of `-q`. It also seems unlikely this change would break other users, but you could permanently ban `1` as a valid return code (outside of `--matches`) so existing error handling keeps working (e.g. `"$rc" -gt "1"` and similar). That just leaves users who are specifically looking for return code `1` without `-q`. I can come up with some pretty contrived examples, but it doesn't seem like a particularly useful pattern. The trade off is future users can finally forget about this behavioral oddity, and you actually fix existing scripts that (I would say reasonably) expect ripgrep to return `0` for non-errors. You can deprecate `-q`, or not, remove it in some future version, or not. It's up to you.

Alternatively, you can add a flag so users can opt into saner return codes.

```bash
# this:
alias rg="rg --non-zero-return-code-on-error" # TODO -- better flag name

# instead of:
function _rg() {
    rg "$@"
    rc="$?"
    if [[ "$rc" == "1" ]]; then
        return 0
    fi
    return "$rc"
}
alias rg="_rg"
```

Outside of that, IMO a breaking change here would still be a net win because of how many things the current behavior breaks inadvertently. It's not unheard of for terminal tools to change behavior. Ironically, we just recently encountered changing behavior in `grep` during a migration to a newer version. Changes happen; we updated our scripts and moved on. Should we make breaking changes regularly? Definitely not. But, that doesn't mean we can't consider it fairly and weigh the trade-offs.

Anyway, for your consideration. Hopefully this doesn't come across the wrong way. Ripgrep is a great piece of technology, and I really enjoy using it. Thank you for all your efforts.

---

_Comment by @BurntSushi on 2023-05-13 12:19_

> Agreed. That ship has sailed, but if we can't have nice things in ripgrep because grep doesn't have nice things, we might as well just stick to grep, right?

Who said we can't have nice things in ripgrep because grep doesn't have nice things? I certainly didn't. And the existence of ripgrep and its differences from grep makes it absolutely clear that I don't believe it either. So I really just do not know where you're going with this.

> Personally, ripgrep's saner defaults that are the primary attraction, this particular behavior being a notable exception. I don't find it confusing that ripgrep behaves differently than grep, I find it refreshing. If ripgrep and grep behaved exactly the same, I wouldn't use ripgrep. Sample size of one, but there it is.

Again, I don't understand your point. It seems like you're arguing against some straw man argument in favor of ripgrep and grep behaving exactly the same. But nobody in this issue is taking that position.

Let's revisit what I said:

> It's certainly not going to change for `grep` because of backcompat, and differing from `grep` on this particular point would be _massively_ confusing.

Notice that this isn't a general argument in favor of making ripgrep and grep behave exactly the same. I was extremely careful to say "**on this particular point**."

This isn't some binary issue where our choices are "bug-for-bug compatibility with grep" and "absolutely no compatibility at all." ripgrep differs from grep, *of course*, but there is a lot in common too. ripgrep covers a significant subset of GNU grep's flags for example, certainly a bigger subset than the flags that are unsupported. But it's not just about flags. ripgrep generally also behaves similarly with respect to binary data (with ripgrep defaulting to `-I/--binary-files=without-match`). And of course, the exit code behavior.

Just because ripgrep differs from GNU grep on some points doesn't mean there aren't any costs to it. And it doesn't mean there aren't any costs to any new divergence from behavior.

As I said before, shell scripts are a mine field of issues, and the fact that grep uses an error exit code to indicate "no match" is a time tested tradition. While changing the default in ripgrep *might* be easier on beginning shell script programmers, I'm 100% certain it would confound and piss off most others writing shell scripts and using ripgrep in them. ripgrep already probably pisses off enough people by defaulting to "smart filtering." That's where it spends its "weirdness" budget. And that is where I want to continue to spend it. I don't want to spend it on changing how exit codes behave. Dealing with grep exit codes is already subtle enough. Making ripgrep differ on this point when so much capital has already been expended on how grep currently behaves. It's reasonable to expect ripgrep to behave the same on this point.

On top of all of that, one of the most compelling reasons against changing the default is that the **failure modes are potentially silent.** That is the worst kind of breaking change. Previously, shell scripts that use `rg -q foo path` relied on them to exit with `1` when no match was found, but with your change, it wouldn't. No matches would exit with `0` instead. That's a very subtle change.

Even if I could start all over again with ripgrep or a new tool, I'm pretty sure I would continue to mimic grep's behavior here. Despite the downsides you've listed, it also has upsides. And the downsides, at least to me, seem more like general downsides of shell scripting as opposed to how specific commands behave.

What I'm getting at here, and what I mentioned before, is that this problem isn't something I see as solvable at the level of individual programs. You need to pick a different paradigm to fix things like this.

It's easy for you to suggest changes like this. You aren't the maintainer. I am. I'm the one who has to deal with people coming here filing bugs about the change in behavior. I'm the one who's going to have to defend the choice. That's on *me*. Not you. You don't have to worry about the unseen masses of people that care a lot more about a program not breaking their shell scripts than an arguably small improvement to error handling in shell scripts. What I mean when I say this is that I don't think you are appropriately considering the downsides of a change like this.

> It's not unheard of for terminal tools to change behavior. Ironically, we just recently encountered changing behavior in `grep` during a migration to a newer version. Changes happen; we updated our scripts and moved on. Should we make breaking changes regularly? Definitely not. But, that doesn't mean we can't consider it fairly and weigh the trade-offs.

Yes and? Obviously I'm aware that can ripgrep can make breaking changes. The CHANGELOG has several of them. My very first response to you very clearly used explicit language suggesting that I was indeed weighing trade offs!

The frustrating part of your response here is that it seems to be a response to someone holding one or both of these dogmatic positions:

* ripgrep should behave exactly like GNU grep.
* ripgrep shouldn't have breaking changes in subsequent releases.

I of course hold neither position. Despite you asking me to weigh the trade offs, it seems to me like your comment is more about treating that as a binary issue.

> Alternatively, you can add a flag so users can opt into saner return codes.

A lot of my concerns revolve around changing the default behavior of ripgrep, so it's worth exploring this idea which I understand to be "the current behavior remains, but a new flag is introduced to change how exit codes are handled." I am sympathetic to this approach, but ultimately against it. ripgrep has an enormous number of flags, and I am trying very hard to avoid adding more of them. New flags like this that target a niche of a niche, particularly ones that have existing work-arounds as you've documented, are generally not ones that I want to add.

---

_Comment by @dubrowgn on 2023-05-15 06:07_

I apologize if I offended you, as that is not my intent. I am sincerely just trying to make a rationale argument.

Maybe we can clear up a lot of this by taking grep off the table. I was only trying to address a concern you raised:

>  It's certainly not going to change for grep because of backcompat, and differing from grep on this particular point would be massively confusing.

Which I took to mean, "we can't do it differently because of grep." If my argument seemed dogmatic or straw-man, maybe we can just agree this isn't about grep and move on. I probably just misunderstood.

I appreciate your response on the other points. Aside from avoiding any change in behavior at all, I would be curious is there is a counter argument against making `-q` an synonym for `--matches`.

---

_Comment by @BurntSushi on 2023-05-15 11:46_

> I apologize if I offended you, as that is not my intent.

I'm not offended! I wasn't personally insulted by your comments. It's not about taking offense. It's about being frustrated with the arguments being put forth. My previous comment was meant to tease them apart to show why they aren't good arguments. And those types of arguments are incredibly tedious to tease apart.

> Which I took to mean, "we can't do it differently because of grep."

No... You're dropping at least two critical parts from that summarization. Try this: "we can't do it different because of grep *in this specific example* because *BurntSushi believes* it will cause more confusion than it's worth." Your short summary is dangerously imprecise because it gives the impression that we can't do *anything* differently from grep. But that's just not true. The reality is that the existence of grep and how it works has a non-absolutist influence on the design and behavior of ripgrep.

I guess we can take grep off the table, but it's a pretty critical part of my argument here.

> Aside from avoiding any change in behavior at all, I would be curious is there is a counter argument against making `-q` an synonym for `--matches`.

I think everything I've said is a counter argument to this? Presumably your proposal here is actually something more like this:

* When the `-q/--quiet` flag is not present, ripgrep will exit with code `0` even when no matches are found.
* When the `-q/--quiet` flag is present, ripgrep behaves as it does today, by returning an exit code `1` when no matches are found.

In other words, it's changing the default behavior of ripgrep. If anyone is using `rg` in a shell script and relying on exit code `1` for the case when no matches are found *without* the `-q/--quiet` flag, then their scripts will almost certainly be broken by this change. And it would be a potentially silent breakage, which is the worst kind.

Moreover, changing the exit code behavior based on whether `-q/--quiet` is given seems quite weird and confounding to me. Imagine someone trying to debug how ripgrep behaves in a script and them forgetting to pass `-q/--quiet` when debugging. They won't see the same kind of exit codes that they're script will see. And they would understandably not pass `-q/--quiet` because it doesn't look like a flag that changes the exit code behavior.

I'll stop here because I really do feel like I've addressed this proposal already by talking above about changing the default behavior of ripgrep.

---

_Comment by @dubrowgn on 2023-05-15 13:57_

> It's about being frustrated with the arguments being put forth ... they aren't good arguments.

I'm sorry to have wasted your time.

---
