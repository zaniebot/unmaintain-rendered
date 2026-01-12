```yaml
number: 9807
title: Keeping an encrypted dotenv file with uv
type: issue
state: closed
author: matejaputic
labels:
  - show-and-tell
assignees: []
created_at: 2024-12-11T11:39:46Z
updated_at: 2025-02-16T19:11:29Z
url: https://github.com/astral-sh/uv/issues/9807
synced_at: 2026-01-12T15:59:59Z
```

# Keeping an encrypted dotenv file with uv

---

_@matejaputic_

Since there's no discussion section in this repo, I thought I'd post this here. Just wanted to share this solution for keeping an encrypted dotenv file with uv that I've found useful.

## Context

Dotenv files often contain secrets, but despite good intentions, they often end up checked into a repo, which is never good. I initially searched for a solution that would allow me to run `uv` with a pre-run hook that decrypts secrets and enters them into the environment, but that doesn't exist, so here's my solution in the mean time.

SOPS is an awesome solution for keeping encrypted files, and is even compatible with teams if users share their public keys. When your dotenv file is encrypted with SOPS, you can check it into a repo and you and your teammates will be the only ones who'll be able to read it. I won't go into too much detail on SOPS here, but I'm assuming for this setup that you have SOPS installed and have a keypair set up already. In my case I'm using `age` as the key provider. See the [SOPS repo](https://github.com/getsops/sops) for docs.

## Step 1: Add a SOPS configuration

Add a file called `.sops.yaml` to your project:

```
creation_rules:
  - path_regex: \.env$
    input_type: dotenv
    age: <agekey>
```

This file configures SOPS to keep a file called `.env` encrypted with the key `<agekey>`, and that its format is of type `dotenv`.

## Step 2: Add a dotenv file

Running the `sops` command on a file decrypts the file (or creates it if it doesn't exist) and opens it in an editor. When you save and exit, SOPS encrypts and writes the file.

Run `sops --input-type ENV .env`, then populate it with some variables. I'm using some fake credentials from the SOPS documentation here.

```
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Then save and exit the editor. `cat` the file to see the encrypted contents.

## Step 3: Add a SOPS wrapper

Next add a Python script that will act as a SOPS wrapper for `uv run`. We'll use a nifty little library called [`sopsy`](https://github.com/nikaro/sopsy) that is a Python wrapper for SOPS. 

I saved the following file to `scripts/sops_wrapper.py`:

```
import argparse
import os
import sys

from sopsy import Sops


def main():
    # Parse arguments
    parser = argparse.ArgumentParser(
        description="Run a command with environment variables from a decrypted dotenv file."
    )
    parser.add_argument(
        "command", nargs=argparse.REMAINDER, help="The command to execute."
    )
    parser.add_argument(
        "--env-file",
        "--e",
        default=".env",
        help="Path to the dotenv file to decrypt (default: .env).",
    )
    args = parser.parse_args()

    # Ensure a command is provided
    if not args.command:
        print(
            "Usage: uv run sopsy [--env-file ENV_FILE] <command> [args...]",
            file=sys.stderr,
        )
        sys.exit(1)

    # Decrypt the specified dotenv file
    dotenv_path = args.env_file
    try:
        if os.path.exists(dotenv_path):
            sops = Sops(dotenv_path)
            env_vars = sops.decrypt()
            env_vars = env_vars.split()
            tmp_vars = []
            for env_var in env_vars:
                k, _, v = env_var.partition("=")
                tmp_vars.append((k, v))
            env_vars = dict(tmp_vars)
            os.environ.update(env_vars)
        else:
            print(f'Error: dotenv file "{dotenv_path}" not found.', file=sys.stderr)
            sys.exit(1)
    except Exception as e:
        print(f'Error decrypting dotenv file "{dotenv_path}": {e}', file=sys.stderr)
        sys.exit(1)

    # Extract the command and its arguments
    command = sys.argv[1:]

    # Execute the command in the modified environment
    try:
        os.execvp(command[0], command)
    except FileNotFoundError:
        print(f"Error: Command '{args.command[0]}' not found.", file=sys.stderr)
        sys.exit(127)
    except Exception as e:
        print(f"Error executing command {e}", file=sys.stderr)
        sys.exit(1)


if __name__ == "__main__":
    main()
```

The script is pretty self explanatory, but essentially it:

- Accepts a command with arguments
- Sources an encrypted dotenv file
- Delegates decryption of the file to SOPS
- Enters the variables from the file into the environment
- Executes the command in that environment

Also, you'll want to `uv add sopsy` to your project.

## Step 4: Add the wrapper to uv

Finally, add a script called `sops` to `uv` to enable the SOPS wrapper.

Add the following to your `pyproject.toml`:

```
[project.scripts]
sops = "scripts.sops_wrapper:main"
```

## Step 5: Verify that it works

As an example, let's say you have a FastAPI project that expects the credentials we entered earlier into `.env` as environment variables.

If per usual, you run:

`uv run fastapi dev`, it will complain:

```
AWS_ACCESS_KEY_ID
  Field required [type=missing, input_value={}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/missing
AWS_SECRET_KEY
  Field required [type=missing, input_value={}, input_type=dict]
    For further information visit https://errors.pydantic.dev/2.10/v/missing
```

But now if you run `uv run sops fastapi dev` instead:

```
 FastAPI   Starting development server 🚀
 ...
```

All without keeping any loose secrets in your project directory. Neato!

## Step 6: Maintaining the dotenv file

Editing the existing dotenv file is easy. Simply run `sops .env` in your project directory, and it will bring up an editor. When you save and exit, SOPS will encrypt the file, and you can run `uv run sops <your command>` again to pull in the new variables.

That's all! 

Comments/revisions/critiques welcome.

---

_Label `show-and-tell` added by @zanieb on 2025-01-07 19:10_

---

_Comment by @charliermarsh on 2025-02-16 19:11_

(Thank you for sharing :))

---

_Closed by @charliermarsh on 2025-02-16 19:11_

---
