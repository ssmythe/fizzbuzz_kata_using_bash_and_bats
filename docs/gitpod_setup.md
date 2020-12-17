# gitpod.io setup

In order to run this demo in gitpod.io, you need a few things added to your repo

## .gitpod.yml

This is the main config file for gitpod and points to all the other config files

```yaml
image:
  file: .gitpod.Dockerfile

tasks:
  - init: 'echo auto-running tests'
    command: 'watch bats --tap .'
```

## .gitpod.Dockerfile

This is the main Dockerfile gitpod uses to prepare your environment.
We need to install bats here.

```yaml
FROM gitpod/workspace-full

# Install custom tools, runtime, etc.
RUN brew install bats
```

Once those files are in place, you should be able to use:

https://gitpod.io/#YOUR_REPO_URL_HERE

...to start up the session.

# Tip: Enable Auto Save

Navigate to File > Auto Save (and make sure it's checked)

You'll notice a directory structure created because of this:
.theia/settings.json

settings contains:
```json
{
    "editor.autoSave": "on"
}
```

# Tip: use watch to auto run your tests

The startup command will start watch automatically.

If you ever need to restart it, in the command line, just type
```bash
watch bats --tap .
```

This will enable auto run of your BATS tests every time a file changes.

To exit watch, just use ctrl-C.
