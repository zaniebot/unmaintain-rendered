```yaml
number: 13252
title: "Add --docker flag to `uv init` to create Dockerfile"
type: issue
state: open
author: jackwotherspoon
labels:
  - enhancement
assignees: []
created_at: 2025-05-01T13:55:00Z
updated_at: 2025-05-03T21:13:41Z
url: https://github.com/astral-sh/uv/issues/13252
synced_at: 2026-01-12T16:01:23Z
```

# Add --docker flag to `uv init` to create Dockerfile

---

_@jackwotherspoon_

### Summary

For developing applications with `uv` that are intended to be deployed to serverless environments, it would be awesome if I could run `uv init --docker` and get `Dockerfile` and `.dockerignore` files generated that follow uv best practices. 

These could be taken from [uv-docker-example](https://github.com/astral-sh/uv-docker-example).

This would greatly speed up development and would make `uv` even more usable! 😄 

### Example

Addition of new `--docker` flag to the `uv init` command.

```sh
# generate uv project with additional Dockerfile and .dockerignore
uv init --docker
```

---

_Label `enhancement` added by @jackwotherspoon on 2025-05-01 13:55_

---

_Comment by @jackwotherspoon on 2025-05-01 13:59_

@zanieb I would be curious what you think of this idea?

It would allow a project to be created and deployed with uv with basically zero lift from a user's perspective 😄

Happy to potentially work on it if there is interest  

---

_Comment by @zanieb on 2025-05-01 18:17_

I'm pretty interested, I think it's a good idea. I worry a little about maintaining it. Let me see what the rest of the team thinks.

---

_Comment by @Gankra on 2025-05-01 18:27_

I think this kind of thing is really reasonable as long as it's indeed an `init` thing and not something we are expected to keep sync'd as your project changes. Hard to say how useful it would be, but I could imagine it is.

My experience with this kind of thing is that it's trivial to make *a* perfectly reasonable barebones Docker image, but that devs are gonna have a bunch of opinions or customizations they'll want to make, and anything but "cool now go ahead and hand-edit this" is a mess.

---

_Comment by @jackwotherspoon on 2025-05-01 21:51_

@Gankra @zanieb Totally agree, I only thought of it for the `init` command, as it would be too hard to maintain otherwise like you mentioned.

> My experience with this kind of thing is that it's trivial to make a perfectly reasonable barebones Docker image, but that devs are gonna have a bunch of opinions or customizations they'll want to make, and anything but "cool now go ahead and hand-edit this" is a mess.

Yes I guess this is the challenge, I was really just thinking we could pull from [uv-docker-example](https://github.com/astral-sh/uv-docker-example) but you are right that everyone has opinions on things and it could become a headache if its not properly documented as a barebones template 😆 

---

_Comment by @orishamir on 2025-05-03 21:13_

This could be really cool, as it could serve as a good way of promoting best practices for using uv with Docker

---
