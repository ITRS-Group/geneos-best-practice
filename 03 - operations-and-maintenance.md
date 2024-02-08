# Geneos Best Practices - Operations and Maintenance

## Introduction

In this guide we are going to explore how to use the `geneos` command to manage your ITRS Geneos environment. This includes:

* Adding new components
* Updating installed software
* Maintaining TLS certificates and AES Encryption key files
* Cleaning up old files

You should already have the `geneos` command installed, which you can check by simply trying to run it at the command line, like this:

```bash
$ geneos

  The  geneos  program will help you manage your Geneos environment.                                                          
                                                                                                                              
  The program will help you initialise a new installation, migrate an
  old  geneos-utils  one, install and update software releases, add
  and remove instances, control processes and build template based
  configuration files for SANs and more.       
...
```

You should always try to have the latest version installed, which you can check with the `geneos version` command, like this:

```bash
$ geneos version
geneos version v1.12.0
```

To see what the current release is, visit the [cordial repo in github](https://github.com/ITRS-Group/cordial/releases).

> 💡 Future releases may include a command to check for later releases and even self-update, as long as you have access to github from your server.

If `geneos` is not installed then please follow the instructions on [github](https://github.com/ITRS-Group/cordial/tree/main/tools/geneos#getting-started) to download and install it.

> ⚠ Reminder: `geneos` will only perform operations as the user running it, so be careful using commands which elevate privileges like `sudo` and these will most likely not work as you expect.

## Common Command Usage

Like many modern CLI programs `geneos` follows a common structure of sub-commands with additional flags and arguments depending on the command being run. For `geneos` there are two core command types:

1. `geneos COMMAND [flags|arguments...]`

    For example:
    ```bash
    geneos ls
    geneos restart gateway
    geneos ps 'LDN*'
    geneos logs gateway PT1 -f
    ```

2. `geneos SUBSYSTEM COMMAND [flags|arguments]`

    For example:
    ```bash
    geneos tls ls
    geneos config show
    ```

> 💡 We follow the conventions for UNIX/Linux command documentation by enclosing optional arguments in square brackets, like this `command [ARG]`, alternatives are separated by a pipe symbol, like this `command THIS|THAT`, and multiple arguments are indicated by ellipsis, like this `command ARG...`. These can be combined like this `geneos example [flags] [TYPE] [NAME...] [PATH|URL...]`

> ☀ You can get help for every command using either the `--help` / `-h` flag, for usage information, or use the `geneos help` command to get more in-depth documentation, for example `geneos help tls new`.

## Geneos Components and Instances

The `geneos` program works with a specific directory structure for the Geneos installation. In this guide we assume that the installation is already complete and you should not need to do anything special to use the `geneos` program. For more details on how to install the initial environment see the [Installation Guide](<./01 - installation.md>)

Geneos, the software product, is made up of a set of components. Each of these performs a different function in the Geneos architecture.

The `geneos` program distinguishes between a component type, referred to as `TYPE` in command documentation and examples, and an named instance of a component, referred to as `NAME` in documentation and examples.

Each `geneos` command can only operate on either all component types (the default) or exactly one specific type, and either all instances or a selection. This means that if you see an example in thie guide or the program documentation like `geneos ls [TYPE] [NAME...]` then this means you can refer to an optional component type and also an optional list of instance names.

Most commands will accept more than one instance name or a wildcard pattern, but because these are not file names you should wrap any patterns in quotes to stop your shell from trying to expand them, so instead of `geneos ls Example*` you should use `geneos ls "Example*"` - single or double quotes are generally interchangeable at this point but those with more shell experience will know this is not always the case.

If you don't supply a `TYPE` or at least one `NAME` then, in general, commands will work across all components or instances, respectively.

