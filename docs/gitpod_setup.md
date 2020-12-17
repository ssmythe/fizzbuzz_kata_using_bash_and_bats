# gitpod.io setup

In order to run this demo in gitpod.io, you need a few things added to your repo

## .gitpod.yml

This is the main config file for gitpod and points to all the other config files

```yaml
 image:
   file: .gitpod.Dockerfile
 
 tasks:
   - init: 'echo "TODO: Replace with init/build command"'
     command: 'echo "TODO: Replace with command to start project"'
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
