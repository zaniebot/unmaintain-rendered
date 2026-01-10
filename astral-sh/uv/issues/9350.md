```yaml
number: 9350
title: "Generate AWS lambda zips using `uv build`"
type: issue
state: open
author: kishaningithub
labels:
  - enhancement
  - needs-design
  - external
assignees: []
created_at: 2024-11-22T09:08:47Z
updated_at: 2025-09-25T19:04:50Z
url: https://github.com/astral-sh/uv/issues/9350
synced_at: 2026-01-10T03:23:53Z
```

# Generate AWS lambda zips using `uv build`

---

_Issue opened by @kishaningithub on 2024-11-22 09:08_

Generating python lambda zips is a cumbersome process. It would be great if `uv build` can have the ability to generate AWS Lambda zip files which can be used to create lambda function in AWS.

AWS Documentation Link: https://docs.aws.amazon.com/lambda/latest/dg/python-package.html#python-package-create-dependencies

---

_Comment by @samypr100 on 2024-11-23 02:49_

Could you expand on how are you currently using `uv` to generate these?

---

_Comment by @kishaningithub on 2024-11-23 04:05_

My shell script does the following 

- Use uv to build wheel file
- Extract the wheel file
- Copy contents of the wheel into a directory
- ~~Copy~~ _Install_ project runtime dependencies into the same directory _using `pip install --target`_

- Zip the directory 
- Upload the zip to AWS



---

_Comment by @samypr100 on 2024-11-23 16:32_

Is there a reason you wouldn't be able to use `--target` like the AWS lambda guide says? I'm trying to understand if there's a compatibility gap with what pip enables you to do.

---

_Comment by @kishaningithub on 2024-11-24 02:05_

The 4th point of `copying runtime dependencies` is exactly a `pip install --target`. I guess "installing" would have been a better word than "copying".. will update the above 

---

_Label `enhancement` added by @samypr100 on 2024-11-24 04:20_

---

_Comment by @zanieb on 2024-11-26 00:13_

At the very least it'd be nice to include some documentation for this process.

I'm not sure how we'd expose this if we did. An alternative `uv build` output format? 

---

_Label `needs-design` added by @zanieb on 2024-11-26 00:13_

---

_Comment by @doron98 on 2024-11-27 10:06_

What is tricky is including path dependencies https://github.com/sst/sst/pull/5132

---

_Comment by @kishaningithub on 2024-11-27 14:51_

@zanieb In my mind it was an alternative uv build output format

---

_Comment by @williamrjribeiro on 2024-11-28 01:29_

Hi @kishaningithub , I'm struggling with this issue as well. Can you share your shell script please?

---

_Comment by @kishaningithub on 2024-12-09 10:18_

