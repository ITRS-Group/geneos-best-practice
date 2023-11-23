# Geneos Operations and Maintenance

In this guide we are going to explore how to use the `geneos` command to manage your ITRS Geneos environment. 

## Getting Started

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
geneos version v1.11.0
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

The `geneos` program works with a specific directory structure for the Geneos installation. In this guide we assume that the installation is already complete and you should not need to do anything special to use the `geneos` program. For more details on how to install the initial environment see the [Installation Guide](./01%20-%20installation-guide.md)

Geneos, the software product, is made up of a set of components. Each of these performs a different function in the Geneos architecture.

The `geneos` program distinguishes between a component type, referred to as `TYPE` in command documentation and examples, and an named instance of a component, referred to as `NAME` in documentation and examples.

Each `geneos` command can only operate on either all component types (the default) or exactly one specific type, and either all instances or a selection. This means that if you see an example in thie guide or the program documentation like `geneos ls [TYPE] [NAME...]` then this means you can refer to an optional component type and also an optional list of instance names.

Most commands will accept more than one instance name or a wildcard pattern, but because these are not file names you should wrap any patterns in quotes to stop your shell from trying to expand them, so instead of `geneos ls Example*` you should use `geneos ls "Example*"` - single or double quotes are generally interchangeable at this point but those with more shell experience will know this is not always the case.

If you don't supply a `TYPE` or at least one `NAME` then, in general, commands will work across all components or instances, respectively.

## Basic Commands

Let's start with some basic commands.

### `geneos list` / `geneos ls`

To see which Geneos instances exist, including information about what component type they are and the version of release installed, use the `list` command (or the simpler, for UNIX/Linux users, alias `ls`):

```bash
$ geneos ls
Type      Name                 Host       Flags Port  Version                     Home
gateway   test1                localhost  A     7038  active_prod:6.5.0           /opt/geneos/gateway/gateways/test1
licd      perm                 localhost  PA    7041  active_prod:6.5.0           /opt/geneos/licd/licds/perm
netprobe  localhost            localhost  PA    7036  active_prod:6.5.0           /opt/geneos/netprobe/netprobes/localhost
san       hdci-ecr1adh01a      localhost  A     7103  netprobe/active_prod:6.5.0  /opt/geneos/netprobe/sans/hdci-ecr1adh01a
```

Here you can see columns for:

* `Type` - The component type
* `Name` - The instance name
* `Host` - The host the instance is configured on
* `Flags` - Flags that show if the instance is `P`rotected, `A`uto-start, `D`isabled
* `Port` - The TCP port the instance is configured to listen on
* `Version` - The component package type, base name and underlying version. For the `san` type the `netprobe/` prefix tells you that the underlying release is a normal Netprobe
* `Home` - The working (run time) directory

### `geneos status` / `geneos ps`

To see which Geneos instances are running use the `status` command (or, again, the simpler alias `ps`):

```bash
$ geneos ps
Type      Name                 Host       PID      Ports        User   Group  Starttime             Version                     Home
gateway   test1                localhost  1017     [7038]       peter  peter  2023-11-15T11:31:06Z  active_prod:6.5.0           /opt/geneos/gateway/gateways/test1
licd      perm                 localhost  1014     [7041 7853]  peter  peter  2023-11-15T11:31:06Z  active_prod:6.5.0           /opt/geneos/licd/licds/perm
netprobe  localhost            localhost  1016     [7036]       peter  peter  2023-11-15T11:31:06Z  active_prod:6.5.0           /opt/geneos/netprobe/netprobes/localhost
san       hdci-ecr1adh01a      localhost  1028     []           peter  peter  2023-11-15T11:31:06Z  netprobe/active_prod:6.5.0  /opt/geneos/netprobe/sans/hdci-ecr1adh01a
```

The output looks similar to the `list` / `ls` command but with some important differences. Notably the `Ports` column contains the actual TCP ports the running process is listening on and these might not be the same as those that may be configured - knowing this can be important in some situations.

The first three - `Type`, `Name` and `Host` - and last two columns - `Version` and `Home` have the same meaning, but then we also have:

* `PID` - The PID (Process ID) of the running process
* `Ports` - The actual TCP ports the process is listening on
* `User` and `Group` - The User and Group of the user running the process
* `Starttime` - The process start time

Remember that you can also limit the results by giving additional arguments on the command line, for example:

```bash
geneos ps gateway
geneos ps "text*"
```

### `geneos start`, `geneos stop` and `geneos restart`

We can control each Geneos instance using the three sub-command `geneos start`, `geneos stop` and `geneos restart`. Each command does what the name suggests.

> ⚠ It's very important to now recall that if you do not specify a `TYPE` or a list of instance `NAMEs` then most commands will operate on all matching instances. This is especially important with these three commands and you can unintentionally impact Geneos components that you didn't expect.

While there are a variety of options to these commands, it is probably worth mentioning a couple:

* The `--log` / `-l` flag to `geneos start` and `geneos restart` will start watching the log files of all the instances that have been started and will continue to do so until you interrupt it using CTRL-C (which will not affect the running instances).

* The `--all` / `-a` flag to `geneos restart` tells the command to start all matching instances, even if they were already stopped. This is useful when you have a set of instances, e.g. all netprobes, that you need to start but also stop any running instances first.

* The `--force` / `-F` flag tells the command to override protection labels - see below.

### `geneos protect`

To help avoid unintentional actions affecting more of your instances than you intended, you can label any of them as _protected_ with the `geneos protect` command. Any instance that is protected will not be affected by commands with side-effects, such as the ones above. Instead you have to explicitly force actions using the `--force` or `-F` flags for these instances.

