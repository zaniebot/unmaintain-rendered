---
number: 3929
title: Real-time code changes cannot be detected in Docker. (--watch)
type: issue
state: open
author: ngnl0
labels:
  - bug
assignees: []
created_at: 2023-04-11T04:18:08Z
updated_at: 2023-04-17T18:17:58Z
url: https://github.com/astral-sh/ruff/issues/3929
synced_at: 2026-01-10T01:22:42Z
---

# Real-time code changes cannot be detected in Docker. (--watch)

---

_Issue opened by @ngnl0 on 2023-04-11 04:18_

Summary: Ruff check <src.py> --watch not working under Docker 

Detailed information: I am developing using Docker+Poetry+Python, but I found the ruff check <src.py> --watch mode does not detect file changes in real time when running under Docker. 

Reproduction steps: Create a container image based on python:3.11-bullseye and run it in sleep infinity mode, use ruff check <src> --watch to detect code changes in real time. 

Version information: Ruff 0.0.261, Python 3.11-bullseye.

---

_Label `bug` added by @charliermarsh on 2023-04-11 04:24_

---

_Comment by @charliermarsh on 2023-04-11 04:25_

Oh interesting, I've never tried this. To confirm my understanding: you're editing and saving files within the Docker container, and Ruff is also running within the Docker container. Is that right?

---

_Comment by @ngnl0 on 2023-04-11 04:43_

> 哦，有趣，我从来没有试过这个。为了确认我的理解：您正在 Docker 容器中编辑和保存文件，Ruff 也在 Docker 容器中运行。是对的吗？

Yes, the first error was that I used poetry run ruff check in the docker container but it didn't work, so I suspected that it was poetry and abandoned poetry run and used the ruff check command directly in the container, and found that it still didn't work

---

_Comment by @evanrittenhouse on 2023-04-12 03:41_

Hmm, I'm seeing errors pop up successfully when I try to repro this. I pulled down `python:3.11-bullseye`, then attached to the session, created `test.py`, and ran `ruff check --watch test.py`.  The linter started with no errors.

In a separate terminal, I edited `test.py` to have:
```python
x = ["1']
```
and `E999` (syntax error) popped up on my terminal running Ruff. I do notice that changing the file several times doesn't update the watch. Essentially, the watch is only picking up the first change made during its lifetime. Is this the behavior that you're seeing, or is your Ruff instance not picking up anything at all?

---

_Comment by @ngnl0 on 2023-04-13 06:46_

> Hmm, I'm seeing errors pop up successfully when I try to repro this. I pulled down `python:3.11-bullseye`, then attached to the session, created `test.py`, and ran `ruff check --watch test.py`. The linter started with no errors.
> 
> In a separate terminal, I edited `test.py` to have:
> 
> ```python
> x = ["1']
> ```
> 
> and `E999` (syntax error) popped up on my terminal running Ruff. I do notice that changing the file several times doesn't update the watch. Essentially, the watch is only picking up the first change made during its lifetime. Is this the behavior that you're seeing, or is your Ruff instance not picking up anything at all?

Yes. If you restart Ruff in the original terminal after making changes to the file, while ensuring that the Docker container is still running, then the --watch option will work normally until the container is paused or terminated. I'm very curious about the root cause of this bug. Later today, I will record a short video to reproduce the issue I encountered.Also, thank you very much for the solution to the problem.

---

_Comment by @evanrittenhouse on 2023-04-13 13:35_

That'd be great, thanks!

---

_Comment by @dhruvmanila on 2023-04-13 13:46_

I'm unable to reproduce this. If possible, can you provide the `Dockerfile` content, command used to run the container and how are you editing the files (from the container or attaching a volume).

***These steps assumes your working directory to be: `/tmp/ruff-docker`***

### Case 1: Editing the file inside the container

1. `Dockerfile` content:
```dockerfile
FROM python:3.11-bullseye

WORKDIR /root/ruff-docker
RUN touch t.py
RUN python -m pip install --no-cache ruff

CMD [ "ruff", "check", "--watch", "t.py" ]
```

2. Build the image using:
```console
docker build -t repro/ruff-docker:latest .
```

3. Run the container using:
```console
docker run --rm -it --name=ruff-docker repro/ruff-docker:latest
```

4. In a separate terminal access the docker container using:
```console
docker exec -it ruff-docker bash
```

5. Run this command to update the file which should throw a `SyntaxError`:
```console
echo "import" > t.py
```

### Case 2: Attaching a volume, updating the file outside the container

1. Remove the following line from the `Dockerfile`
```dockerfile
RUN touch t.py
```

2. Rebuild the image using step 2 mentioned above

3. Run the container using:
```console
docker run --rm -it -v /tmp/ruff-docker:/root/ruff-docker --name=ruff-docker repro/ruff-docker:latest
```

4. Update the content of `/tmp/ruff-docker/t.py`

---
Stop the container using:
```console
docker container stop ruff-docker
```


---

_Comment by @evanrittenhouse on 2023-04-13 17:02_

> If possible, can you provide the Dockerfile content, command used to run the container and how are you editing the files (from the container or attaching a volume).