@williamrjribeiro The script i am using is basically the same as the in the [AWS documentation](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html#python-package-create-dependencies). Can you let know the issue you are facing?

---

_Comment by @toddlers on 2024-12-16 13:38_

here is something worked out for me @williamrjribeiro  , I didn't try to use `pip`, because that's what I am running away from.

```sh
#!/usr/bin/env bash

# Exit immediately if a command exits with a non-zero status
set -e

# Step 1: Clean up old builds
echo "Cleaning up old builds..."
rm -rf build dist lambda_package lambda_function.zip

# Step 2: Verify the virtual environment exists
if [ ! -d ".venv" ]; then
    echo "Error: Virtual environment '.venv' not found!"
    exit 1
fi

# Step 4: Build the wheel file using `uv`
echo "Building the wheel package..."
uv build --wheel


# Step 3: Use `uv run` to determine the Python version
echo "Detecting Python version..."
PYTHON_VERSION=$(echo "import sys; print(f'{sys.version_info.major}.{sys.version_info.minor}')"|uv run -)
if [ -z "$PYTHON_VERSION" ]; then
    echo "Error: Could not determine Python version!"
    exit 1
fi
echo "Detected Python version: $PYTHON_VERSION"


# Step 4: Locate site-packages directory
VENV_SITE_PACKAGES=".venv/lib/python$PYTHON_VERSION/site-packages"
if [ ! -d "$VENV_SITE_PACKAGES" ]; then
    echo "Error: Could not locate site-packages in the virtual environment at $VENV_SITE_PACKAGES!"
    exit 1
fi


# Step 5: Extract the wheel file
echo "Extracting the wheel file..."
WHEEL_FILE=$(ls dist/*.whl | head -n 1)  # Get the first wheel file
if [ -z "$WHEEL_FILE" ]; then
    echo "Error: No wheel file found in 'dist/' directory!"
    exit 1
fi
mkdir -p lambda_package
unzip -q "$WHEEL_FILE" -d lambda_package

# Step 6: Copy dependencies from the virtual environment
echo "Copying dependencies from virtual environment..."
cp -r "$VENV_SITE_PACKAGES/"* lambda_package/

# Step 7: Package the Lambda deployment
echo "Packaging Lambda deployment..."
cd lambda_package

# here using -u option helped me and it speed up the zip creation too
# Replace (update) an existing entry in the zip archive only if it has been 
# modified more recently than the version already in the zip archive

zip -qr ../lambda_function.zip .
cd ..

echo "Lambda deployment package created successfully: lambda_function.zip"

# Step 8: Upload to AWS Lambda
LAMBDA_FUNCTION_NAME="delete_me"  # Replace with your Lambda function name
echo "Uploading to AWS Lambda: $LAMBDA_FUNCTION_NAME"
aws lambda update-function-code --function-name "$LAMBDA_FUNCTION_NAME" --zip-file fileb://lambda_function.zip

echo "Deployment complete!"
```



---

_Comment by @williamrjribeiro on 2024-12-18 00:18_

@toddlers Thank you for the script! 🙏 

I'm using SST+Pulumi to deploy my little Python script to a lambda function. The lambda fails when it runs with error `[ERROR] Runtime.ImportModuleError: Unable to import module 'main': No module named 'main' Traceback (most recent call last):`

Here's my `pyporject.toml`:

```toml
[project]
name = "poc-events_python-consumer"
version = "0.0.0"

dependencies = [
    "sst",
    "pydantic>=2.10.2",
    "cloudevents[pydantic]",
]

requires-python = "~=3.12"

[tool.uv.pip]
no-strip-extras = true

[tool.uv.sources]
sst = { git = "https://github.com/sst/sst.git", subdirectory = "sdk/python", branch = "dev" }
cloudevents = { git = "https://github.com/cloudevents/sdk-python.git", branch = "main" }

[tool.setuptools]
py-modules = []

[project.scripts]
handler = "lambda:handler"
```

The script path is `python-consumer/src/lambda/__init__.py` which has the `def handler` function.

This is how the lambda function is created with Pulumi:

```ts
const pythonConsumer = new aws.lambda.Function("PocEventPythonConsumer", {
      role: lambdaRole.arn,
      runtime: "python3.12",
      handler: "lambda.handler",
      code: $asset("packages/python-consumer/lambda_package/"),
    });
```

Maybe I should just wait for https://github.com/sst/sst/pull/5132 to be merged and released.

---

_Comment by @kucharzyk-sebastian on 2025-01-04 11:41_

In case anyone is looking for a solution using Python, UV, and AWS CDK, it seems they're going to support it in [#31238](https://github.com/aws/aws-cdk/issues/31238)

---

_Comment by @eidorb on 2025-02-19 23:59_

This will be nice.

I hacked together a bundler using Poetry some time ago when arm64 Lambda functions dropped. It bundled arm64 Lambda functions (with pure Python dependencies) on non-arm platforms, without the Docker overhead. CDK bundled the result with `aws_lambda.Code.from_asset()`.

```python
def bundle():
    """Bundles lambda function to lambda/dist/lambda-bundle."""
    # The target is relative to this directory.
    target_path = Path(__file__).parent / "lambda/dist/lambda-bundle"

    # Build a Python wheel. It appears in lambda/dist/lambda-0.1.0-py3-none-any.whl.
    subprocess.run("poetry build --format wheel".split(), cwd="lambda")

    # Delete the target directory if it exists.
    try:
        shutil.rmtree(target_path)
    except FileNotFoundError:
        pass

    # Install the wheel to the target directory. This installs the wheel AND its
    # dependencies.
    #
    # Becaouse our Lambda functions run on arm64 architecture, we specify the
    # manylinux2014_aarch64 platform. This installs Linux aarch64 packages,
    # regardless of the platform that bundles the code. i.e., bundling on Windows
    # will result in the desired Linux aarch64 packages.
    subprocess.run(
        [
            "poetry",
            "run",
            "pip",
            "install",
            "dist/lambda-0.1.0-py3-none-any.whl",
            "--target",
            target_path,
            "--platform",
            "manylinux2014_aarch64",
            "--only-binary",
            ":all:",
        ],
        cwd="lambda",
    )
```




---

_Label `external` added by @samypr100 on 2025-02-26 04:06_

---

_Comment by @maruryota on 2025-05-27 09:21_

@kishaningithub If you're using an OS other than Linux, try changing @toddlers 's Step 6 as follows:

```bash
# Step 6: Install dependencies
echo "Creating requirements.txt..."
uv export --frozen --no-emit-workspace --no-dev --no-editable -o requirements.txt

echo "Installing dependencies for Linux (Lambda runtime)..."
uv pip install \
    --no-installer-metadata \
    --no-compile-bytecode \
    --python-platform x86_64-manylinux2014 \
    --python $PYTHON_VERSION \
    --target lambda_package \
    -r requirements.txt
```

I'm using macOS and this worked for me.

---

_Comment by @Mjb141 on 2025-09-13 18:25_

Feels like we shouldn't have to do all that to build a zip for Lambda.  Poetry has [plugin support](https://github.com/micmurawski/poetry-plugin-lambda-build) for this.

---

_Renamed from "[Feature request] Generate AWS lambda zips using `uv build`" to "Generate AWS lambda zips using `uv build`" by @konstin on 2025-09-25 19:04_

---
