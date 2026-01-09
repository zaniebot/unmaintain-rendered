---
number: 3667
title: Support for nested classes
type: issue
state: closed
author: ShalokShalom
labels: []
assignees: []
created_at: 2023-03-22T14:37:21Z
updated_at: 2023-03-23T13:19:57Z
url: https://github.com/astral-sh/ruff/issues/3667
synced_at: 2026-01-07T13:12:14-06:00
---

# Support for nested classes

---

_Issue opened by @ShalokShalom on 2023-03-22 14:37_

I know that nested classes are usually not considered to be pythonic, or usable at all. 
I actually found a use, and suggest to consider them:

![Screenshot_20230322_153212](https://user-images.githubusercontent.com/6344099/226937354-7619905c-fa3c-4245-a22a-5ee4265b56d9.png)

@choice implements choice classes, also called [tagged unions](https://en.wikipedia.org/wiki/Tagged_union), and I like to group them together, in order to increase readability. 

The error shows doesn't take this into account, since the nested classes seem to be considered, as you see. 

It thinks Exit is unused, and therefor doesn't assume, that it might be an indentation error. 
I understand, if you think this to be the edge case it might be, and the error to be accurate enough in this case. Still, I think its worth acknowledging this issue. 

I appreciate your response. :)


---

_Comment by @JonathanPlasse on 2023-03-22 17:08_

```python
@choice
class ChoiceExit:
    class PassableExit: ...
    class LockedExit: ...
    class NoExit: ...

Exit = PassableExit | LockedExit | NoExit
```
Would the code above work for you?
The problem is that you are overriding Exit.

---

_Comment by @ShalokShalom on 2023-03-23 12:14_

Well, idk to be honest. I want to create a discriminated union.

That is an Enum in Rust 😃 

I would probably prefer, to have an unified name for both, the outer class, and the union of the three inner classes.

I guess it would become confusing to refer to them, otherwise. 

I could also see implementing @choice via init_subclass, but that is not your concern. 
I was more intending, to make you aware of the lack of consideration of the usage of nested classes in general. 

I know that your example would work, but the linter doesn't give any hint about this.
I guess there are cases, where indenting the bottom line would lead to what the programmer actually wants. And in this case, the current warning doesnt suggest this. 

---

_Comment by @JonathanPlasse on 2023-03-23 12:32_

Can you provide the link to `@choice`?

---

_Comment by @ShalokShalom on 2023-03-23 12:36_

That's not implemented yet, also because I dont know, how I should implement it. 😄 

The plan was to inject the Union building structure, and eventually including the functionality of @final. 

But I am very new to Python, and try to bring over the DUs from FSharp, to continue this style of programming. It seems like I am going against the common coding practices in the Python community. 

There is also no real maintained, properly ergonomic sumtype implementation, as far as I see. 
Do you know more about that? 😄  

---

_Comment by @JonathanPlasse on 2023-03-23 12:39_

```python
@choice
class Exit:
    class PassableExit: ...
    class LockedExit: ...
    class NoExit: ...

SomeExitPattern = Exit.PassableExit | Exit.LockedExit | Exit.NoExit
```
Why not this?

---

_Comment by @ShalokShalom on 2023-03-23 13:19_

Yeah, that would also be an option. 

---

_Closed by @ShalokShalom on 2023-03-23 13:19_

---