Protecting an instance also prevents unintentional deletion and other similar actions.

As you may recall from the `geneos ls` command, the `Flags` column will show a `P` for each protected instance.

> ⚠ There is no `geneos unprotect` command and this is intentional. Instead there is an `--unprotect` or `-U` to this command to reverse it's affects.

### `geneos disable` and `geneos enable`

Another way to control how instances behave is through the `geneos disable` and `geneos enable` commands. The principal difference between `geneos protect` and `geneos disable` is that if you disable an instance then you cannot override this state per command with `--force`, unlike a protected instance.

Disabling an instance is useful when you want to perform maintenance or you want to create a manual backup copy of an instance and disable it as a placeholder.

Disabled instances show in the `geneos list` output with a `D` flag.

### `geneos logs`

Every Geneos component creates log files. You can view, search or track these logs using the `geneos logs` command. You can view logs for multiple instances and across multiple hosts, in any combination.

Like other commands, you can run `geneos logs` with no arguments and you will see the logs for all instances. By default you will be shown the last 10 lines per instance log file, with a heading telling you where the log lines are from. You can see less or more by using the `--lines N` or `-n N` flag, where N is the number of lines.

If you use the `--follow` or `-f` flag then the command will follow all the matching instances and output new lines until you interrupt the command using CTRL-C. This flag is just like the UNIX / Linux `tail` utility.

It's also possible to see the whole log file with the `--cat` or `-c` flag, to "grep" (search for matching lines) with the `--match STRING` or `-g STRING` flag, or the opposite and ignore lines with `--ignore STRING` or `-v STRING` flag.

## Managing Installed Software

The `geneos` program features a range of commands to help manage the installed Geneos releases for each component type. These are all found in the `geneos package` sub-system.

As you have seen, each Geneos instance is of a specific component type. Each of these components is associated with a software release available on the ITRS Downloads site. To allow you to manage which version is used for which instance we use the concept of a symbolic "base version". In most cases you will see this listed as `active_prod`.

> ⚠ Note that installed packages are only related to their component types and not specific instances. You manage packages per component and the symbolic base version. Each instance then uses a specific base version, and commonly all share the same one, per component. All of the commands below only work with components and symbolic base versions, not instances.

### `geneos package list` / `geneos package ls`

You can see what packages are installed using the `geneos package list` command. You can also limit this to a specific TYPE, which is especially useful if you have many installed versions.

```bash
$ geneos package ls gateway
Component  Host       Version         Links        LastModified          Path
gateway    localhost  6.5.0 (latest)  active_prod  2023-09-05T10:50:10Z  /opt/geneos/packages/gateway/6.5.0
gateway    localhost  6.4.0                        2023-09-01T08:08:15Z  /opt/geneos/packages/gateway/6.4.0
gateway    localhost  5.14.3          current      2023-09-05T11:02:15Z  /opt/geneos/packages/gateway/5.14.3
```

In the output above you will see the following columns:

* `Component` - The component type.
* `Host` - The host for this installed release.
* `Version` - The underlying version, based on the directory name
* `Links` - The symbolic base version name(s) linked to this release. Note the default `active_prod` but also the `current` base names.
* `LastModified` - The modification time of the top-level directory for the release. This is a fair indication of when it was installed.
* `Path` - The installation path

As with many other informational commands provided by `geneos`, you can also choose to have this information in CSV or JSON formats, for further processing. Use the `--help` or `-h` flag to any command to see the options available.

### `geneos package install`

You can download and install Geneos releases directly from the command line, without the need to use a web browser and then copy files between systems, as long as you can reach the download site from the server you are working on. You may need to configure details for you corporate web proxy - you can read more about this in the `geneos` documentation either by running `geneos help package install` or going to GitHub at <https://github.com/ITRS-Group/cordial/blob/main/tools/geneos/docs/geneos_package_install.md>

To download ITRS software you must have a registered account and use these to access the releases. `geneos package install` can accept your user name with the `--username EMAIL` / `-u EMAIL` flag and will then prompt you for your password, or you can use the `geneos login` command to securely save your credentials in an encrypted file.

With no other arguments the command will download and install the latest available version of all supported components. This is normally too many, so you should specify the component type on the command line:

```bash
$ geneos package install netprobe -u email@example.com
Password:
geneos-netprobe-6.6.0-linux-x64.tar.gz 100% |███████████████████████████████████████████████████| (394/394 MB, 27 MB/s)         
installed "geneos-netprobe-6.6.0-linux-x64.tar.gz" to "/home/peter/geneos/packages/netprobe/6.6.0"
```

The command has quite a few options, which you can see in the main documentation. To mention common flags, the `--local` / `-L` lets you install from a local directory or files, for when you cannot connect directly to the Internet from your server. You can also specify a URL, if you have releases located on internal web site.

Conversely, the `--download` / `-D` flag tells the command to only download the release but not to install it. You will then be able to find the release in the `${GENEOS}/packages/downloads` directory.

The `--update` / `-U` flag also lets you trigger an update of the symbolic base version, which will also stop instances and restart them as needed, unless they are protected. See below for more.

### `geneos package update`

The `geneos package update` command lets you control when instances are updated, which installed releases to use and more.

> ⚠ Remember that all of the `geneos package` commands work on component types and symbolic versions and not on individual instances.



### `geneos package uninstall`

## Manage Instances

You can add new instances with the `geneos add` command, delete with the `geneos delete` command and more.

### `geneos add`

### `geneos delete`

### `geneos deploy`



## Diagnostics

### `geneos show` and `geneos command`


