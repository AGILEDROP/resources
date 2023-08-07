---
last_modified_on: "2023-08-07"
title: DDEV Expose Behat and PHPUnit
description: A guide on how to expose Behat and PHPUnit to the host machine when using DDEV.
series_position: null
author_github: https://github.com/admirlju
author_name: Admir Ljubijankic
tags: ["language: bash", "ddev", "behat", "phpunit"]
---

## About

This guide will show you how to expose Behat and PHPUnit to the host machine when using DDEV.
This will allow us to run tests with `ddev <command>` instead of `ddev exec <command>` or using `ddev ssh`.

## Prerequisites

- [DDEV](https://ddev.readthedocs.io/en/stable/)
- [Behat](https://docs.behat.org/en/latest/)
- [PHPUnit](https://phpunit.readthedocs.io/en/9.5/)

## Assumptions

- You have a working DDEV PHP project.
- You have Behat and PHPUnit installed inside your DDEV project.

## Steps

### 1. Create new bash scripts for Behat and PHPUnit

In your DDEV project, you should have a `.ddev/commands` directory.
This directory contains directories for each container that DDEV uses. In our case, we will be using the `web` container.

Inside the `web` directory, create two new files: `behat` and `phpunit`.

```bash
touch .ddev/commands/web/behat
touch .ddev/commands/web/phpunit
```

#### Remember

These files do not end with `.sh` extension.

At this point you can also make these files executable by running:

```bash
chmod +x .ddev/commands/web/behat
chmod +x .ddev/commands/web/phpunit
```

### 2. Edit the `behat` and `phpunit` scripts

Let's first add the necessary code and I'll explain what it does later.

#### behat:

```bash
#!/bin/bash

## Description: Run Behat inside the web container
## Usage: ddev behat [options]
## Example: ddev behat --tags @javascript
## HostWorkingDirectory: true
## ExecRaw: true
$DDEV_COMPOSER_ROOT/vendor/bin/behat "$@"
```

#### phpunit:

```bash
#!/bin/bash

## Description: Run PHPUnit inside the web container
## Usage: ddev phpunit [options]
## Example: ddev phpunit --filter testSomething
## HostWorkingDirectory: true
## ExecRaw: true
$DDEV_COMPOSER_ROOT/vendor/bin/phpunit "$@"
```

#### Explanation

- `#!/bin/bash` - This is the shebang. It tells the shell what interpreter to use when executing the script.
- `## Description: Run Behat inside the web container` - The description of the command that will be shown when running `ddev list`.
- `## Usage: ddev behat [options]` - The usage of the command that will be shown when running `ddev help behat`.
- `## Example: ddev behat --tags @javascript` - The example of the command that will be shown when running `ddev help behat`.
- `## HostWorkingDirectory: true` - Your container command will be executed in the same directory as your current host working directory.
- `## ExecRaw: true` - This will pass command arguments directly to the container. This is recommended for all container commands.
- `$DDEV_COMPOSER_ROOT/vendor/bin/behat "$@"` - The command that will be executed inside the container. `$DDEV_COMPOSER_ROOT`
is the absolute path to the composer root directory inside the container. `"$@"` will pass all arguments to the command.

### 3. Run Behat and PHPUnit

Now that we have our scripts ready, we can test them out.

```bash
ddev behat --version
ddev phpunit --version
```

You should see the output of the commands.

## Conclusion

We have successfully exposed Behat and PHPUnit to the host machine when using DDEV. Remember you can do this for any
other command you want to expose. These commands are basic bash scripts that will be executed, so you can make them as complex as you need.
You can also put commands in the global ddev scope if you want to run them from any container.

In the official DDEV documentation, you can find more useful annotations and environment variables that you can use.

## Resources

- [DDEV Documentation](https://ddev.readthedocs.io/en/latest/users/extend/custom-commands)