Agreed with @dhruvmanila, this information would probably be more helpful than a video

---

_Comment by @ngnl0 on 2023-04-15 14:58_

Here is my dockerfile file
```Dockerfile
# base image
FROM python:3.11-bullseye

# install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# add poetry to the PATH environment variable
ENV PATH="/root/.local/bin:${PATH}"

# install the zsh shell and set it as the default shell
RUN apt update && \
    DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends zsh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \ 
    chsh -s $(which zsh)

# install oh-my-zsh (quiet install)
RUN curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -o install.sh && \
    sh install.sh --unattended

# add poetry's complementary suggestions to oh-my-zsh
RUN mkdir /root/.oh-my-zsh/custom/plugins/poetry && \
    poetry completions zsh > /root/.oh-my-zsh/custom/plugins/poetry/_poetry
```

And my compose file
```yaml
services:
  app:
    entrypoint:
    - sleep
    - infinity
    build:
      context: ./docker/app
      dockerfile: Dockerfile
    init: true
    volumes:
    - type: bind
      source: /var/run/docker.sock
      target: /var/run/docker.sock
```

I am sorry that due to some unforeseen circumstances, I was unable to fulfil my commitment in a timely manner.

---

_Comment by @ngnl0 on 2023-04-15 14:58_

Here is a demo video of the error I encountered

https://user-images.githubusercontent.com/57034574/232232561-7da28f63-3de9-45af-8703-525f17e05aac.mp4



---

_Comment by @evanrittenhouse on 2023-04-16 00:06_

Thanks for the information @ngnl0! That looks what I was experiencing as well.

Since the diagnostics are found initially, but not on update, I think that [this loop](https://github.com/charliermarsh/ruff/blob/main/crates/ruff_cli/src/lib.rs#L210) isn't catching `rx.recv()` events. The `recv()` call [blocks](https://docs.rs/crossbeam-channel/latest/crossbeam_channel/struct.Receiver.html#method.recv) until it sees another message, but throws if the channel closes.

---

_Comment by @evanrittenhouse on 2023-04-16 17:47_

It seems like I can replicate this on my native Ubuntu 22.04 setup **without** running it in a Docker container. Very strange.

1. Create empty file called `test.py`
2. Run `ruff check test.py --watch`
3. Edit `test.py` to `import math` - should trigger unused imports error
4. Unused import error comes in as expected
5. Remove `import math`
6. Ruff doesn't update the screen - the unexpected import error remains.

@dhruvmanila Are you able to repro that behavior? I'm surprised that it isn't working even on native Linux

E: Keeping for posterity, but it looks like Neovim actually emits events on swapfiles or something, not the actual `test.py` file.
~~On Linux, `notify` uses [`INotifyWatcher`](https://github.com/notify-rs/notify/blob/main/notify/src/lib.rs#L187), monitoring [inotify](https://man7.org/linux/man-pages/man7/inotify.7.html) syscalls, to read file system events. Using [`inotify-tools`](https://github.com/inotify-tools/inotify-tools) (used `inotifywait -m test.py` to monitor) to check the events myself, it seems like the events aren't getting emitted when I expect them to. They get emitted after the first save, but not after any subsequent ones.~~



---

_Comment by @dhruvmanila on 2023-04-17 17:37_

> I am sorry that due to some unforeseen circumstances, I was unable to fulfil my commitment in a timely manner.

No need to be sorry, we're all doing this in our free time :)

> @dhruvmanila Are you able to repro that behavior? I'm surprised that it isn't working even on native Linux

I'm unable to reproduce it using the mentioned steps :(
I'm on a mac machine with 13.0. I tried it in WSL as well but it works as expected.

Now, back to @ngnl0 issue -- using the provided `Dockerfile` and `docker-compose.yml` file, I tried the following but still unable to reproduce the error:
1. Build the image
2. Run using `docker-compose up -d`
3. Open VSCode and, using the official Docker extension, connect to the shell
4. Initialize a project with `poetry` and `ruff`
5. Run `ruff` in watch mode
6. Update the file through VSCode

A few things I'm noticing in the video:
* Running on Windows (will try this on a windows machine)
* Using Docker dev environment -- If possible, could you provide a way to replicate the environment?



---

_Comment by @evanrittenhouse on 2023-04-17 17:41_

I'm thinking it may be Windows. Docker will use the host kernel (in @ngnl0's case, Windows) to process kevents. I think there may be an issue with Windows trying to process the Linux-specific inotify event, but need to check into it more

---

_Comment by @dhruvmanila on 2023-04-17 18:17_

> I'm thinking it may be Windows.

Yes, it is Windows.

I can reproduce this issue but some unexpected things are going on:
* The cache is behaving incorrectly as I'm getting diagnostics for the file content from an earlier revision.
* The `--watch` flag works on the first save, then it stops refreshing from all the subsequent saves.

I'm using the same dockerfile and compose content as mentioned by the author and running it using `docker-compose up -d`. I'm connected to the container using the shell and opened a test file in VSCode.

---
