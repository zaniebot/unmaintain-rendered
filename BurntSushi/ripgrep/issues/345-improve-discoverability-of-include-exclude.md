```yaml
number: 345
title: improve discoverability of include/exclude functionality
type: issue
state: closed
author: ssokolow
labels:
  - question
  - doc
assignees: []
created_at: 2017-01-31T11:59:28Z
updated_at: 2017-03-13T00:31:23Z
url: https://github.com/BurntSushi/ripgrep/issues/345
synced_at: 2026-01-12T16:13:21Z
```

# improve discoverability of include/exclude functionality

---

_@ssokolow_

When using `ack`, I often want to ignore some pattern just once.

In order to get ripgrep's UX (User eXperience) to match ack's on everything but performance, I had to write a shell wrapper function (so the wrapper adds as little overhead as possible) which parses the command-line and re-serializes it, just so that I can get the functionality that tools like ack (`--ignore-dir=DIRNAME`) and grep (`--exclude=GLOB`) give me for free.

Here it is pruned down to the relevant bit. (It also implements a `~/.rgrc` to complement `~/.ackrc`)

```sh
function rg() {
    declare -a OUTPUT
    declare -a IGNORES

    for ARG in "$@"; do
        if [[ "$ARG" = --ignore=* ]]; then
            IGNORES+=( ${ARG#*=} )
        elif [ "$ARG" = "--ignore" ]; then
            take_next=1
        elif [ "$take_next" = 1 ]; then
            IGNORES+=( "$ARG" )
            unset take_next
        else
            OUTPUT+=( "$ARG" )
        fi
    done

    if [ ${#IGNORES[@]} -gt 0 ]; then
        command rg --ignore-file <(printf '%s\n' "${IGNORES[@]}") "${OUTPUT[@]}"
    else
        command rg "${OUTPUT[@]}"
    fi
}
```

Regardless of your own opinions, this sort of "being less functional than competitors in easy-to-trip-over ways" behaviour is bad PR. If I had an SSD, I'd just have dismissed ripgrep as "obviously not yet mature enough" and moved on.

---

_Comment by @BurntSushi on 2017-01-31 12:19_

> Regardless of your own opinions, this sort of "being less functional than competitors in easy-to-trip-over ways" behaviour is bad PR. If I had an SSD, I'd just have dismissed ripgrep as "obviously not yet mature enough" and moved on.

Can you leave this kind of unconstructive commentary at the door please? It immediately puts me on the defensive and makes it harder to have a productive conversation with you. I don't know what you mean by `"being less functional than competitors in easy-to-trip-over ways" behaviour is bad PR`. I don't know what an SSD has to do with this.

> just so that I can get the functionality that tools like ack (--ignore-dir=DIRNAME) and grep (--exclude=GLOB) give me for free.

Could you please explain why you can't use ripgrep's own support for this feature? Why is the `-g/--glob` flag insufficient? Or did you not realize it was there? How could I improve the man page and/or the whirlwind tour so that you would have seen it?

---

_Label `doc` added by @BurntSushi on 2017-01-31 12:20_

---

_Label `question` added by @BurntSushi on 2017-01-31 12:20_

---

_Comment by @ssokolow on 2017-01-31 13:13_

> Can you leave this kind of unconstructive commentary at the door please? It immediately puts me on the defensive and makes it harder to have a productive conversation with you. I don't know what you mean by "being less functional than competitors in easy-to-trip-over ways" behaviour is bad PR. I don't know what an SSD has to do with this.

In hindsight, you're absolutely right and I should have been more diplomatic. Being tired enough to compromise your judgement is insidious when you're not very in touch with your emotions and have no social intuition because nothing feels off until someone else bites your head off for letting your frustrating leak into things.

I'll try to clarify my points so that the constructive aspects I intended come through:

* To make sure we're on the same page, when I talk about PR, I'm referring to the fact that any project intended as more than purely "I scratched my own itch. Here's a code dump" should be thinking about how it will be perceived in order to avoid driving away users who assign value differently than the author. (ie. "Trivial thing X"  being a deal-breaker.)

* When I referred to SSDs, I was saying that, for users who have already customized a competing tool, the up-front cost of switching is going to be at the front of their mind, likely making ripgrep's speed into its biggest selling point... and that will be heavily influenced by seek time of the underlying device if the cache isn't hot.

Part of the frustration that I accidentally let leak through came from an impression of "First, there's no `.ackrc` equivalent, and now *this*!?" that I had built up.

> Could you please explain why you can't use ripgrep's own support for this feature? Why is the -g/--glob flag insufficient? Or did you not realize it was there? How could I improve the man page and/or the whirlwind tour so that you would have seen it?

I didn't know about it.

