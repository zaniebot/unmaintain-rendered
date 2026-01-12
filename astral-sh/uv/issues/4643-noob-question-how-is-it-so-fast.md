```yaml
number: 4643
title: "Noob Question: How is it so fast?"
type: issue
state: closed
author: drisspg
labels: []
assignees: []
created_at: 2024-06-29T05:20:39Z
updated_at: 2024-06-30T22:59:49Z
url: https://github.com/astral-sh/uv/issues/4643
synced_at: 2026-01-12T15:58:51Z
```

# Noob Question: How is it so fast?

---

_@drisspg_

First of all I wanna say thank you for making such an amazing tool! It has greatly improved my work flow so can't thank you enough! 

I am often astonished by the speed. I was curious if there is any documentation on how uv is able to be so much more performant? I assume a lot of caching but also curious if there are any algorithmic changes that enable the performance?

---

_Comment by @brianmego on 2024-06-29 16:12_

While I'm not the maintainer of this repo, I'll take a stab at answering for the team to use as a baseline.

As with many improvement stories, there's more than one factor. In the case of UV, here's a few:

1. UV is primarily written in Rust, rather than Python, with a little bit of Python sprinkled in to facilitate building it for installation with pip. Rust is fast for lots of reasons, but there's a few big ones that matter the most
    - It's compiled rather than interpreted. There's no program running that interprets a text file (with a .py ending) line by line into commands that are translated on the fly into computer instructions. Instead all those computer instruction calculations are done ahead of time in the compiler step, resulting in a program that can run without "the language" being installed. To run a Rust program, you don't need Rust installed on your system.
    - Rust makes it easier to work with lots of simultaneous instructions. With Python it's possible to make 100 simultaneous network calls, but it's a big difficult to manage and takes a fair amount of thought to get right. Rust is optimized for this type of operation.
    - Most languages are either "fast" or "safe". Python is on the "safe" side of things, with a separate piece of the code running called the Garbage Collector (GC) that keeps track of which variables no longer need memory allocated from the operating system and managing that on your behalf. Languages like C and C++ don't have a garbage collector, but require you to keep track of your own memory allocations, otherwise leading to program crashes or other bad side effects. Rust strives for both "fast" and "safe", and the tradeoff is effort up front when developing to get the program to compile in the first place. This effort is nonzero, and the primary reason why Rust is not just "better" for everything, but certainly when the effort is made the results are stunning.
2. UV does some aggressive caching on disk, as you pointed out. Though it's also a lot faster on the first run.
3. Anytime you take an older project and rework it from the ground up, you know the problem better. You are able to make better calls as to what is required and what is worth forgetting about, what the primary flows are you should optimize for and what are nice to haves that can be built later. You see this with other pip competitors, too. While poetry or pdm are not as fast as uv, and these things are sometimes subjective, I would argue the interfaces of both are more thoughtful and slick than pip, which is evolving rather than starting with a green field.
4. uv doesn't do as much as pip, broadly speaking. There's many years of deprecated features that pip continues to support, I'm sure much to the chagrin of the developers. It's not that more features makes something slower necessarily, but I'm certain there's stuff in pip along the lines of "make sure to check all 3 possible places for the data before making a decision".



---

_Comment by @shailen-naidoo on 2024-06-30 16:28_

@dineshtrivedi I thought you might be interested in this tool for Python 👍🏾 

---

_Comment by @charliermarsh on 2024-06-30 22:59_

I'm giving a talk on this topic (or something variant of it) at [EuroRust 2024](https://eurorust.eu/)! But @brianmego hit some of the most important ideas:

- Rust.
- Heavy use of parallelism and concurrency.
- A cache design that's optimized for warm installs (it's common to install a package more than once on a given machine, so we bias towards that use-case and make those reinstalls much faster and more efficient).

---

_Closed by @charliermarsh on 2024-06-30 22:59_

---
