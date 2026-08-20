# Geneos Good Practices

## Introduction

Geneos is ...

* Good Practice
* Style Guide
* Examples

In these guides we show you how we recommend you install, configure, operate and maintain Geneos. These guides have been written by the ITRS Professional Services team who have, collectively, many decades of experience with Geneos.

While it is not critical that you have Geneos experience, it would be better if you take some time to familiarise yourself with the architecture of the software. To find out more about Geneos as an enterprise monitoring platform, please take a look around the online documentation, starting with [Geneos Architecture](https://docs.itrsgroup.com/docs/geneos/current/getting-started/architecture/index.html).

We have additional tools to make your experience easier; The main one we will be using is a command line program called, fittingly, `geneos` (note how we display is in fixed-width `code-like` text, to distinguish it from the actual Geneos software) which is part of the [`cordial`](https://github.com/ITRS-Group/cordial) tools, published on github. While everything we show you can be done using traditional system commands, using `geneos` will make things much quicker, simpler and more consistent.

## Prerequisites

### Permissions and Access

You will need the appropriate level of privileges, or be able call on the help of colleagues in the appropriate teams to assist, including:

* Registered for an ITRS web account to download software
* Internet access to download software either directly to your systems or via an intermediate host, like your desktop, and can copy them to each server
* Command line access to the Linux system where Geneos Gateways will be installed
* Permissions to create new directories and files
* Can ensure selected TCP ports are open for access on servers where Geneos components are installed
* Administrative access to any Windows systems where Netprobes are needed, including rights to install software

The various Geneos components all have their own prerequisites and these are listed in the respective technical reference guides.

### Download `geneos`

Download the latest release of the `geneos` utility from github and install it in a suitable directory as an executable. The latest standalone Linux binary can always be downloaded from <https://github.com/ITRS-Group/cordial/releases/latest/download/geneos>:

```bash
mkdir ${HOME}/bin && cd ${HOME}/bin
curl -OL https://github.com/ITRS-Group/cordial/releases/latest/download/geneos
chmod +x geneos
```

>[!NOTE]
>
> * You can instead place the binary in a system directory such as `/usr/local/bin`, if you have administrator privileges.
> * You may also need to re-read your shell dot files if the destination directory did not exist when you logged in.
> * If you download in a location that already contains a *directory* called `geneos` - such as an existing installation - then the `curl` command will fail.
> * On some Linux distributions you may need to replace `curl -OL` with `wget` to do the same thing.
> * Remember, this is **NOT** the Geneos product release but a tool developed by ITRS Professional Services to help you manage Geneos.

You can run `geneos` at the command line and you should see the standard help text, like below. If you don't see this then please check the downloaded file, it's permissions and if the directory you downloaded to is in your execution `PATH`.

```bash
$ geneos

  The  geneos  program will help you manage your Geneos environment.

  ...

  Use "geneos [command] --help" for more information about a command.
```