* The README is a big thing with no hyperlinked table of contents and my Firefox is overdue for a restart, so I was relying purely on `rg --help | less` because it's more responsive and avoids things like the build instructions.
* When I'm searching a large, non-hypertext thing like `man zshall` or long `--help` output, I've built a habit of prefixing my searches with a dash or `^\s+` as appropriate to get a poor man's equivalent to checking a book's index. (which means that `--glob` wouldn't turn up in the results)
* All of the other tools I have experience with use `ignore` (ack) or `exclude` (grep, rsync, etc.) as their terminology.
* When I saw `--ignore-file`, I fixated on the idea that you'd settled on that for terminology and, when I didn't find something like `--ignore=GLOB`, I didn't think to go further than also searching for `exclude` "to play it safe".

That said, using `!` to negate a pattern typed at the command-line is a big pain because of how it interacts with zsh command history if you forget to escape it (You get an "event not found" error when you press <kbd>Enter</kbd> and the command history is truncated to just before the `!`) so I'll adjust my wrapper to map `-G <foo>` to `-g !<foo>` and use that instead.

(It also doesn't help that it ensures the need to chord at least one character in an exclusion, but `-G` doesn't fix that.)

---

_Comment by @BurntSushi on 2017-02-02 12:12_

> In hindsight, you're absolutely right and I should have been more diplomatic. Being tired enough to compromise your judgement is insidious when you're not very in touch with your emotions and have no social intuition because nothing feels off until someone else bites your head off for letting your frustrating leak into things.

I understand. It happens. :-)

> should be thinking about how it will be perceived in order to avoid driving away users who assign value differently than the author

I understand that. But this doesn't help me at all because I think I'm already doing it. The presence or absence of features alone isn't enough to determine my thought processes.

I will counter with a *should* of my own: when reporting bugs, drop the meta commentary. If you have a serious problem with how the project is being run, then open a new issue and address it directly. Please. Otherwise, it's just a distraction.

(I **hate** being told what I *should* be doing, as if there were some Clearly One Correct Way to run a project. Let's try to focus on the actual user experience and work to improve it instead.)

> When I referred to SSDs, I was saying that, for users who have already customized a competing tool, the up-front cost of switching is going to be at the front of their mind, likely making ripgrep's speed into its biggest selling point... and that will be heavily influenced by seek time of the underlying device if the cache isn't hot.

ripgrep's speed *when the cache is hot* is the primary thing that I've benchmarked. If your search needs to do slow I/O, the relative performance of your chosen search tool matters a lot less. So it seems to me like the opposite is intended: if you have spinning rust and your searches always hit disk, then ripgrep probably isn't going to be a huge win for you. But if you have SSDs, lots of RAM or frequently search the same stuff, then ripgrep might be a big win.

> That said, using ! to negate a pattern typed at the command-line is a big pain because of how it interacts with zsh command history if you forget to escape it

Yes, I don't like it either. It has similarly undesirable effects when left unescaped in bash. The intention was to be consistent with the glob syntax found in the ignore files, and to use exactly one flag so that precedence could be accurately captured. I would support the addition of `--include`/`--exclude` flags, but we need a way to determine the global ordering of their invocation.

I'm going to change this issue to "improve discoverability of include/exclude functionality" since that seems to me to be your core issue here. A solution to this problem could include actually adding `--include/--exclude` flags or somehow making the existing functionality more discoverable.

---

_Renamed from "Support one-off file-ignoring" to "improve discoverability of include/exclude functionality" by @BurntSushi on 2017-02-02 12:12_

---

_Comment by @ssokolow on 2017-02-02 12:28_

> I will counter with a should of my own: when reporting bugs, drop the meta commentary. If you have a serious problem with how the project is being run, then open a new issue and address it directly. Please. Otherwise, it's just a distraction.
> 
> (I hate being told what I should be doing, as if there were some Clearly One Correct Way to run a project. Let's try to focus on the actual user experience and work to improve it instead.)

Having now had a better night's sleep, I can see that too. Given that, from my perspective, it's a less glaring way to screw up, I can't guarantee that I'll succeed, but I'll definitely try to find ways to buffer myself against making that mistake again.

> ripgrep's speed when the cache is hot is the primary thing that I've benchmarked. If your search needs to do slow I/O, the relative performance of your chosen search tool matters a lot less. So it seems to me like the opposite is intended: if you have spinning rust and your searches always hit disk, then ripgrep probably isn't going to be a huge win for you. But if you have SSDs, lots of RAM or frequently search the same stuff, then ripgrep might be a big win.

Fair enough. I'm not sure I've ever needed to search a codebase big enough that hot-cache `ack` didn't complete instantly. However, when I was translating my ackrc, I did notice the following advantages for ripgrep which aren't immediately obvious from the elevator pitch:

* ripgrep comes with a better set of default filetype definitions
* ripgrep's approach to `.gitignore` and friends is better aligned with my intuitive expectations
* `-g\!` is fewer characters to type than `--ignore-dir=`

> I'm going to change this issue to "improve discoverability of include/exclude functionality" since that seems to me to be your core issue here. A solution to this problem could include actually adding --include/--exclude flags or somehow making the existing functionality more discoverable.

Sounds good.

---

_Comment by @MartinBonner on 2017-03-09 10:30_

While addressing this issue, could you also please clarify whether `--glob` is in addition to `.gitignore`, or replaces it.  I would prefer "in addition" (with `-u` to switch off `.gitignore` if I don't want it). I often want to ignore both my generated make files (which are in `.gitignore`) and test directories (which obviously aren't!)


---

_Comment by @MartinBonner on 2017-03-09 10:37_

Ah.  A little bit of experimentation convinces me that rg does what I want.  Better documentation would be nice though.  (I know, I know, a pull request would be good.)


---

_Closed by @BurntSushi on 2017-03-13 00:31_

---
